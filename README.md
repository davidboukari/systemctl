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

## Create a service
* http://perso.univ-lemans.fr/~emicoul/Documentations/Linux/outils_syst/Nouveau%20service%20systemd.html
```
 Créer un service systemd pour démarrage au boot

1 - Éditer le fichier de service :
vi /etc/systemd/system/openvpn.service
[[Unit]
Description=Openvpn service
#After=network-online.target

[Service]
Type=forking
Environment="CONF_DIR=/etc/openvpn"
WorkingDirectory=/etc/openvpn
RemainAfterExit=yes
#ExecStart=/usr/bin/sudo -b /usr/sbin/openvpn server.conf
ExecStart=/usr/sbin/openvpn --config server.conf --daemon

[Install]
WantedBy=multi-user.target

2 - [Optionnel] Créer le lien vers systemd :
ln -s /lib/systemd/system/openvpn.service /etc/systemd/system/multi-user.target.wants/

3 - Recharger le démon systemctl :
systemctl daemon-reload

4 - Activer le nouveau service :
systemctl enable openvpn.service 
systemctl start openvpn.service 
```
