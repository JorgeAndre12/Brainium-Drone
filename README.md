# Brainium-Drone

When you start driving drones with a remote control, it's like giving a small child the detonator of a bomb, at any time it is possible for the drone to get out of control and take an eye out of someone.

Some examples of problems caused by the bad handling of drones.

"During a Christian Democratic Party campaign in September 2014, a Parrot AR drone crashed in front of German Chancellor Angela Merkel."

"What started out as a goofy holiday promotion ended terribly when a drone crashed into the face of Brooklyn Daily photographer Georgine Benvenuto"

https://www.techrepublic.com/article/12-drone-disasters-that-show-why-the-faa-hates-drones/

Personally, the first time I flew a drone, I almost cut a finger off of my friend, so I say it from my own experience.

The idea is to make a control system for the drone manipulated by gestures made with the SmartEdge Agile.

<img src="https://foto321.com/10111-thickbox_default/drone-dji-tello.jpg" width="800">

# Table of contents

* [Introduction](#introduction)
* [Materials](#materials)
* [Brainium Config](#brainium-config)
* [Raspberry Pi Configuration](#raspberry-pi-configuration)
* [Train](#train)
* [IFTTT](#ifttt)
* [Demo](#demo)
* [Comments](#comments)
* [References](#references)

## Introduction:

I'm going to create a system that can perform full control of a drone using gestures made with the SmartEdge Agile, instead of using a radio control system or an application on the cell phone.

The current solutions to the types of control over a drone are:

Radio control system: This type of systems are efficient for professional drone pilots, but when an inexperienced person wants only to use a drone to have fun, it is very likely that the drone ends up on the pavement with a broken propeller.
https://www.dummies.com/consumer-electronics/drones/how-to-fly-your-drone-with-an-rc-transmitter/

System by Application: This type of systems are common in drones manipulated by Wi-Fi, such as the Tello, however these applications are not very intuitive and in general only make the drone manipulation more clumsy.
https://play.google.com/store/apps/details?id=com.ryzerobotics.tello&hl=en_US

System of control by gestures captured by camera: this type of systems although they are already implemented by some drones, they are only capable of doing simple actions and without any ability to learn new commands.
https://www.heliguy.com/blog/dji-spark-gesture-control-tutorial/

Autonomous control systems by gps: This type of systems, despite being the most expensive and precise, they take away everything interesting to be a pilot of a drone since it literally flies itself.
https://www.droneomega.com/gps-drone-navigation-works/

My solution will generate a more intuitive and fun way to manipulate the controls of the drone, as well as doing cartwheels and so if the user wishes, plus the ability to add new gestures to the system and make it more fun and interesting to fly a drone.

<img src="https://i.ibb.co/R4JrJg3/Tello-01.png" width="800">

## Materials:

List the hardware and software you will use to build this.

Hardware: 
- SmartEdge Agile.
- RaspberryPi 3.
- Serial Module CH340.
- Laptop or Desktop PC (or another RaspberryPy 3). 

Software: 
- Brainium Portal. (https://spa.brainium.com/signup/login)
- Python (Spyder - Anaconda). (https://anaconda.org/anaconda/spyder)
- Smartphone APP Gateway. (https://play.google.com/store/apps/details?id=com.brainium.android.gateway&hl=es_MX)

Libraries:

- Paho-Mqtt. (https://pypi.org/project/paho-mqtt/)
- TelloPy. (https://pypi.org/project/tellopy/) 

For this project, the following connection scheme is considered.

<img src="https://i.ibb.co/g4SHfpc/Esquema.png" width="1000">

## Brainium Config:

We entered the motion recognition tab and created a new project.

<img src = "https://i.ibb.co/85qRq8S/Capture11.png" width = "1000">

In the project we create in our case we create the movements that we will use to control the drone.

<img src="https://i.ibb.co/mc91Hyg/Capture1.png" width="1000">

| Moves                  | Dron Action                   | 
|------------------------|-------------------------------|
| Back                   | Drone Backward                |
| Forw                   | Drone Forward                 |
| Land                   | Dron Landing                  |
| Take                   | Drone Takeoff                 |
| R                      | Drone Right Move              |
| L                      | Drone Left Move               |
| Circle L               | Drone Counter Clockwise       |
| Circle R               | Drone Clockwise               |
| Down                   | Drone Down                    |
| Up                     | Drone Up                      |

In the following video we show a compilation of the movements that were taught to the AI.

Video: Click on the image:

[![SGD](https://media.giphy.com/media/9VeuJ8sII8rYD7XViE/giphy.gif)](https://youtu.be/IpRtHPIniXw)

Sorry github does not allow embed videos.

To configure the communication by MQTT from Brainium, we must create a widget that allows to send the last registered pattern.

- We created a new widget.

<img src = "https://i.ibb.co/WP9pwFR/Capture20.png" width = "500">

- We configure as shown in the image.

<img src = "https://i.ibb.co/1L9Lbjq/Capture21.png" width = "500">

- We select the device that will record the data.

<img src = "https://i.ibb.co/DK9scnh/Capture22.png" width = "500">

- In the widget we can see the last recorded movement, this information will be sent by MQTT in the form of JSON, the code for the raspberry is already configured to filter only the registered patterns.

<img src = "https://i.ibb.co/yhp4Php/Capture23.png" width = "500">

## Raspberry Pi Configuration:

If you are noob consider setting up your raspberry with the following tutorial.
https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up

You need to have the following configuration to be able to perform Serial communication.

<img src="https://i.ibb.co/6gKKmy3/raspberry-pi-configuration.png" width="500">

Once we have the raspberry configured we will have to obtain the credentials for the connectivity with MQTT of Brainium, these credentials will be obtained in the following link.

https://spa.brainium.com/profile

<img src="https://i.ibb.co/2trk9Zb/Capture.png" width="500">

Obtain this credentials:
- External access token
- User ID

Into device tab obtain your device id: (If you have not renamed your module instead of "Dedsec" you will see the device ID)

<img src = "https://i.ibb.co/NLqMp9G/device.png" width = "500">

If you already renamed it and did not point the Device Id, you can find it by pressing the "+" button and you will see it below the name.

<img src="https://i.ibb.co/ZLQrQFX/deviceID.png" width="500">

Put the credentials inside the "RaspCode.py" code.

    mqtt_password = 'YOURTOKEN'  # copy and paste here external client id from your account
    user_id = 'YOURUSERID'  # copy and paste here your user id
    device_id = 'YOURDEVICEID'  # copy and paste here your device id
    
In this case we will obtain all the commands that we send from AI module as a command to the raspberry, in addition this command will be sent by serial communication to our code in the laptop "TelloSerial.py".

## Train:

  - We created a new workspace.
  
<img src="https://i.ibb.co/9rGSVnX/WS1.png" width="1000">

  - We give the project a name.
  
  <img src="https://i.ibb.co/cCvszMs/WS2.png" width="1000">

  - We select the device that will obtain the patterns of the machine.
  
  <img src="https://i.ibb.co/nfK3wCQ/WS3.png" width="1000">

  - In our case we connect the machine to our bridge simulator, as you can see in the following video to detect oscilation patterns.

[![SGD](https://img.interempresas.net/fotos/1843350.jpeg)](https://youtu.be/cFr_DeH4-iQ)

Sorry github does not allow embed videos.

- Once we have a large sample of patterns, we rename the patterns that are relevant to our model and we create the model.

<img src="https://i.ibb.co/qr4hqsf/Capture11.png" width="1000">

The patterns we recorded were the following, the most important for us and our notification that predictive maintenance is required is Max Vibration S:

- S1: Static State.
- Med Vibration S.
- S2: Static State.
- Max Vibration S.
- S3: Static State.

- Once the model is created, we go to the projects tab and create a new one.

<img src="https://i.ibb.co/Wyz3VrH/tab.png" width="1000">

- We assign to our project the BBBM device.

<img src="https://i.ibb.co/1Xb4kCB/bbbbbbbm.png" width="1000">

# IFTTT:

- First we need to create an account in IFTTT, fortunately Brainium has an integration with this platform for integration of services.

<img src="https://i.ibb.co/Bf9cYLg/Capture1.png" width="1000">

- We search for "Brainium" in the search bar.

<img src="https://i.ibb.co/tDLgCLW/Capture2.png" width="1000">

- We enter any of the applets and press on the author Brainium.

<img src="https://i.ibb.co/KbR8SJ8/Capture3.png" width="1000">

- We select "Connect" to make the connection with the Brainium platform.

<img src="https://i.ibb.co/SxMmKXB/Capture45.png" width="1000">

- We put our credentials to connect.

<img src = "https://i.ibb.co/LJ5R4MH/Capture55.png" width = "1000">

- We allow the access.

<img src = "https://i.ibb.co/6rg7qqx/Capture56.png" width = "1000">

- We selected the Applet "Get all Brainium alerts on your device".

<img src = "https://i.ibb.co/y42CbmL/Capture57.png" width = "1000">

- Turn ON and thats all!

<img src = "https://i.ibb.co/sJ23cFV/Capture58.png" width = "1000">

- To finish this, we need to install the IFTTT app on your device.

https://play.google.com/store/apps/details?id=com.ifttt.ifttt&hl=es_MX

## Demo:

In this video we will present the demo of our product and also we will show you the search for the best AI to do it, please enjoy it.

Video: Click on the image:

[![SGD](https://media.giphy.com/media/9VeuJ8sII8rYD7XViE/giphy.gif)](https://youtu.be/IpRtHPIniXw)

Sorry github does not allow embed videos.

* [Table of Contents](#table-of-contents)
