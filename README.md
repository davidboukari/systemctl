# systemctl

## systemctl
```
systemctl list-units --type=service
systemctl status sshd
systemctl enble sshd 
systemctl enable sshd --now
```

## journalctl
```
journalctl
journalctl -f
journalctl -u crond
journalctl _PID=1
journalctl /usr/sbin/sshd
journalctl -p err
journalctl --since "2016-02-10 21:00:00" --until "2016-02-10 22:00:00"
journalctl --since "10 minutes ago"
```
