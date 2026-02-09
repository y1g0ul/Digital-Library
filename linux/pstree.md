---
created-dt: 2025-08-18 11:14
tags:
  - review
sr-due: 2026-04-12
sr-interval: 62
sr-ease: 250
---
Команда в [[Linux]] которая выводит дерево [[процесс]]ов со взаимосвязями(кто кого запустил)

```bash
user@serv:~$ pstree
systemd─┬─ModemManager───3*[{ModemManager}]
        ├─agetty
        ├─containerd───8*[{containerd}]
        ├─cron
        ├─dbus-daemon
        ├─dockerd───10*[{dockerd}]
        ├─fwupd───5*[{fwupd}]
        ├─gpg-agent
        ├─multipathd───6*[{multipathd}]
        ├─polkitd───3*[{polkitd}]
        ├─rsyslogd───3*[{rsyslogd}]
        ├─sshd─┬─sshd───sshd─┬─bash───pstree
        │      │             └─bash───sleep
        │      └─sshd───sshd───sftp-server
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-network
        ├─systemd-resolve
        ├─systemd-timesyn───{systemd-timesyn}
        ├─systemd-udevd
        ├─udisksd───5*[{udisksd}]
        ├─unattended-upgr───{unattended-upgr}
        └─upowerd───3*[{upowerd}]

```
