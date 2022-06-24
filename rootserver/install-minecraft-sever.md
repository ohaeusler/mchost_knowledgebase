##  Minecraft Paper Server (1.19) unter Ubuntu installieren
> In einem Kasten stehende Command können auf einmal kopiert und in die SSH-Shell eingefügt werden
```bash
apt update && apt upgrade -y && apt install openjdk-18-jre tmux wget tar zip sudo cron
adduser minecraft
su minecraft
```
```bash
cd ~
wget https://papermc.io/ci/job/Paper-1.19/lastStableBuild/artifact/paperclip.jar
java -Xms128M -Xmx<MaximalerRAMinMB>M -jar paperclip.jar
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
while true
do
  java -Xms128M -Xmx<MaximalerRAMinMB>M -nogui -jar paperclip.jar 
  echo 'Willst Du den Server komplett stoppen, drücke STRG+C!'
  echo "Neustart in:"
  for i in 5 4 3 2 1
  do
  echo "$i..."
  sleep 1
  done
  echo 'Server neustart!'
done
```
> Bestätigen mit der Tastenkombination **STRG** + **x**, gefolgt von **y**
```bash
chmod +x start.sh
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
nano tmux.sh && chmod +x tmux.sh
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

Du hast die Installation nun abgeschlossen.
