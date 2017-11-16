# QRobot (Qbot for short)
This is a development off of Lukas' great work on his robot.  Originally forked by fmacrae to add things like mapping capability, notification APIs and imporved self driving.
QRobot is a fork in which I will try to add some functionality, once I get the basics running my main interest is offloading vision recognition off to an Nvidia TK1 as well as adding voice recognition.

## Hardware 

These are the components I have used in my bot exactly:

- Raspberry PI 3
- Voltage dividers 5V to 3.3V for the echo channels for the sonars, built from 1K and 2K Ohm resistors.
- Adafruit amplifier: https://www.amazon.de/gp/product/B00KLBTQPS/ref=oh_aui_detailpage_o01_s00?ie=UTF8&psc=1
- A couple of small speakers: https://www.amazon.de/gp/product/B072LD4XPS/ref=oh_aui_detailpage_o09_s00?ie=UTF8&psc=1
- Pan Tilt module for the camera: https://www.adafruit.com/product/1967
- Get mini breadboards, lots of dupont wires and make sure to order sufficient header pins, including stackable ones for the adafruit hats. (I bought most from Adafruit or Amazon.de)
- 16GB SD Card
- A 8GB USB Pen Drive (VERY IMPORTANT to configure it as SWAP before you install Tensorflow, I strongly recommend to keep it in even afterwards) 

- Adafruit Motor Hat (for wheels): https://www.adafruit.com/product/2348

- Chassis with 4 DC motors - https://www.amazon.com/Aideepen-Robot-Chassis-Encoder-Arduino/dp/B01LZU008Y/ref=sr_1_1?ie=UTF8&qid=1510864010&sr=8-1&keywords=4WD+chassis&dpID=51-cmqNXvhL&preST=_SY300_QL70_&dpSrc=srch (I have NOT used the encoders)

- Adafruit Servo Hat (for arms): https://www.adafruit.com/product/2327 (bought directly from Adafruit it is cheaper)
- HC-SR04 sonars (I bought a set of 5): https://www.amazon.de/gp/product/B01M9CMJ9O/ref=oh_aui_detailpage_o09_s00?ie=UTF8&psc=1
- Raspberry PI compatible camera - for example: https://www.amazon.de/gp/product/B01ICNT3HC/ref=oh_aui_detailpage_o09_s00?ie=UTF8&psc=1
- DC Power Adapter to Terminal Block: https://www.amazon.de/gp/product/B00MIKWEBI/ref=oh_aui_detailpage_o05_s01?ie=UTF8&psc=1
- A set of heatsinks (the camera and raspberry heat up quite a bit : https://www.amazon.de/gp/product/B01N6POSOR/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1
-A USB Microphone: https://www.amazon.de/gp/product/B00XA01IQC/ref=oh_aui_detailpage_o09_s02?ie=UTF8&psc=1


A couple of things more: 

I used some pieces from my children Meccano to make it easier to attach stuff as well as provide some support and structure, most of the pieces were taken from this set (although any set would work I guess) : https://www.amazon.de/gp/product/B019K8J8A0/ref=oh_aui_detailpage_o09_s01?ie=UTF8&psc=1

I had to buy a few of these, use a tester before soldering, my first attempt did not work and it took me a bit to figure out why:
https://www.amazon.de/gp/product/B00OK6EXIK/ref=oh_aui_detailpage_o06_s00?ie=UTF8&psc=1

Of course I re-used a lot of things I already had, like the 3.5mm cable for Audio, my own soldering station and workbench, tweezers, etc. If you do not have a complete toolset be prepared to invest in some additional tools.
I have also used batteries I alread had, one of them had a 2.1 mm terminal and is used to power the servo and motor HATs in parallel because the raspberry cannot provide enough current for the motors (and you risk damaging the raspberry), the other one is just a USB battery pack that I use to power the raspberry itself.

I will probably look to add these later:
- for Whiskers to work you have to connect two bump sensors like this guy made http://www.instructables.com/id/Cheap%2C-Durable%2C-Very-Effective-Robot-Bump-Sensor/#ampshare=http://www.instructables.com/id/Cheap%252C-Durable%252C-Very-Effective-Robot-Bump-Sensor/ and connect via a safe circuit from your 3.3V outputs like this guy shows.  http://www.cl.cam.ac.uk/projects/raspberrypi/tutorials/robot/buttons_and_switches/ Do not miss out the resistors or you may fry your pi.

- an epaper HAT that I originally bought for a Raspberry Zero, or an LCD that I have lying around just to display some basic output about service status every few minutes (like, services are up and running, the IP of the robot, maybe temperature and battery stats would be nice).



To get started, you should be able to make the robot work without the arm, sonar and servo hat.



## Programs

- robot.py program will run commands from the commandline
- sonar.py tests sonar wired into GPIO ports
- wheels.py tests simple DC motor wheels
- arm.py tests a servo controlled robot arm
- autonomous.py implements a simple driving algorithm using the wheels and sonar
- inception_server.py runs an image classifying microservice
- Notification_Test.py tests the Twitter and Gmail integration.

## Example Robots

Here is the robot I built, still in the workbench and making some adjustments:

![Qbot](https://raw.github.com/mrqbit/Robot/Qbot1.jpg)
![Qbot2](https://raw.github.com/mrqbit/Robot/Qbot2.jpg)


## Wiring The Robot
### Sonar

If you want to use the default sonar configuation, wire like this (I have used the same wirign fmacrae used)

- Left sonar trigger GPIO pin 23 echo 24
- Center sonar trigger GPIO pin 17 echo 18
- Right sonar trigger GPIO pin 22 echo 27
- Right whisker GPIO pin 21 (not in use ATM)
- Left whisker GPIO pin 20 (not in use ATM)

You can modify the pins by making a robot.conf file.

### Wheels

You can easily change this but this is what wheels.py expects

- M1 - Front Left
- M2 - Back Left 
- M3 - Back Right
- M4 - Front Right 

### Camera Pan/Tilt

Connected the servos to the first 2 terminals in the servo HAT


## Installation

### basic setup

There are a ton of articles on how to do basic setup of a Raspberry PI - one good one is here https://www.howtoforge.com/tutorial/howto-install-raspbian-on-raspberry-pi/

You will need to turn on i2c and the camera

```
raspi-config
```

Next you will need to download i2c tools and smbus

```
sudo apt-get install i2c-tools python-smbus python3-smbus
```

Test that your hat is attached and visible with

```
i2cdetect -y 1
```

Install this code

```
sudo apt-get install git
git clone https://github.com/mrqbit/Robot.git
cd Robot
```

Install dependencies (I have not update the requirements file and I had to deal with a couple of installations manually)

```
sudo pip install -r requirements.txt
sudo apt-get install flite

```

At this point you should be able to drive your robot locally, try:

```
./robot.py forward
```

### server

To run a webserver in the background with a camera you need to setup gunicorn and nginx

#### nginx

Nginx is a lightway fast reverse proxy - we store the camera image in RAM and serve it up directly.  This was the only way I was able to get any kind of decent fps from the raspberry pi camera.  We also need to proxy to gunicorn so that the user can control the robot from a webpage.

copy the configuration file from nginx/nginx.conf to /etc/nginx/nginx.conf

```
sudo apt-get install nginx
sudo cp nginx/nginx.conf /etc/nginx/nginx.conf
```

restart nginx

```
sudo nginx -s reload
```

#### gunicorn

install gunicorn


copy configuration file from services/web.service /etc/systemd/system/web.service

```
sudo cp services/web.service /etc/systemd/system/web.service
```

start gunicorn web app service

```
sudo systemctl daemon-reload
sudo systemctl enable web
sudo systemctl start web
```

Your webservice should be started now.  You can try driving your robot with buttons or arrow keys

#### camera

In order to stream from the camera you can use RPi-cam.  It's documented at http://elinux.org/RPi-Cam-Web-Interface but you can also just run the following

```
git clone https://github.com/silvanmelchior/RPi_Cam_Web_Interface.git
cd RPi_Cam_Web_Interface
chmod u+x *.sh
./install.sh
```

Now a stream of images from the camera should be constantly updating the file at /dev/shm/mjpeg.  Nginx will serve up the image directly if you request localhost/cam.jpg.

#### tensorflow

There is a great project at https://github.com/samjabrahams/tensorflow-on-raspberry-pi that gives instructions on installing tensorflow on the Raspberry PI.  Recently it's gotten much easier, just do

Follow the instructions here
https://github.com/samjabrahams/tensorflow-on-raspberry-pi/blob/master/GUIDE.md

CHECK that you are using the latest available version that has arm support or the builds will fail.

Now create a symbolic link for the labels in your tensorflow directory to the pi_examples label_image directory

```
pi@raspberrypi:~/tensorflow $ ln -s tensorflow/contrib/pi_examples/label_image/gen/bin/label_image label_image
```


Next start a tensorflow service that loads up an inception model and does object recognition the the inception model

```
sudo cp services/inception.service /etc/systemd/system/inception.service
sudo systemctl daemon-reload
sudo systemctl enable inception
sudo systemctl start inception
```


Once everything is installed and ready you can get the robot running using:
```
sudo sh server.sh &
python inception_server.py &
```
think second one is to d/l the files needed to tmp
 
Then on localhost:
- port 9999 for inception  http://localhost:9999
- port 8000 for drive http://localhost:8000
- /cam.jpg to see what it sees  http://localhost/cam.jpg
 
I have an issue with drive as it tries to show the picture and fails as its appending ?T=1242341…
 
Not sure how to resolve and lukas has an issue open for it.
 
https://github.com/lukas/robot/issues/6
 
 

#### notification

- Update Notification_Settings.csv with your Twitter API OAuth settings, 
Siraj has a good guide on how to set it up here https://www.youtube.com/watch?v=o_OZdbCzHUA 
- Also create a Gmail API OAuth token called client_secret.json using instructions here https://developers.google.com/gmail/api/quickstart/python
- Run the Notification_Test.py which will hopefully Tweet then ask for your permission via a browser to send email.
- If you cannot do this due to running via SSH or similar then install the dependancies and run the Notification_Test.py on your desktop which creates a special json file in your home directory in a hidden subfolder called .credentials
- sftp the file to your Pi 
```
sftp pi@yourpisaddress
lcd ~/.credentials
cd /home/pi/.credentials
put gmail-python-email-send.json
```


