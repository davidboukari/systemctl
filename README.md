# systemctl

## systemctl
```
systemctl list-units --type=service
systemctl status sshd
systemctl enable sshd 
systemctl enable sshd --now


systemctl is-enabled vault
enabled

systemctl list-units --type=service
UNIT                               LOAD   ACTIVE SUB     DESCRIPTION
auditd.service                     loaded active running Security Auditing Service
containerd.service                 loaded active running containerd container runtime
crond.service                      loaded active running Command Scheduler
dbus.service                       loaded active running D-Bus System Message Bus
docker.service                     loaded active running Docker Application Container Engine
firewalld.service                  loaded active running firewalld - dynamic firewall daemon
getty@tty1.service                 loaded active running Getty on tty1
irqbalance.service
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

## Create a service

* https://doc.ubuntu-fr.org/creer_un_service_avec_systemd
* http://perso.univ-lemans.fr/~emicoul/Documentations/Linux/outils_syst/Nouveau%20service%20systemd.html
```
 Créer un service systemd pour démarrage au boot

1 - Éditer le fichier de service :
export SERVICE=openvpn

tee /etc/systemd/system/${SERVICE}.service<<EOF
[Unit]
Description=${SERVICE} service
#After=network-online.target

[Service]
Type=forking
Environment="CONF_DIR=/etc/${SERVICE}"
WorkingDirectory=/etc/${SERVICE}
RemainAfterExit=yes
#ExecStart=/usr/bin/sudo -b /usr/sbin/${SERVICE} server.conf
ExecStart=/usr/sbin/${SERVICE} --config server.conf --daemon

[Install]
WantedBy=multi-user.target
EOF

2 - [Optionnel] Créer le lien vers systemd :
ln -s /lib/systemd/system/${SERVICE}.service /etc/systemd/system/multi-user.target.wants/

3 - Recharger le démon systemctl :
systemctl daemon-reload

4 - Activer le nouveau service :
systemctl enable ${SERVICE}.service 
systemctl start  ${SERVICE}.service 
```

## Service activated
{code}
ls -lahtr /etc/systemd/system/multi-user.target.wants/
total 8.0K
lrwxrwxrwx  1 root root   44 Jan 31 21:10 systemd-networkd.service -> /lib/systemd/system/systemd-networkd.service
lrwxrwxrwx  1 root root   44 Jan 31 22:26 systemd-resolved.service -> /lib/systemd/system/systemd-resolved.service
lrwxrwxrwx  1 root root   36 Jan 31 22:26 remote-fs.target -> /lib/systemd/system/remote-fs.target
lrwxrwxrwx  1 root root   36 Jan 31 22:26 ondemand.service -> /lib/systemd/system/ondemand.service
lrwxrwxrwx  1 root root   32 Jan 31 22:26 cron.service -> /lib/systemd/system/cron.service
lrwxrwxrwx  1 root root   33 Jan 31 22:26 dmesg.service -> /lib/systemd/system/dmesg.service
lrwxrwxrwx  1 root root   35 Jan 31 22:26 rsyslog.service -> /lib/systemd/system/rsyslog.service
lrwxrwxrwx  1 root root   47 Jan 31 22:27 networkd-dispatcher.service -> /lib/systemd/system/networkd-dispatcher.service
lrwxrwxrwx  1 root root   41 Jan 31 22:27 console-setup.service -> /lib/systemd/system/console-setup.service
lrwxrwxrwx  1 root root   42 Jan 31 22:27 ua-reboot-cmds.service -> /lib/systemd/system/ua-reboot-cmds.service
lrwxrwxrwx  1 root root   41 Jan 31 22:27 ua-license-check.path -> /lib/systemd/system/ua-license-check.path
lrwxrwxrwx  1 root root   40 Jan 31 22:27 lxd-agent-9p.service -> /lib/systemd/system/lxd-agent-9p.service
lrwxrwxrwx  1 root root   37 Jan 31 22:27 lxd-agent.service -> /lib/systemd/system/lxd-agent.service
lrwxrwxrwx  1 root root   33 Jan 31 22:27 rsync.service -> /lib/systemd/system/rsync.service
lrwxrwxrwx  1 root root   38 Jan 31 22:27 irqbalance.service -> /lib/systemd/system/irqbalance.service
lrwxrwxrwx  1 root root   31 Jan 31 22:28 atd.service -> /lib/systemd/system/atd.service
lrwxrwxrwx  1 root root   41 Jan 31 22:28 secureboot-db.service -> /lib/systemd/system/secureboot-db.service
lrwxrwxrwx  1 root root   47 Jan 31 22:28 unattended-upgrades.service -> /lib/systemd/system/unattended-upgrades.service
lrwxrwxrwx  1 root root   41 Jan 31 22:28 open-vm-tools.service -> /lib/systemd/system/open-vm-tools.service
lrwxrwxrwx  1 root root   31 Jan 31 22:28 ufw.service -> /lib/systemd/system/ufw.service
lrwxrwxrwx  1 root root   39 Jan 31 22:28 grub-common.service -> /lib/systemd/system/grub-common.service
lrwxrwxrwx  1 root root   48 Jan 31 22:28 grub-initrd-fallback.service -> /lib/systemd/system/grub-initrd-fallback.service
lrwxrwxrwx  1 root root   42 Jan 31 22:28 snapd.apparmor.service -> /lib/systemd/system/snapd.apparmor.service
lrwxrwxrwx  1 root root   44 Jan 31 22:28 snapd.autoimport.service -> /lib/systemd/system/snapd.autoimport.service
lrwxrwxrwx  1 root root   44 Jan 31 22:28 snapd.core-fixup.service -> /lib/systemd/system/snapd.core-fixup.service
lrwxrwxrwx  1 root root   58 Jan 31 22:28 snapd.recovery-chooser-trigger.service -> /lib/systemd/system/snapd.recovery-chooser-trigger.service
lrwxrwxrwx  1 root root   40 Jan 31 22:28 snapd.seeded.service -> /lib/systemd/system/snapd.seeded.service
lrwxrwxrwx  1 root root   33 Jan 31 22:28 snapd.service -> /lib/systemd/system/snapd.service
lrwxrwxrwx  1 root root   31 Jan 31 22:28 ssh.service -> /lib/systemd/system/ssh.service
lrwxrwxrwx  1 root root   37 Jan 31 22:28 pollinate.service -> /lib/systemd/system/pollinate.service
lrwxrwxrwx  1 root root   42 Jan 31 22:28 snap-core20-1328.mount -> /etc/systemd/system/snap-core20-1328.mount
lrwxrwxrwx  1 root root   40 Jan 31 22:28 snap-lxd-21835.mount -> /etc/systemd/system/snap-lxd-21835.mount
lrwxrwxrwx  1 root root   36 Feb  8 07:57 fail2ban.service -> /lib/systemd/system/fail2ban.service
lrwxrwxrwx  1 root root   38 Feb 10 07:26 conntrackd.service -> /lib/systemd/system/conntrackd.service
lrwxrwxrwx  1 root root   42 Feb 16 22:31 snap-snapd-14978.mount -> /etc/systemd/system/snap-snapd-14978.mount
lrwxrwxrwx  1 root root   42 Feb 23 23:19 snap-core20-1361.mount -> /etc/systemd/system/snap-core20-1361.mount
lrwxrwxrwx  1 root root   40 Feb 24 09:14 snap-lxd-22526.mount -> /etc/systemd/system/snap-lxd-22526.mount
drwxr-xr-x 20 root root 4.0K Feb 24 09:14 ..
lrwxrwxrwx  1 root root   45 Feb 24 09:14 snap.lxd.activate.service -> /etc/systemd/system/snap.lxd.activate.service
lrwxrwxrwx  1 root root   42 Mar  7 09:02 firewall_perso.service -> /etc/systemd/system/firewall_perso.service
{code}





