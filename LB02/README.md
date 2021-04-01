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
Shell von inaktiven Container starten: \
```
docker container start [ID des Containers]
```
Bsp.: _docker start f40e56f22f29_ \
Sobald der Container lauft: \
```
docker exec -it [Name des Containers]] /bin/bash
```
Bsp.: _docker exec -it brave\_meninsky /bin/bash_ \
