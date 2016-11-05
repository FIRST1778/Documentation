**_How to set up Computer Vision Processing on the Raspberry Pi and have it Communicate Target Info back to the Roborio_**

**Install Raspbian OS (Jessie)**

First, install Linux on the Raspberry Pi (Raspbian OS):

[https://www.raspberrypi.org/documentation/installation/installing-images/](https://www.raspberrypi.org/documentation/installation/installing-images/)

Set up networking on the Raspberry Pi by adding the following lines to the bottom of the /etc/dhcpcd.conf file:

interface wlan0

static ip_address=<wireless IP address for Raspberry PI>/24

static router=<router IP address>

static domain_name_servers=8.8.8.8 8.8.4.4

interface eth0

static ip_address=10.17.78.179/24

static router=<robot radio IP address>

Make sure Raspbian is up to date, including libraries, firmware, then reboot:

sudo apt-get update

sudo apt-get upgrade

sudo rpi-update

sudo reboot

Note: when the RPi is up-to-date you will want to disable wlan0 for competition.  This is done by adding the following lines to the /etc/modprobe.d/raspi-blacklist.conf file:

*blacklist brcmfmac*

*blacklist brcmutil*

Reference:

[http://raspberrypi.stackexchange.com/questions/43720/disable-wifi-wlan0-on-pi-3](http://raspberrypi.stackexchange.com/questions/43720/disable-wifi-wlan0-on-pi-3)

**Install OpenCV & Support Libraries**

Load OpenCV support libraries on Raspbian:

sudo apt-get install build-essential git cmake pkg-config

sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev

sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev

sudo apt-get install libxvidcore-dev libx264-dev

sudo apt-get install libgtk2.0-dev

sudo apt-get install libatlas-base-dev gfortran

sudo apt-get install python2.7-dev python3-dev

Download and unzip OpenCV 2.4.13 on the Raspbian OS.

[http://opencv.org/downloads](http://opencv.org/downloads)

Switch to the OpenCV directory and build OpenCV using cmake:

a

**cmake .**

**make -j4**

**sudo make install**

**sudo ldconfig**

Reference:

[http://www.pyimagesearch.com/2015/10/26/how-to-install-opencv-3-on-raspbian-jessie/](http://www.pyimagesearch.com/2015/10/26/how-to-install-opencv-3-on-raspbian-jessie/)

**Sending target info back via Network Tables**

Detected target information to be sent from RPi to Roborio via Network Tables.  Network tables in Roborio are easy to set up via WPILib, a little more challenging to build and use NetworkTable functionality into the RPi side - you can build ntcore (the RPi network table package) with gradle or cmake.   Use git to clone the following repository on the RPi:

git clone [https://github.com/wpilibsuite/ntcore](https://github.com/wpilibsuite/ntcore)

Many recommend building and installing the ntscore library with cmake, the easier option:

**cd ntcorecmake . -DWITHOUT_JAVA=truemakesudo make install**

**sudo ldconfig**

Then when you compile your own code, you would add the "-lntcore" flag to G++.

Roborio Network Table references: [http://wpilib.screenstepslive.com/s/3120/m/7912/l/80205-writing-a-simple-networktables-program-in-c-and-java-with-a-java-client-pc-side](http://wpilib.screenstepslive.com/s/3120/m/7912/l/80205-writing-a-simple-networktables-program-in-c-and-java-with-a-java-client-pc-side)

Chief Delphi Thread:

[https://www.chiefdelphi.com/forums/showthread.php?t=145382](https://www.chiefdelphi.com/forums/showthread.php?t=145382)

**Auto starting your OpenCV console app**

To auto start an application on boot of the Raspberry Pi, first make sure xterm is installed:

sudo apt-get install xterm

Then open the following file:

/home/pi/.config/lxsession/LXDE-pi/autostart

Add the following files at the bottom of the file and save:

@xterm -hold --working-directory=/home/pi/<directory to use> -e ‘/home/pi/<bash script>

**Streaming OpenCV output to webserver**

You may want to stream OpenCV-processed output images from the RPi for viewing by the driver station.  This is possible by writing images out to MJPG files and running MJPG streamer.  By default the images will be available on https://<RPi_ip_addr>:8080

First, install necessary packages on the RPi: 

$ sudo apt-get install libjpeg8-dev imagemagick libv4l-dev

Create a symbolic link on one of the include files needed:

$ sudo ln -s /usr/include/linux/videodev2.h /usr/include/linux/videodev.h

Download and unzip mjpg-streamer in the /home/pi directory:

$ wget [http://sourceforge.net/code-snapshots/svn/m/mj/mjpg-streamer/code/mjpg-streamer-code-182.zip](http://sourceforge.net/code-snapshots/svn/m/mj/mjpg-streamer/code/mjpg-streamer-code-182.zip)

$ unzip mjpg-streamer-code-182.zip

Build mjpg-streamer:

$ cd mjpg-streamer-code-182/mjpg-streamer$ make mjpg_streamer input_file.so output_http.so

Install mjpg-streamer:

$ sudo cp mjpg_streamer /usr/local/bin$ sudo cp output_http.so input_file.so /usr/local/lib/$ sudo cp -R www /usr/local/www

Make sure you update LD_LIBRARY_PATH:

$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

In your OpenCV application source, you need to use the VideoWriter class to create and write to an output stream:

	…

	Mat outputImg;

String outFileName = "./out.mjpg";

VideoWriter outStream(outFileName,CV_FOURCC(‘M’,’J’,’P’,’G’),2, Size(frameWidth,frameHeight), true);

if (outStream.isOpened)  {

	outStream.write(outputImg);

}

else {

	printf("could not write to output stream");

	return -1;

	}

Once your app is built, run it normally.  It will be streaming to the specified mjpg file in your directory.

In a separate terminal window on the RPi, start mjpg-streamer:

mjpg_streamer -i "input_file.so -f **_<directory of your app>_**" -o "output_http.so -w /usr/local/www"

References:

[http://blog.miguelgrinberg.com/post/how-to-build-and-run-mjpg-streamer-on-the-raspberry-pi](http://blog.miguelgrinberg.com/post/how-to-build-and-run-mjpg-streamer-on-the-raspberry-pi)

[https://ariandy1.wordpress.com/2013/04/07/streaming-opencv-output-through-httpnetwork-with-mjpeg/](https://ariandy1.wordpress.com/2013/04/07/streaming-opencv-output-through-httpnetwork-with-mjpeg/)

[http://docs.opencv.org/trunk/dd/d9e/classcv_1_1VideoWriter.html#gsc.tab=0](http://docs.opencv.org/trunk/dd/d9e/classcv_1_1VideoWriter.html#gsc.tab=0)

**Access to the RPi during operation**

In case the driver station needs to see what is happening on the RPi,  there are two methods:

Basic:  SSH into the RPi using a TTY program like PuTTY

Remote into 10.17.78.179 port 22, and you will get a terminal to the RPi.  Pretty basic, no graphics.

Extra fancy:  Remote desktop into the RPi to see what the desktop is doing.

First, install xrdp on the RPi:

sudo apt-get install xrdp

Then, bring up a Remote Desktop window on the driver station, and connect.

Alternative Remote Desktop:

Install tightVNCserver to the raspberry pi:

sudo apt-get install tightvncserver

Start an instance of tightVNCserver:

tightvncserver

Use the [RealVNC Chrome App](https://chrome.google.com/webstore/detail/vnc%C2%AE-viewer-for-google-ch/iabmpiboiopbgfabjmgeedhcmjenhbla?utm_source=chrome-ntp-icon) to connect to the Raspberry Pi using its IP.

Reference:

[http://www.raspberrypiblog.com/2012/10/how-to-setup-remote-desktop-from.html](http://www.raspberrypiblog.com/2012/10/how-to-setup-remote-desktop-from.html)

**Reducing exposure of the camera**

For high light environments like competition fields, it is often recommended to reduce the light entering the camera to prevent image washout and false positives on targets.  This can be done by physically adding a polarization filter to the camera lens.  It can also be done on the RPi by commanding the camera to reduce exposure on the input.  This can be done via the Video for Linux (v4l-utils) utilities package. 

On the RPi, ensure the toolkit is installed with the following:

sudo apt-get install v4l-utils

Create the following script file:

set_exposure.sh

And add the following commands to the bottom to disable camera auto exposure:

#!/bin/sh -e

/usr/bin/v4l2-ctl -c exposure_auto=1

(setting exposure_absolute value higher will significantly limit light, darken the image)

Copy the script to the following directory

/etc/init.d/

And set the file to executable:

sudo chmod 755 /etc/init.d/set_exposure.sh

Register the script to run during boot:

sudo update-rc.d set_exposure.sh defaults

Reference:

[https://www.chiefdelphi.com/forums/showthread.php?t=145829](https://www.chiefdelphi.com/forums/showthread.php?t=145829)

[http://raspberrypi.stackexchange.com/questions/8734/execute-script-on-start-up](http://raspberrypi.stackexchange.com/questions/8734/execute-script-on-start-up)

Github location of RPi source examples for Chill Out:

RPi_Vision_2016

