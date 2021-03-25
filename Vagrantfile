Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
  config.vm.synced_folder ".", "/var/www/html"  
config.vm.provider "virtualbox" do |vb|
  vb.memory = "512"  
end
config.vm.provision "shell", inline: <<-SHELL
	#VM Update
	sudo apt-get update
	#Apache Webserver installieren
	sudo apt-get -y install apache2
	#Firewall "enablen"
	sudo ufw --force enable
	#Port 22 und 2222 erlaunben/freischalten
	sudo ufw allow 22
	sudo ufw allow 2222
	#SSH-Service starten
	sudo systemctl start ssh
	#Im sshd_config FIle zwei "Statments" mit Ja austauschen, damit keine Fehlermeldung kommnt
	sudo sed -i "s/.*PasswordAuthentication.*/PasswordAuthentication yes/g" /etc/ssh/sshd_config
	sudo sed -i "s/.*ChallengeResponseAuthentication.*/ChallengeResponseAuthentication yes/g" /etc/ssh/sshd_config
	#Owner von edm Verzeichis /var/mail an den User vagrant geben
	sudo chown -c vagrant /var/mail
	#Nur noch den Besitzer (in diesem Fall vagrant) auf das Verzeichnis erlauben
	sudo chmod -R 700 /var/mail
	#Nginx --> Reverse-proxy installieren
	sudo apt-get -y install nginx
	#Virtual Host deaktivieren
	sudo unlink /etc/nginx/sites-enabled/default
	#File für Reverse-proxy erstellen
	sudo touch /etc/nginx/sites-available/reverse-proxy.conf
	#Inhalt in File einfügen
	cat <<%EOF% | sudo tee -a /etc/nginx/sites-available/reverse-proxy.conf
	server {
		listen 8080;
		location / {
			proxy_pass http://127.0.0.1;
		}
	}
%EOF%
	sudo ln -s /etc/nginx/sites-available/reverse-proxy.conf /etc/nginx/sites-enabled/reverse-proxy.conf
	#Apache Service stoppen
	sudo systemctl stop apache2
	#Nginx Service starten
	sudo systemctl restart nginx
	#VM rebooten
	sudo reboot
SHELL
end