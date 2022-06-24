##  [DEUTSCH/GERMAN] Minecraft Paper Server (1.18.1) unter Ubuntu installieren
> In einem Kasten stehende Command können auf einmal kopiert und in die SSH-Shell eingefügt werden
```bash
apt update && apt upgrade -y && apt install openjdk-18-jre tmux wget tar zip sudo cron
adduser minecraft
su minecraft
```
```bash
cd ~
wget https://papermc.io/api/v2/projects/paper/versions/1.18.2/builds/267/downloads/paper-1.18.2-267.jar
java -Xms128M -Xmx6000M -jar paper-1.18.2-267.jar
nano eula.txt
```
Hier `eula=false` zu`eula=true` ändern.
> Bestätigen mit der Tastenkombination **STRG** + **x**, gefolgt von **y**
```bash
nano start.sh
```
Folgendes einfügen:
```bash
#/bin/bash
cd /home/minecraft
java -Xms128M -Xmx6000M -nogui -jar paper-1.18.2-267.jar 
```
> Bestätigen mit der Tastenkombination **STRG** + **x**, gefolgt von **y**
```bash
chmod 774 start.sh
tmux -u
```
```bash
/home/minecraft/start.sh
```

Weitere Commands:
> Bestätigen mit der Tastenkombination **STRG** + **b**, gefolgt von **d**

>  Konsole wieder aufrufen mit
> ```bash
> tmux a -t 0
> ```



Automatischen Start einrichten mit:
```bash
nano tmux.sh && chmod 774 tmux.sh
```
Folgendes einfügen:
```bash
echo "Starting minecraft-server"
tmux new-session -d
tmux send-keys -t 0:0 "/home/minecraft/start.sh" Enter
tmux send-keys -t 0:0 "tmux detach" Enter
```
> Bestätigen mit der Tastenkombination **STRG** + **x**, gefolgt von **y**

```bash
crontab -e
```
Folgendes einfügen:
```bash
@reboot /home/minecraft/tmux.sh
```
