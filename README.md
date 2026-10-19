# XtrackCad-flatpak

This repo contains workflow (_build-fp.yml_) to build XtrackCad flatpak from GTK3V2MAIN branch (by default) and upload to [Development/Flatpak/TestBuilds/](https://sourceforge.net/projects/xtrkcad-fork/files/Development/Flatpak/TestBuilds/) (by default). It uses ssh and so a user id is required as well as a secret private key, without a passphrase, stored in `SF_SSH_KEY` secret variable. Its public key must also be added for the user id on Sourceforge.

## ~/.config/systemd/user/gh.timer

```
[Unit]
Description=Start workflows on github every 5 minutes

[Timer]
OnBootSec=5m
OnUnitActiveSec=5m
Persistent=true

[Install]
WantedBy=timers.target
```

## ~/.config/systemd/user/gh.service

```
[Unit]
Description=Start workflows on github every 5 minutes

[Service]
Type=oneshot
ExecStart=/usr/bin/bash $HOME/bin/start_workflows.sh
WorkingDirectory=$HOME
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=default.target
```

## ~/bin/start_workflows.sh

```
#!/usr/bin/env bash

Branches=( \
    GTK3V2MAIN \
    default \
    v5.3.1 \
)

export GH_TOKEN="github_pat_*REDACTED*"

for i in $(seq 0 $(( ${#Branches[@]} - 1)) ); do
    [[ $i != 0 ]] && sleep 30
    #echo "$i: ${Branches[$i]}"
    /usr/bin/gh workflow run build-fp.yml --repo=strobelight/XtrackCad-flatpak --field version="${Branches[$i]}" --field upload_flag=true >/dev/null 2>&1
done
```

## start via systemd

```
sudo loginctl enable-linger $UID
systemctl --user daemon-reload
systemctl --user enable gh.timer
```

## check timer and service

```
journalctl --user -u gh.timer -n 10
journalctl --user -u gh.service -n 10
loginctl user-status $UID
```
