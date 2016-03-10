#umcam
Python app that uses an internal webserver (tornado) to serve a client to control / monitor your ultimaker / other marlin based 3d printer. Can also use mjpeg-streamer to stream and record pictures (TLs). It will run on Window, Linux and on the Raspberry Pi (wheezy, jessie). It also has a pygame interface included that can be used as a touchscreen interface when you have a piTFT (or similar with 320x240 resolution attached).
The applications name is derived from Ultimaker ([Great 3D Printer](http://www.ultimaker.com)) **um** and the word camera, **cam**. Below you can see a screenshot of the webinterface's control section where you can control and monitor your printer and one of the pygame interface.
![alt umcam_web](/documentation/umcam_web.JPG)
![alt umcam_pygame](/documentation/umcam_pygameinterface.png)


#Credits
- [Octoprint](http://octoprint.org/)
- [bootsrap](http://getbootstrap.com/)
- [mjpeg-streamer](https://github.com/jacksonliam/mjpg-streamer)
- [jquery](http://jquery.com/)
- [python](https://www.python.org/)
- [pygame](http://www.pygame.org/hifi.html)
- [Tornado](http://www.tornadoweb.org/en/stable/)
- [pyserial](https://github.com/pyserial/pyserial)
- [crosshair](https://github.com/eschmar/crosshair)
- [bootbox.js](http://bootboxjs.com/)
- [bootstrap-switch](https://github.com/mewsoft/bootstrap-switch)

#Installation (Raspberry Pi)

Start with a fresh install of the latest wheezy or Jessie. Expand filesystem, install piTFT drivers, touchscreen interface etc.

#install dependencies
##pygame
```
sudo apt-get install python-pygame
```

##install tornado / pyserial
```
sudo apt-get install python-pip
sudo pip install pyserial
sudo pip install tornado
```

##make sure the user pi has access to the serial port
```
sudo usermod -a -G tty pi
sudo usermod -a -G dialout pi
```
If there are any issues with a non-standard baudrate, like 250000, try to patch pyserial according to this [guide](https://github.com/foosel/OctoPrint/wiki/OctoPrint-support-for-250000-baud-rate-on-Raspbian). This should fix the connection issues. As far as I know, pyserial was patched some time ago, but until now, I could not test it on a new system.

##install mjpeg-streamer
```
cd ~
sudo apt-get install git subversion libjpeg8-dev imagemagick libav-tools cmake
git clone https://github.com/jacksonliam/mjpg-streamer.git
cd mjpg-streamer/mjpg-streamer-experimental
export LD_LIBRARY_PATH=.
make
```
make sure mjpeg-streamer is running, best to run it as a deamon (or try with screen (```sudo apt-get install screen```)), look at 'raspi_stream' (in the root umcam folder, after the installation), configure for your input device, make it executable and run it.

The command I Use for the raspi cam:
```
cd mjpg-streamer/mjpg-streamer-experimental
./mjpg_streamer -i "./input_raspicam.so -fps 30 -x 1024 -y 768 -quality 90" -o "./output_http.so -w ./www"
```
For any standard USB Camera try (you can experiment with the settings, but try without first):
```
cd mjpg-streamer/mjpg-streamer-experimental
./mjpg_streamer -i "./input_uvc.so" -o "./output_http.so -w ./www"
```

If it's up and running you can point your browser to http://```YourPisIP```:8080 and check if it's streaming.

#install umcam
```
cd ~
git clone https://github.com/MartinBienz/umcam.git
```

##run (no (touch)screen = no pygame interface):
```
python umcam.py --headless
```

##run (pitft 2.8 / 320x240, touchscreen):
```
python umcam.py --fullscreen --hidemouse
```

##run (GPIO enabled (I check fo sudo), no pygame interface)
```
sudo python umcam.py --headless
```

#configuration
After the application started, you can point your browser to http://`yourPisIP`:8081 and start configuring the application using the web / tornado interface.
