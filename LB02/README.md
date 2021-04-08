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
Wichtig hier ist, dass man das Dockerfile selbst nicht im Pfad angeben muss. Nur der Ordner, in welchem sich das File befindet, muss angegeben werden. Das File muss aber zwingend "Dockerfile" heissen. 

#### Container bauen
Anhand des oben erstellten Images habe ich dann einen Container gebaut. Dies habe ich mit folgendem Command gemacht: 
```
docker run --rm -d -p 8080:80 -v /web:/var/www/html --name web aa6106ddd123
```