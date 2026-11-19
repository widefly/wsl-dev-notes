# Essential network configs

## TCP congestion control: switch from CUBIC to BBR

CUBIC (`cubic`) is the usual Ubuntu default. BBR (`bbr`) is an alternative
TCP congestion control algorithm that controls how this host sends TCP data.
The setting also applies to IPv6 TCP, despite the `net.ipv4` sysctl name.
It does not change the remote sender's algorithm or UDP traffic.

### 1. Check the running kernel

```bash
uname -r
sysctl net.ipv4.tcp_congestion_control
sysctl net.ipv4.tcp_available_congestion_control
```

The available list shows algorithms registered in the running kernel, including
loaded modules. If `bbr` is missing, load it and check again:

```bash
sudo modprobe tcp_bbr
sysctl net.ipv4.tcp_available_congestion_control
```

Proceed only when `bbr` appears. Support depends on the running kernel and its
installed modules, not just the Ubuntu release. If `modprobe` reports that the
module is missing, check for matching kernel modules or use a kernel built with
`CONFIG_TCP_CONG_BBR`. This also matters for custom and WSL kernels.

### 2. Save settings for boot

These steps assume Ubuntu uses systemd to load modules and apply sysctl settings
at boot. On WSL, check that systemd is enabled before relying on this mechanism.

Load the fair queueing scheduler now:

```bash
sudo modprobe sch_fq
```

If it is unavailable, check the kernel's modules or `CONFIG_NET_SCH_FQ` support
before proceeding. Ensure the modules are loaded before sysctl settings at boot
(modules already built into the kernel do not need loading):

```bash
printf '%s\n' tcp_bbr sch_fq | sudo tee /etc/modules-load.d/bbr.conf
```

Create the persistent sysctl configuration:

```bash
sudo tee /etc/sysctl.d/99-bbr.conf > /dev/null <<'EOF'
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
EOF
```

`fq` supports packet pacing and is a suitable companion to BBR. Modern kernels
can also use BBR with other queueing disciplines through TCP's internal pacing.

### 3. Apply and verify now

```bash
sudo sysctl --system
sysctl net.ipv4.tcp_congestion_control net.core.default_qdisc
```

Expected defaults:

```text
net.ipv4.tcp_congestion_control = bbr
net.core.default_qdisc = fq
```

`sysctl --system` reloads all sysctl configuration files. Resolve any errors or
conflicting assignments in other files (including `/etc/sysctl.conf`); later
assignments can override these values.

The TCP default takes effect without rebooting, but existing connections keep
their algorithm. Applications can select an algorithm per socket. Accepted
connections inherit their listener's algorithm, so restart a long-running server
if its new connections still use CUBIC.

While a fresh TCP transfer is running, inspect its connection:

```bash
ss -tin state established
```

Find the relevant local/remote address and port. Its TCP details should show
`bbr`. This confirms the algorithm on that socket; a sysctl value alone only
confirms the default. An empty result means there are no established TCP
connections to inspect in the current network namespace.

Check the actual interface queueing disciplines separately:

```bash
ip route show default
tc qdisc show dev eth0  # replace eth0 with the outgoing interface
```

Changing `default_qdisc` does not replace queues already attached to interfaces.
After reboot or interface recreation, expect `fq`, or `mq` with `fq` leaf queues
on a multiqueue device, unless network configuration overrides it. Virtual
devices may use `noqueue`. A different queue does not by itself mean BBR is off;
use `ss` to check the TCP algorithm.

### 4. Verify persistence after reboot

After the next reboot, repeat the sysctl, `ss`, and `tc` checks above using a new
TCP connection. If the defaults did not persist, inspect boot-time errors:

```bash
systemctl status systemd-modules-load.service systemd-sysctl.service
journalctl -b -u systemd-modules-load.service -u systemd-sysctl.service
```

References:

- [Linux TCP sysctls: available algorithms and per-connection defaults](https://docs.kernel.org/networking/ip-sysctl.html)
- [systemd sysctl.d: boot loading and configuration precedence](https://www.freedesktop.org/software/systemd/man/latest/sysctl.d.html)
- [systemd modules-load.d: persistent module loading](https://www.freedesktop.org/software/systemd/man/latest/modules-load.d.html)
- [Linux default_qdisc: multiqueue and virtual devices](https://docs.kernel.org/admin-guide/sysctl/net.html#default-qdisc)
- [Linux BBR implementation: fq and internal pacing](https://github.com/torvalds/linux/blob/master/net/ipv4/tcp_bbr.c)
- [iproute2 ss manual: TCP socket information](https://github.com/iproute2/iproute2/blob/main/man/man8/ss.8)
