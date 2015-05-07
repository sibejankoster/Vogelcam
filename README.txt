Vogelcam met USB camera, Raspberry Pi, WiFi dongle, push button set up en Node.js


1. Set up RPi
a. Download image Linux Wheezy voor Raspberry en brand dit op een 8 GB sD card (bv met Windows Diskimager)
b. Plaats de sD card in de Raspberry Pi en sluit de RPi aan op een voeding en op het network (Ethernet aansluiting)
c. Vind het IP adres van de RPi op het netwerk (bijvoorbeeld door in de loggen op de internet page van de router (meestal iets als 192.168.2.254) en bekijk de aangesloten devices lijst
d. Maak de hele sD card bereikbaar op de RPi:
i. Log in op de RPi met bijvoorbeeld Putty (Windows): start Putty, kies SSH connection en vul het IP adres van de RPi in. User = pi , password = raspberry
ii. Typ sudo raspi-config in de command line. Er verschijnt een menu
iii. Kies optie 1: expand file system, sluit het menu (doorloop het menu met de tab key). Kies NIET voor ‘Enable camera’, dit is alleen voor het RPi camera board, niet voor een USB camera.
iv. Reboot RPi, daarna moet de SSH verbinding opnieuw worden opgezet (werkt niet onmiddellijk)
e. Update Wheezy
i. sudo apt-get update 
ii. sudo apt-get upgrade
iii. Dit duurt enige tijd (halverwege moet een accoord worden gegeven)
f. Installeer motion
i. Sudo apt-get install motion
ii. Sudo nano /etc/default/motion. Zet start_motion_daemon = yes
iii. Sudo nano/etc/motion/motion.conf. Pas de volgende instellingen aan:
1. Daemon = on
2. Width = 640
3. Height = 480
4. Output_normal =off
5. Ffmjpg_bps = 0 (default = 40000)
6. Webcam_motion = on
7. Webcam optioon ppm = off
8. Webcam_localhost = off (access over network)
9. Webcam = video0 (RPi probably, check with ls /dev/video*) 
	
g. Test set up
i. Lsusb
ii. Sudo service motion start
iii. Start Firefox en kijk op port 8081 (192.168.2.5:8081, IP adres kan afwijken)



Dit werkt alleen op Firefox, andere browsers kunnen geen Mjpg verwerken.

2. Installeer Node.js / Express / Paparazzo. Dat maakt het mogelijk het format om te zetten:


1. Create a node directory under the /opt directory.  This is where we’re going to store our version of node:

sudo mkdir /opt/node
2. Go to http://nodejs.org/dist/ and find the latest distro with a linux arm version.  At the time of this writing, 11.3 was available.  The file you are looking for will be named something like node-v0.11.3-linux-arm-pi.tar.gz but it will have the words “linux” and “arm” in the name.
3. Right-click on the .gz file and copy the full path.
4. SSH into your Raspberry Pi
5. Use wget to pull the path you just copied:

sudo wget http://nodejs.org/dist/v0.11.3/node-v0.11.3-linux-arm-pi.tar.gz
6. Use tar to unstuff the .gz file you just downloaded:

tar xvzf node-v0.11.3-linux-arm-pi.tar.gz
7. Copy the contents of the unstuffed directory to the /opt/node directory we created earlier:

sudo cp -r node-v0.11.3-linux-arm-pi/* /opt/node
8. This next part is where Matt and I deviate.  I’m a fan of making things easy.  Had we been installing NodeJS on a desktop or laptop, we would be able to use node from the command line simply by typing “node [COMMAND]”.  This was how I set that up.  We’re going to create a symbolic link to both node and npm and put those links in /usr/local/bin:

9. sudo ln -s /opt/node/bin/node /usr/local/bin/node
sudo ln -s /opt/node/bin/npm /usr/local/bin/npm
10. Now restart the .bashrc as follows:

source ~/.bashrc
11. Now that .bashrc has been restarted, let’s verify that not only did our symbolic link work, but we have node and npm properly installed.  First type node -v to see the installed version of node and then type npm -v to see the installed version of npm.  You should see something like the following:

12. pi@raspberrypi ~ $ node -v
13. v0.11.3
14. pi@raspberrypi ~ $ npm -v
1.2.25
1. Installeer de module express:
2. sudo npm install -g express
3. cd /var
4. sudo express -e www
 


