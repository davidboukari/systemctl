# systemctl

## systemctl
```
systemctl list-units --type=service
systemctl status sshd
systemctl enble sshd 
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
[[Unit]
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
