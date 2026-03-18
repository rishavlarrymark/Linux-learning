# 🧠 LINUX — QUICK RECALL (LAYER 1)
> Purpose: fast memory refresh only (no explanations)

---

## Filesystem & Navigation
- `pwd` → current location
- `ls -la` → all files + permissions
- `cd` → move directory
- `/` → root
- `/etc` → configs
- `/var` → logs, runtime
- `/home` → user space
- `/tmp` → temporary

---

## File Operations
- `touch` → create empty file
- `mkdir -p` → nested dirs
- `cp` → copy
- `mv` → move/rename
- `rm` → delete
- `rm -r` → delete dir
- `file` → real file type
- `stat` → metadata

---

## Viewing & Logs
- `less` → safe file view
- `cat` → small files only
- `head` → start of file
- `tail` → end of file
- `tail -f` → live logs
- `watch` → repeat command
- `ls -lh` → file size

---

## Permissions & Ownership
- `r w x` → read write execute
- `user | group | others`
- symbolic → `u+x g-r o+w a+rwx`
- numeric → `4=r 2=w 1=x`
- common → `755 644 600`
- `chmod` → permissions
- `chown` → ownership
- `sudo` → admin

---

## Users & Groups
- `id user` → user exists?
- `groups user` → user’s groups
- `getent passwd user` → user DB entry
- `getent group group` → group exists
- `useradd -m -s /bin/bash user` → create login user
- `passwd user` → set password
- `groupadd group` → create group
- `usermod -aG group user` → add user to group
- `sudo -l` → allowed sudo actions
- `usermod -aG sudo user` → grant sudo
- `userdel -r user` → delete user + home

---

## System, CPU, Memory & Disk
- `uptime` → system runtime + load
- `top` → live CPU processes
- `htop` → visual CPU/core view
- `mpstat -P ALL 2` → per-core CPU usage
- `free -h` → memory availability
- `vmstat 2` → CPU + memory + IO wait
- `df -h` → disk usage
- `df -T` → filesystem type
- `df -i` → inode usage
- `du -sh *` → directory sizes
- `du -sh /var/*` → space hogs
- `ls -li` → inode numbers
- `mount` → mounted filesystems
- `umount` → detach filesystem

---

## Process Management & Runtime Control
- `ps aux` → list all running processes
- `ps -fp PID` → detailed information for a specific PID
- `pgrep name` → get PID by process name
- `kill PID` → graceful termination (SIGTERM)
- `kill -9 PID` → force termination (SIGKILL)
- `nice -n 10 cmd` → start process with lower priority
- `renice 5 PID` → modify priority of running process
- `ss -tulnp` → show listening ports and owning processes
- `lsof -i :PORT` → identify process using a specific port
- `lsof -p PID` → list files opened by a process
- `strace -p PID` → trace system calls of a running process
- `sleep 300 &` → run process in background
- `jobs` → list background jobs (current shell)
- `fg %1` → move background job to foreground
- `bg %1` → resume stopped job in background
- `/proc/PID` → kernel-level process information

---

## Disk & Storage Management
- `lsblk` → block devices overview
- `lsblk -f` → filesystem + UUID
- `blkid` → UUID details
- `df -h` → disk usage (human)
- `df -H` → disk usage (decimal)
- `df -T` → filesystem type
- `df -i` → inode usage
- `du -sh *` → directory size summary
- `du -sh /*` → root-level usage
- `du -sh /var/*` → log growth check
- `ncdu -x /` → interactive disk analyzer (same FS only)
- `mount` → show mounted filesystems
- `mount /dev/sdb1 /data` → mount volume
- `umount /data` → unmount volume
- `/etc/fstab` → persistent mount config
- `lsof | grep deleted` → hidden disk usage
- `tune2fs -l /dev/sdX` → reserved blocks info
- `resize2fs /dev/sdX` → resize ext filesystem
- `xfs_growfs /mount` → grow XFS filesystem
- `fsck /dev/sdX` → filesystem check

---

## File Search & Disk Debug
- `find /path -name "file"` → locate missing file
- `find / -iname "file"` → case-insensitive search
- `find /path -type f` → only files
- `find /path -type d` → only directories
- `find /var -size +100M` → large files
- `find / -size +1G` → very large files
- `find /var/log -mtime +30` → old logs
- `find /var -mtime -1` → recently modified files
- `du -sh *` → directory size summary
- `du -ah /var | sort -rh | head` → top space consumers
- `stat file` → mtime, ctime, owner, inode
- `ls -li` → inode numbers
- `lsof | grep deleted` → deleted but still open files

---

## Networking & Connectivity

- `ip a` → show interfaces + IP addresses
- `ip r` → show routing table
- `ping <ip>` → test connectivity (ICMP)
- `ping <domain>` → test DNS + connectivity
- `cat /etc/resolv.conf` → DNS configuration
- `ss -tulnp` → listening ports + processes
- `ss -tulnp | grep :PORT` → check specific port
- `lsof -i :PORT` → process using port
- `curl http://localhost:PORT` → test service locally
- `curl -I http://host` → check HTTP response headers
- `wget http://host` → test download/connectivity
- `traceroute <host>` → path of packets
- `ip route get <ip>` → route decision for destination
- `hostname -I` → get system IP quickly
- `netstat -tulnp` → (older) port check
- `tcpdump -i eth0` → capture packets
- `tcpdump port 80` → capture HTTP traffic

### Quick Debug Flow
- `ip a` → interface up?
- `ip r` → route exists?
- `ping 8.8.8.8` → network reachable?
- `ping google.com` → DNS working?
- `ss -tulnp` → port listening?
- `curl localhost:PORT` → service responding?

(add later)
