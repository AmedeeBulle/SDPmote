#umcam
Python app that uses an internal webserver (tornado) to serve a client to control your ultimaker / other marlin based 3d printer. Can also use mjpeg-streamer to stream and record pictures (TLs). It will run on Window, Linux and on the Raspberry Pi (wheezy, jessie).
The applications name is derived from Ultimaker ([Great 3D Printer](http://www.ultimaker.com)) **um** and the word camera, **cam**.

#Credits
- Octoprint, Gina Häussge, http://octoprint.org/
- bootsrap, http://getbootstrap.com/
- mjpeg-streamer, https://github.com/jacksonliam/mjpg-streamer
- jquery, http://jquery.com/
- python 2.7, https://www.python.org/
  - pygame, http://www.pygame.org/hifi.html
  - Tornado, http://www.tornadoweb.org/en/stable/
  - pyserial ()

#Installation (Raspberry Pi)

Start with a fresh install of the latest wheezy or Jessie. Expand filesystem, install piTFT drivers, touchscreen interface etc.

#install dependencies
##pygame
```
sudo apt-get install python-pygame
```

##install tornado / pyserial if required
```
sudo apt-get install python-pip
(sudo pip install pyserial)
sudo pip install tornado
```

##make sure the user pi has access to the serial port
```
sudo usermod -a -G tty pi
sudo usermod -a -G dialout pi
```
##install mjpeg-streamer
```
cd ~
sudo apt-get install git subversion libjpeg8-dev imagemagick libav-tools cmake
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
export LD_LIBRARY_PATH=.
make
```
make sure mjpeg-streamer is running, best to run it as a deamon, look at 'raspi_stream', configure for your input device, make it executable and run it

#install umcam
```
cd ~
git clone https://github.com/MartinBienz/umcam.git
```

##run (no touchscreen):
```
python umcam.py --fullscreen --headless
```

##run (pitft 2.8 / 320x240, touchscreen):
```
python umcam.py --fullscreen --hidemouse
```
#configuration
After the application started, you can point your browser to http://<yourPisIP>:8081 and start configuring the application using the web / tornado interface.
