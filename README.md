# systemctl

## systemctl
```
systemctl list-units --type=service
systemctl status sshd
systemctl enble sshd 
systemctl enable sshd --now
```

## journalctl
https://www.golinuxcloud.com/view-logs-using-journalctl-filter-journald/
```
journalctl
# last page + desc if possible
journalctl -xe
# tail -f
journalctl -f
# -u unit
journalctl -u crond
journalctl _PID=1
journalctl /usr/sbin/sshd
journalctl -p err
journalctl -p info -u logstash
journalctl --since "2016-02-10 21:00:00" --until "2016-02-10 22:00:00"
journalctl --since "10 minutes ago"
```
