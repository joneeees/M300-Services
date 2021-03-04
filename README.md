# M300-Services
## 10 - Toolumgebung 
In diesem Kapitel habe ich folgendes gemacht:
- GitHub Account eingerichtet
  - Repository erstellt
  - SSH Key generiert und eingebunden \
    In der Git Konsole: \
    `ssh-keygen -t rsa -b 4096 -C "jonas.wiesendanger@edu.tbz.ch"`
  - Repository an lokalen Ort geklont
- Git Client installiert und konfiguriert
- VM auf VMware Workstation aufgesetzt und apache2 via synaptic installiert
- Virtual Box installiert (für Vagrant)
- Vagrant installiert
- Visual Studio Code mit Github verbunden, Extensions installiert und als Mardown Editor ausgewählt \
\
Dabei bin ich folgender [Anleitung](https://github.com/mc-b/M300/tree/master/10-Toolumgebung) gefolgt.

## 20 - Infrastruktur
In diesem Kapitel habe ich folgendes gemacht:
- Theorie gelesen
- Dokumentation im .md File angefangen \
\
Dabei bin ich folgender [Anleitung](https://github.com/mc-b/M300/tree/master/20-Infrastruktur) gefolgt. 

## Vagrant
In diesem Kapitael habe ich folgendes gemacht: 
- Vagrant VM ubuntu/xenial64 installiert \
  In CMD \
  Folgenden Pfad erstellt und dort hin navigiert: \
  `mkdir C:\temp\Vagrant\Vagrant01` \
  `cd C:\temp\Vagrant\Vagrant01` \
  `vagrant init ubuntu/xenial64` \
  `vagrant up --provider virtualbox` \
  `vagrant ssh` Dieser Command wurde in der Git Bash ausgeführt, da es im CMD und CMDer Public Key Probleme gab. 
- VM aktualisiert und Apache installiert \
  `sudo apt-get update` \
  `sudo apt-get upgrade -y` \
  `sudo apt-get install apache2 -y` \
Infos zur Virtual Machine: \
| Vagrant Box | Ubuntu/xenial64 |
| Hostname: | ubuntu-xenial |
| Network Interface: | enp0s3 |
| IP Address: | 10.0.2.15 |