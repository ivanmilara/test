<div id="page">

<div id="main" class="aui-page-panel">

<div id="main-header">

<div id="breadcrumb-section">

1.  <span>[Fab Lab activities](index.html)</span>
2.  <span>[Tutorials](Tutorials_73535691.html)</span>

</div>

# <span id="title-text"> Fab Lab activities : Infrared camera with live streaming </span>

</div>

<div id="content" class="view">

<div class="page-metadata">

Created by <span class="author"> Ivan Sanchez Milara</span>, last
modified on Apr 10, 2018

</div>

<div id="main-content" class="wiki-content group">

<div class="toc-macro rbtoc1589210689727">

  - [Intro](#Infraredcamerawithlivestreaming-Intro)
  - [Components](#Infraredcamerawithlivestreaming-Components)
  - [Installing
    raspbian](#Infraredcamerawithlivestreaming-Installingraspbian)
  - [Configuring raspbian for first
    time](#Infraredcamerawithlivestreaming-Configuringraspbianforfirsttime)
      - [Configuring
        WiFi](#Infraredcamerawithlivestreaming-ConfiguringWiFi)
      - [Enabling camera and ssh. Restarting in CLI (cli
        interface)](#Infraredcamerawithlivestreaming-Enablingcameraandssh.RestartinginCLI\(cliinterface\))
  - [Checking Camera](#Infraredcamerawithlivestreaming-CheckingCamera)
  - [Creating live stream in
    Youtube](#Infraredcamerawithlivestreaming-CreatinglivestreaminYoutube)
  - [Start streaming from your
    Pi](#Infraredcamerawithlivestreaming-StartstreamingfromyourPi)
  - [Other
    improvements](#Infraredcamerawithlivestreaming-Otherimprovements)
      - [Start the application when raspberry
        boots](#Infraredcamerawithlivestreaming-Starttheapplicationwhenraspberryboots)
      - [Get IP address by email (dynamic ip
        networks)](#Infraredcamerawithlivestreaming-GetIPaddressbyemail\(dynamicipnetworks\))
      - [Communicating with your raspberry pi
        remotely ](#Infraredcamerawithlivestreaming-Communicatingwithyourraspberrypiremotely)

</div>

# Intro

In this tutorial I will explain how to stream a raspberry pi camera to a
you tube live connection. The idea is to place the camera in a self-made
birdhouse. 

The tutorial and the idea is heavily influenced by the following
tutorials: 

  - Infrared Bird
    Box: <https://projects.raspberrypi.org/en/projects/infrared-bird-box/>
  - River Cam: <http://www.instructables.com/id/River-Cam/>

# Components

  - Raspberry Pi Zero W
  - Pi Cam v 2.1
  - Raspberry Pi Zero Camera Cable
  - External display, keyboard and mouse
  - Mini HDMI cable
  - 2A power source

# Installing raspbian

1.  Download the operating system (raspbian). The latest raspian version
    from <https://www.raspberrypi.org/downloads/>. 
2.  Raspbian needs to be installed in a micro sd card. To that end, we
    are using a software named [Etcher](https://etcher.io/) that is
    available for Windows, Linux and Mac. The process is as follows: 
    1.  Open Etcher and select from your hard drive the Raspberry
        Pi `.img` or `.zip` file you wish to write to the SD card.
    2.  Select the SD card you wish to write your image to.
    3.  Review your selections and click 'Flash\!' to begin writing data
        to the SD card.
3.  Move the SD card to the raspberry pi
4.  Connect the keyboard, mouse and screen to the PI.

# Configuring raspbian for first time

## Configuring WiFi

<span style="color: rgb(34,34,34);">Wireless connections can be made via
the network icon at the right hand end of the menu
bar. </span><span style="color: rgb(34,34,34);">The icons on the right
show whether a network is secured (has a lock) or not, and give an
indication of its signal strength. If it is secure, when you press
connect it will ask you for a password. You can edit all network
settings (e.g. static ip address, and so on).</span>

However, we want to use the raspberry pi zero from the command line
instead of the Desktop UI. Hence, we need to follow a different
approach. Configuration file for modern Ubuntu based systems (such as
raspbian) is stored in /etc/wpa\_supplicant/wpa\_supplicant.conf. Open
your terminal (the black icon at the top of the screen) and type the
following command:  (leafpad is the default text editor in raspbian).

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: bash; gutter: false
sudo leafpad /etc/wpa_supplicant/wpa_supplicant.conf
```

</div>

</div>

This file is a text file in which we need to define the settings for the
network we want to connect to. To do that, add a new network
configuration at the bottom of the file. Template for this network
configuration is as follows:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: bash; gutter: false
network={
    ssid="My network"
}
```

</div>

</div>

Each line will contain a property with syntax *property\_name *=
*property\_value*. The most important properties for you are: 

  - ssid: the name of the network. E.g. "panoulu"
  - key\_mgmt: The type of security this network uses. The most utilized
    ones are WPA-PSK (protected networks) or NONE (non password
    protected network)
  - psk: if the network has a password the password should be written in
    this field.

  
Additional
info: <https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md>

TBD: How to have a ip address.

## Enabling camera and ssh. Restarting in CLI (cli interface)

  - Press the Raspberri Pi icon at the top left corner of the screen. 
  - In the menu select *Preferences \> Raspberri Pi configuration.*
  - In the System tab select *Boot *\> *To CLI. *This settings will made
    that your Raspberry PI wil start in CLI (command line interface)
    mode next time it boots. 
  - In the interfaces tab enable *Camera * and SSH . This will allow you
    to connect the camera and access the Raspberry PI remotely using SSH
    protocol.

# Checking Camera

Connecting the camera to the Raspberry Pi. Please, remember that the
Raspberry Pi Zero has a different cable that is sold separately. More
info in this video: 

![](plugins/servlet/confluence/placeholder/unknown-macro) 

In order to check that your camera is working execute the following
command from your terminal. It will show a screen presenting the content
of your camera. 

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
raspivid -t 0
```

</div>

</div>

You can close the window by pressing *CTRL-C*.

Now you can reboot the Raspberry Pi. It will start in Command line mode.

# Creating live stream in Youtube

In order to stream live video we need to use a streaming protocol. In
this case we will use RMTP. This protocol is supportted by You Tube. We
need a special URL (RMTP address) from You Tube that we can use to
direct the footage captured by the Raspberry Pi’s camera to YouTube,
thus streaming it.

To create the stream channel go to YouTube, sign in, and press
the "*Creator studio"*  that is accessible through a contextual menu
from your user image(top right corner). This will open your main channel
dashboard. Select *Live Streaming *\> *Stream now *from the menu on the
left. 

<span style="color: rgb(67,45,45);">Fill in the details you want for the
live feed (below the video screen). This will be information about the
subject of the feed, and a title, which you should add
under </span><span style="color: rgb(67,45,45);">Basic
Info</span><span style="color: rgb(67,45,45);">. Check
the </span><span style="color: rgb(67,45,45);">Stream Options
and </span><span style="color: rgb(67,45,45);">look for Encoder Setup.
Then copy the </span><span style="color: rgb(67,45,45);">Server
URL</span><span style="color: rgb(67,45,45);">and </span><span style="color: rgb(67,45,45);">Stream
name/key</span><span style="color: rgb(67,45,45);"> (you’ll need to
click </span><span style="color: rgb(67,45,45);">Reveal</span><span style="color: rgb(67,45,45);"> to
see this). Note that the Stream key needs to be kept private — anyone
with this information can stream to your YouTube channel\!</span>

More detailed description in this
link: <https://www.makeuseof.com/tag/live-stream-youtube-raspberry-pi/>

# Start streaming from your Pi

We can now start streaming. The problem is that
the `raspivid` application that is utilized to cast video from
raspberry pi does not encode the video with the right format for
streaming. We need to install `ffmpeg` library, that fortunately is
nowadays compile for the Raspberry Pi. Hence, first step is to
install `ffmpeg` library. From the command line write: 

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: bash; gutter: false
pi@raspberrypi: ~ $ sudo apt-get update
pi@raspberrypi: ~ $ sudo apt-get install ffmpeg
```

</div>

</div>

  Once the library is installed we can transform the video content from
the Raspberri Pi into a rmpt stream. We first capture the video with
the `raspivid` command. After that we reencode the video using the
`ffmpeg`. The command is as follows (it is very long): 

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: bash; gutter: false
pi@raspberrypi: ~ $ raspivid -o - -t 0 -w 1280 -h 720 -fps 25 -b 4000000 -g 50 | ffmpeg -re -ar 44100 -ac 2 -acodec pcm_s16le -ac 2 -i /dev/zero -f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -f flv rtmp://x.rtmp.youtube.com/live2/stream_name_key
```

</div>

</div>

 where `stream_name_key` is the one you obtained from You Tube in the
previous step. You can see an explanation of the different parameters in
the following
link: <https://www.digikey.com/en/maker/blogs/streaming-live-to-youtube-and-facebook-using-raspberry-pi-camera>

You can store this command in a text file (e.g. `streaming.txt`). Since
you are in command line you can use nano editor.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ nano streaming.txt
```

</div>

</div>

Once in *nano *just write the previous instruction and type
*CTRL-O  *and press *Enter *to save the file. Finally press *CTRL-X*
to exit *nano*

Once it has been stored it can be executed using:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ bash streaming.txt
```

</div>

</div>

You might need to change the permissions of the file:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo chmod 744 streaming.txt
```

</div>

</div>

# Other improvements

## Start the application when raspberry boots

There are multiple ways of running a command when your service starts
up: 

<https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/>

Nowadays the best option is creating a service using `systemd`. Some
instructions on how to do that can be found
from <https://www.raspberrypi.org/documentation/linux/usage/systemd.md>

We are creating a new service and run it at startup. 

Create the following file using nano:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ nano streaming.service
```

</div>

</div>

Add the following information. After that press *CTRL-O* to save it
as `streaming.service` and *CTRL-X* to exit.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
[Unit] 
Description=Service that streams video to Youtube
After=network.target 
 
[Service]
ExecStart=/bin/bash /home/pi/streaming.txt
WorkingDirectory=/home/pi
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi
[Install]
WantedBy=multi-user.target
```

</div>

</div>

Copy the file in `/etc/systemd/system`

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo cp streaming.service /etc/systemd/system/streaming.service
```

</div>

</div>

Next, let's test it that our service works correctly:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo systemctl start streaming service
```

</div>

</div>

We should see the image on the screen. Let's now stop it.

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo systemctl stop streaming service
```

</div>

</div>

<span style="color: rgb(34,34,34);">When you are happy that this starts
and stops your app, you can have it start automatically on reboot by
using this command:</span>

<span style="color: rgb(34,34,34);"> </span>

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo systemctl enable streaming service
```

</div>

</div>

<span style="color: rgb(34,34,34);">To check that it is working, let's
restart the system: </span>

<span style="color: rgb(34,34,34);"> </span>

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
pi@raspberrypi: ~ $ sudo shutdown -r now
```

</div>

</div>

<span style="color: rgb(34,34,34);">The system should reboot and if
everything is working normally, we should see again the video image on
the screen.</span>

## Get IP address by email (dynamic ip networks)

Sometimes we need to access remotely the raspberry pi (see next section)
to modify some settings or fix some of the files. The raspberry pi can
be accessed via `ssh` using the ip address. However, if you are in a
network with dynamic IP address you need a way to retrieve that address
in order to know where to connect. In this case, we are building
a *Python *script that will check the ip address, and if it has changed
it will send the address via email. We are taking the code from the
following site: <http://www.instructables.com/id/River-Cam/> .

This code do the following: 

  - Check the current ip address
  - It stores the value of the ip address in a text file *Address.txt*
  - Only if the current address is different than the ip address stored
    at the file *Address.txt *modifies it.
  - If there is a change in the ip address, it send an email to a
    predefined email: `yourmail1@blah.blah`  
      

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: py; gutter: false
# coding:utf-8
# Import smtplib to provide email functions
# Routine to check the external IP address and send an email if its changed.
import smtplib
import urllib
import socket
 
# Import the email modules
from email.mime.text import MIMEText
#Method to extract the ip address
def get_ip_address():
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.connect(("8.8.8.8", 80))
    return s.getsockname()[0]
# Get the IP address
ipAddress = get_ip_address()
old_address = ""
filename = "/home/pi/Address.txt"
# Now check to see if it is same as last one recorded
try:
    text_file = open(filename,"r")
    old_address = text_file.read()
    text_file.close()
except:
    print "File read failure"
if ipAddress != old_address:
    # Define email addresses to use
    addr_to   = ['myaddress@gmail.com']
    addr_from = 'mystreamingcamera@gmail.com'
 
    # Define SMTP email server details
    #smtp_server = 'smtp.gmail.com:587'
    #smtp_user   = 'your smtp login'
    #smtp_pass   = 'password'
    
    smtp_server = 'smtp.utu.fi'
    smtp_user   = None
    smtp_pass   = None
    
    # Create the body of the message (a plain-text ).
    text = "Bird cam IP address is  %s \nText and html." % ipAddress
 
    # Construct email
    msg = MIMEText(text, 'plain')
    msg['To'] = ", ".join(addr_to)
    msg['From'] = addr_from
    msg['Subject'] = 'Automated Email From Bird cam - IP address Change'
 
 
    # Send the message via an SMTP server
    s = smtplib.SMTP(smtp_server)
    #If it needs login and password 
    if smpt_user: 
        s.starttls()
        s.login(smtp_user,smtp_pass)
    s.sendmail(addr_from, addr_to, msg.as_string())
    s.quit()
    
    # Now write the new ip Address away
    try:
        text_file = open(filename, "w")
        text_file.write(ipAddress)
        text_file.close()
    except:
        print "File write failure"
```

</div>

</div>

 

 

The line you need to add to crontab is:

<div class="code panel pdl" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` theme: Confluence; brush: java; gutter: false
* * * * * python /home/ftp/storage/ipCheck.py
```

</div>

</div>

 

## Communicating with your raspberry pi remotely 

  - <https://www.digikey.com/en/maker/blogs/2018/how-to-boot-to-command-line-and-ssh-on-raspberry-pi>
  - SSH; change password

 

</div>

</div>

</div>

<div id="footer" role="contentinfo">

<div class="section footer-body">

Document generated by Confluence on May 11, 2020 18:24

<div id="footer-logo">

[Atlassian](http://www.atlassian.com/)

</div>

</div>

</div>

</div>
