# M300-Services LB02
## 30 - Container
### Einfache Docker Befehle
Docker Image aus Web herunterladen: 
```
docker pull [Name des Images]
```
Bsp.: _docker pull ubuntu_ \
\
Docker Container mit bestehendem Image starten:
```
docker run -it [Name des Images]
```
Bsp.: _docker run -it ubuntu_ \
\
Inaktiver Container starten: 
```
docker container start [ID des Containers]
```
Bsp.: _docker start f40e56f22f29_ \
\
Auf Container zugreifen (per Shell): 
```
docker exec -it [Name des Containers] /bin/bash
```
Bsp.: _docker exec -it brave\_meninsky /bin/bash_ 
\
Image Umbenennen: 
```
docker tag [ID des Images] [neuer Name des Images]
```

### Netzwerkplan
![Netzwerkplan](https://github.com/joneeees/M300-Services/blob/main/LB02/Images/Netzwerkplan.png)

### Beispiel mit Apache & Docker
Um einen Apache Webserver mit Docker laufen zulassen, habe ich folgende Schritte gemacht 

#### Dockerfile erstellt
Das Dockerfile habe ich lokal in einem Ordner erstellt. Der Inhalt der Dockerfiles ist von einem Lehrer der TBZ. 
```
#
#	Einfache Apache Umgebung
#
FROM ubuntu:14.04
MAINTAINER Marcel mc-b Bernet <marcel.bernet@ch-open.ch>

RUN apt-get update
RUN apt-get -q -y install apache2 

# Konfiguration Apache
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

RUN mkdir -p /var/lock/apache2 /var/run/apache2

EXPOSE 80

VOLUME /var/www/html

CMD /bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"
```
Mit folgendem Command habe ich das Imagen dann gebaut: 
```
docker build [VerzeichnisInDemSichDasDockerfileBefindet]
```
Bsp.: _docker build C:\temp\Docker\Dockerfiles\Web_ \
\
Wichtig hier ist, dass man das Dockerfile selbst nicht im Pfad angeben muss. Nur der Ordner, in welchem sich das File befindet, muss angegeben werden. Das File muss aber zwingend "Dockerfile" heissen. 

#### Container bauen
Anhand des oben erstellten Images habe ich dann einen Container gebaut. Dies habe ich mit folgendem Command gemacht: 
```
docker run --rm -d -p 8080:80 -v /web:/var/www/html --name web aa6106ddd123
```

#### index.html File
Da das Image kein index.html File mitbringt, habe ich selbst eines erstellt. In diesem HTML File habe ich den Link zu diesem Repository angegeben. 

```
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>M300</title>
</head>
<body>
<h1>LB02 GitHub Repository</h1>
<a>Das LB02 GitHub Repository ist unter</a>
<a href="https://github.com/joneeees/M300-Services/tree/main/LB02">folgendem Link</a>
<a>erreichbar</a>
</body>
</html>
```
Dieses File habe ich dann mit folgendem Command auf den Container kopiert:
```
docker cp [/SpeicherortDerDatei/index.html] [Container Name]:[Destination Path im Container]
```
Bsp.: _docker cp C:\temp\Docker\Dockerfiles\Web\index.html web:/var/www/html/_ 

#### Testfälle
Um die Funktionalität zu testen habe ich im Browser auf den Localhost zugegriffen. Dort sieht man dann das index.html File, welches ich geschrieben uf auf den Container kopiert habe. \
![Testing des "web" Containers](https://github.com/joneeees/M300-Services/blob/main/LB02/Images/web-test.png)

## 35 - Sicherheit
### Sicherheitsmassnahmen
#### Nicht den root User als Standarduser verwenden
Es ergibt Sinn, dass man sich nicht als root User einloggt, damit man kritische sudo-Befehle nicht ohne Passwort ausführen kann. Um das umzusetzen, muss man folgendes ins Dockerfile schreiben: 
```
RUN useradd -ms /bin/bash [NeuerUserName] \
USER [NeuerUserName] \
WORKDIR /home/[NeuerUserName]
```
_Der letzte Befehl ist aus sicherheitstechnischen Gründen nicht notwenid, da dieser nur die Direcotry, in welchem man am Anfang ist ändert._ \
Im Screenshot sieht man nur, dass ich mit dem neuen User eingeloggt bin und ich keinen Befehl mit "sudo" ausführen konnte ohne Passwort. \
![Einloggen mit neuem User](https://github.com/joneeees/M300-Services/blob/main/LB02/Images/newuser.png)

#### Read Only
Wenn man den Docker mit der Option read-only startet, können keine Änderungen am Dateisystem vorgenommen werden (auch mit sudo nicht):
```
docker run --read-only -d -t --name [NameDesContainer] [Image]
```
Im Bild sieht man, dass ich kein File erstellen konnte, da der Container auf "read-only" ist. \
![Readonly](https://github.com/joneeees/M300-Services/blob/main/LB02/Images/readonly.png)

#### Dockerfiles
Ein wichtiger Punkt mit welchem man sich und seine Netzwerkumgebung schützen kann ist, dass man seine DOckerfile's selber schreibt. Wenn man das macht und nicht irgendwelche Images aus dem Internet herunterladet, kann man sich sicher sein, dass diese nicht mit Malware oder sonst was verseucht sind.

### Monitoring
#### Cadvisor
Cadvisor ist ein Überwachungstool von Google. \
Mit folgendem Befehl kann man einen Container erstellen, in welchem diese Webapplikation läuft: 
```
docker run -d --name cadvisor -v /:/rootfs:ro -v /var/run:/var/run:rw -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro -p 8010:8080 google/cadvisor:latest
```
_Wichtig ist, dass man schaut, dass der Port, welcher man verwendet, noch frei ist. Deshalb habe ich hier den Port "8010" genommen._ \

##### Überprüfung der Webapp
Über 127.0.0.1:8010 konnte ich auf Cadvisor zugreifen \
![Cadvisor Webapplikation](https://github.com/joneeees/M300-Services/blob/main/LB02/Images/cadvisor.png)