# Persistent swap on Ubuntu

## Swap space for temporary memory spikes

Swap lets Linux move less-active memory pages to disk, providing headroom during
temporary allocation spikes. It reduces the risk of OOM kills but cannot
guarantee against them. Sustained memory pressure can cause severe slowdown;
service/container memory limits can still trigger OOM even with free host swap.

These steps cover Ubuntu with systemd and an ext4 filesystem. Btrfs needs a
different swap-file procedure. For Ubuntu on WSL 2, prefer the Windows-side
[`swap` setting in `.wslconfig`](https://learn.microsoft.com/en-us/windows/wsl/wsl-config#wslconfig).

### 1. Check capacity and choose a size

```bash
free -h
swapon --show
df -hT /
```

Size **total swap** to cover the expected peak memory demand of all applications
and the OS beyond RAM, plus a safety margin. Count existing disk swap toward
this target. For a development machine with unknown, occasional spikes, **8 GiB
is a reasonable starting point**, then adjust after observing the workload.
For example, 16 GiB RAM with a 20 GiB peak demand suggests 8 GiB total swap
(4 GiB shortfall plus 4 GiB margin). This is a sizing estimate, not an Ubuntu
requirement. Keep disk space available for normal system operation as well.

### 2. Create and enable a swap file

This example **adds 8 GiB** at `/swap-extra`; adjust `count` as needed
(`8192` × 1 MiB = 8 GiB). Use a new filename; do not overwrite existing swap.
Run each command only if the previous one succeeds.

```bash
sudo dd if=/dev/zero of=/swap-extra bs=1M count=8192 oflag=excl status=progress
sudo chmod 600 /swap-extra
sudo mkswap /swap-extra
sudo swapon /swap-extra
```

`oflag=excl` refuses to overwrite an existing file. Writing with `dd` avoids
sparse-file holes, which swap files cannot have. Swap is active immediately;
no reboot is required.

### 3. Enable it at every boot

Back up and edit `/etc/fstab`:

```bash
sudo cp -a /etc/fstab /etc/fstab.before-swap
sudoedit /etc/fstab
```

Add this line **once**, keeping existing entries:

```fstab
/swap-extra none swap sw 0 0
```

Reload systemd's generated units and check the configuration:

```bash
sudo systemctl daemon-reload
sudo findmnt --verify
```

Resolve any reported configuration errors before rebooting.

### 4. Verify now and after reboot

```bash
swapon --show
free -h
```

Expect `/swap-extra` with approximately `8G` in `swapon` and the increased swap
total in `free`. Repeat after the next reboot to confirm persistence. Zero swap
usage is normal when RAM is sufficient.

During a memory spike, `vmstat 1` shows swap-in/swap-out rates in the `si`/`so`
columns; inspect subsequent rows because the first summarizes activity since
boot. Sustained swapping with poor responsiveness calls for more RAM or lower
application concurrency. Leave swappiness at its existing value initially.

References:

- [Ubuntu swapon manual: swap-file requirements and activation](https://manpages.ubuntu.com/manpages/noble/man8/swapon.8.html)
- [systemd swap units: persistent swap through fstab](https://www.freedesktop.org/software/systemd/man/latest/systemd.swap.html)
- [Linux memory and swap limits for services/containers](https://docs.kernel.org/admin-guide/cgroup-v2.html)
