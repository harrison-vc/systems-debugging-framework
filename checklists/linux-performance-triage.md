# Linux Performance Triage Checklist (USE Method)

The **USE Method** (Utilization, Saturation, and Errors) is a starting point for system performance analysis.

## 1. Utilization
*Is the resource busy?*

- **CPU**:
  - `top` or `htop`: Check `%CPU` for processes and aggregate `us` (user) vs `sy` (system).
  - `mpstat -P ALL 1`: Check for per-core imbalance.
- **Memory**:
  - `free -h`: Check `available` memory (not just `free`).
  - `vmstat 1`: Check `swpd` (swap usage).
- **Disk**:
  - `iostat -xz 1`: Check `%util` for specific disks.
  - `df -h`: Check for 100% full partitions.

## 2. Saturation
*Is there more work than the resource can handle (Queuing)?*

- **CPU**:
  - `uptime`: Check Load Average (compare to number of CPU cores).
  - `vmstat 1`: Check `r` (runnable processes) vs `b` (blocked processes).
- **Memory**:
  - `vmstat 1`: Check `si` (swap in) and `so` (swap out). High swap activity indicates saturation.
- **Disk**:
  - `iostat -xz 1`: Check `avgqu-sz` (average queue size) and `await` (latency).

## 3. Errors
*Are things failing?*

- **System Logs**:
  - `dmesg -T | tail -n 50`: Check for OOM kills, disk I/O errors, or hardware faults.
  - `journalctl -p err..emerg`: Check for high-priority system errors.
- **Network Errors**:
  - `netstat -i` or `ip -s link`: Check for `RX-ERR` or `TX-ERR` (dropped packets).

## Summary Triage Command
Run this for a quick 60-second snapshot:
```bash
uptime
dmesg | tail
vmstat 1 5
mpstat -P ALL 1 5
iostat -xz 1 5
free -h
```
