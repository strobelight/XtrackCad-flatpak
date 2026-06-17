# XtrackCad-flatpak

This repo contains workflow (_build-fp.yml_) to build XtrackCad flatpak from GTK3V2MAIN branch (by default) and upload to [Development/Flatpak/TestBuilds/](https://sourceforge.net/projects/xtrkcad-fork/files/Development/Flatpak/TestBuilds/) (by default). It uses ssh and so a user id is required as well as a secret private key, without a passphrase, stored in `SF_SSH_KEY` secret variable. Its public key must also be added for the user id on Sourceforge.
