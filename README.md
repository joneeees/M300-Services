# M300-Services
## 10 - Toolumgebung 
In diesem Kapitel habe ich folgendes gemacht:
- GitHub Account eingerichtet
  - Repository erstellt
  - SSH Key generiert und eingebunden \
    In der Git Konsole: 
    ```
    ssh-keygen -t rsa -b 4096 -C "jonas.wiesendanger@edu.tbz.ch"
    ```
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
In diesem Kapitel habe ich folgendes gemacht: 
- Vagrant VM ubuntu/xenial64 installiert \
  In CMD \
  Folgenden Pfad erstellt und dort hin navigiert: 
  ```
  mkdir C:\temp\Vagrant\Vagrant01
  ``` 
  ```
  cd C:\temp\Vagrant\Vagrant01
  ``` 
  ```
  vagrant init ubuntu/xenial64
  ``` 
  ```
  vagrant up --provider virtualbox
  ``` 
  `vagrant ssh` Dieser Command wurde in der Git Bash ausgeführt, da es im CMD und CMDer Public Key Probleme gab 

### Infos zur Virtual Machine:
    Vagrant Box: Ubuntu/xenial64 
    Hostname: ubuntu-xenial 
    Network Interface: enp0s3 
    IP Address: 10.0.2.15 
    Installierte Software: - 
- Vagrant VM ubuntu/xenial64 mit Apache2 installiert \
  In CMD \
  Folgenden Pfad erstellt und dort hin navigiert: 
  ```
  mkdir C:\temp\Vagrant\Apache2
  ``` 
  ```
  cd C:\temp\Vagrant\Apache2
  ``` 
  Vagrant File im GUI erstellt \
  Der Code in [diesem](https://github.com/mc-b/M300/blob/master/vagrant/web/Vagrantfile) Vagrantfile in das lokale kopiert \
  Im Vagrantfile Port von "8080" auf "187" geändert \
  Wieder im CMD im Ordner "Apache" `vagrant up` \
  `vgrant ssh` Dieser Command wurde in der Git Bash ausgeführt, da es im CMD und CMDer Public Key Probleme gab 

### Infos zur Virtual Machine:
    Vagrant Box: Ubuntu/xenial64 
    Hostname: ubuntu-xenial 
    Network Interface: enp0s3 
    IP Address: 10.0.2.15 
    Installierte Software: Apache2 
    Zugriff auf Webapp: 127.0.0.1:187 
### Netzwerkplan:
![Netzwerkplan](https://github.com/joneeees/M300-Services/blob/main/Images/Netzwerkplan.png)

### Testing der VM:
Zugriff auf Apache über Port 187: \
![Web Zugrif über Port 187](https://github.com/joneeees/M300-Services/blob/main/Images/Apache-Port-187.png) \
\
Änderung in Index File: \
![Änderung im Index File](https://github.com/joneeees/M300-Services/blob/main/Images/Index-%C3%84nderungen.png) \
\
Port Forwarding:\
![Port Forwarding](https://github.com/joneeees/M300-Services/blob/main/Images/Port-Forwarding.png)\

## 25 - Infrastruktur-Sicherheit
In diesem Kapitel habe ich folgendes gemacht:
- Vagrantfile von Ubuntu Xenial64 als Grundlage genommen
- In Vagrantfile ufw aktiviert 
- In Vagrantfile Port 22 und 2222 (SSH) bei ufw zugelassen
- In Vagrantfile das File "/etc/ssh/sshd_config" angepasst

### Commands erklären:
```
sudo ufw --force enable
``` 
\
Dieser Command erzwingt das "enablen" von der ufw (Ubuntu Fire Wall). `--force` ist für das Erzwingen. 
```
sudo ufw allow 22
``` 
\
Dieser Command macht eine Firewallregel, welche den Port 22 (SSH) erlaubt. 
```
sudo ufw allow 2222
``` 
\
Dieser Command macht eine Firewallregel, welche den Port 2222 (SSH) erlaubt. 
```
sudo systemctl start ssh
``` 
\
Dieser Command startet den Dienst "ssh". 
```
sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
``` 
\
Dieser Command sucht im File "/etc/ssh/sshd_config" nach ".*PasswordAuthentication.*" und ersetzt dies durch "PasswordAuthentication yes". 
- "/g" steht für global. Das heisst es ersetzt den Gesuchten Teil überall im genannten File und nicht nur an einem Ort 
```
sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
``` 
\
Dieser Command sucht im File "/etc/ssh/sshd_config" nach ".*ChallengeResponseAuthentication.*" und ersetzt dies durch "ChallengeResponseAuthentication yes". 
- "/g" steht für global. Das heisst es ersetzt den Gesuchten Teil überall im genannten File und nicht nur an einem Ort 
```
sudo chown -c vagrant /var/mail
``` 
\
Dieser Command gibt die Ownerrechte vom edm Verzeichis /var/mail an den User vagrant.
```
sudo chmod -R 700 /var/mail
``` 
\
Dieser Command setzt die Rechte auf 700 --> User hat Read/Write/Execute. Group und Global hat keine Berechtigungen.
```
sudo apt-get -y install nginx
``` 
\
Dieser Command installiert nginx.
```
sudo unlink /etc/nginx/sites-enabled/default
``` 
\
Dieser Command deaktiviert den Virtual Host.
```
sudo touch /etc/nginx/sites-available/reverse-proxy.conf
``` 
\
Dieser Command erstellt das File für Reverse-proxy.
```
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
		listen 8080;
		location / {
			proxy_pass http://127.0.0.1;
		}
	}
%EOF%
``` 
\
Dieser Abschnitt fügt den Inhalt im File "/etc/nginx/sites-available/reverse-proxy.conf" hinzu. Der Inhalt sagt, dass der Reverse Proxy über Port 8080 für den Localhost (127.0.0.1) kommuniziert.
``` 
sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
``` 
\
Dieser Command erstellt einen Symlink von "/etc/nginx/sites-available/reverse-proxy.conf" nach "/etc/nginx/sites-enabled/reverse-proxy.conf".
``` 
sudo systemctl stop apache2
``` 
\
Dieser Command stopt den Dienst "apache2".
``` 
sudo systemctl restart nginx
``` 
\
Dieser Command startet den Dienst "nginx" neu. 
``` 
sudo reboot
``` 
\
Dieser Command startet die VM neu.  
### Überprüfung:
Mit `vagrant up` habe ich die VM erstellt \
![vagrant up](https://github.com/joneeees/M300-Services/blob/main/Images/vagrant-up.png)\
\
Mit `vagrant ssh` habe ich mich mit SSH auf die VM verbunden\
![vagrant ssh](https://github.com/joneeees/M300-Services/blob/main/Images/vagrant-ssh.png)\
Nachdem ich mich verbindet habe, habe ich geprüft, ob die ufw läuft \
Dies habe ich mit  `systemctl status ufw` gemacht \
![systemctl status ufw](https://github.com/joneeees/M300-Services/blob/main/Images/ufw-status.png)\
Nun habe ich geprüft, ob ich mit dem User "vagrant" auf die Location zugriff habe und mit dem User "ubuntu" nicht \
Dies habe ich mit  `cd /var/mail/` gemacht \
![/var/mail/ Zugriffsberechtigungen mit User "vagrant" und "ubuntu"](https://github.com/joneeees/M300-Services/blob/main/Images/var-mail-permission.png)\
Zum Schluss habe ich getestet, ob der Reverse-Proxy funktioniert \
Dies habe ich mit `` gemacht \
![Reverse-Proxy testing](https://github.com/joneeees/M300-Services/blob/main/Images/reverse-proxy.png)