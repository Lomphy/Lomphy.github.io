---
layout: page
title: "Raspberry Pi Slideshow Server"
---
Date: February 05, 2022

# Background

Recently the James Webb Telescope got launched into Space and since I'm a big Space Nerd, I was really thrilled. In a [Reddit Post](https://www.reddit.com/r/space/comments/sgmc4e/i_built_a_james_webb_inspired_bit_of_wall_art/) I saw some amazing wall art mirror Design that I wanted to try for myself. When looking through the comments,  I saw some folks discussing the possibility of putting a Display in the middle Hexagon which is connected to a Raspberry Pi, that pulls the most recent Image of the Telescope and displays it. I really liked the idea and had a spare Raspberry Pi laying around, but the telescope wasn't fully deployed yet so I wanted to have my Raspberry Pi just show a slideshow of Images I put in a certain Folder. Since I don't want to remote connect to the Raspberry Pi everytime I want to change some pictures, I also set up a File Sharing Server with Samba, where I can easily replace files from any computer in my network. In this tutorial I want to explain how I set everything up.

# Photo Slideshow on Raspberry Pi Using Feh

## System Info

The instructions in this tutorial assume a fresh Install of Raspbian using [the official Raspbian imager](https://www.raspberrypi.com/documentation/computers/getting-started.html) 

## Feh Installation and Test

1. Installation of the [feh image viewer program](https://feh.finalrewind.org/). The Raspbian operating system uses the [APT package management](https://embeddedinventor.com/sudo-apt-get-install-command-explained-for-beginners/) so the installation of feh can be done with the following terminal command:

    apt-get install feh

2. Create a folder, where the to be displayed Images will be stored. Either use the desktop interface or create it from the [terminal](https://www.techwalla.com/articles/how-to-make-a-folder-in-ubuntu). In this tutorial, the path to that directory will be called "yourImgDir".

3. Add some Images to that folder, so the slideshow can be tested.

4. Type the following command in the terminal, to test if feh works correctly:

    feh \
    --recursive \
    --randomize \
    --fullscreen \
    --quiet \
    --hide-pointer \
    --slideshow-delay 6 \
    yourImgDir

The parameters can be adjusted and customized to your personal desires. More infos can be found in the [feh manual pages](https://linux.die.net/man/1/feh).

To exit, press Esc.

## Disabling Screen Blanking

To disable the automatic screen blanking, that happens after a few minutes of inactivity, the lightdm.conf file needs to be modified. Access the file by typing the following command in the terminal:

    nano /etc/lightdm/lightdm.conf

Scroll down to the [Seat:*] section and add the following line:

    xserver-command=X -s 0 dpms

Save your changes by clicking Ctrl+X -> Y -> Enter.

## Auto-Start Slideshow on Boot

1. A shell-script file needs to be created, that gets executed when the Raspberry Pi boots up. Do that by entering the following command in the terminal:

    nano /home/pi/slideshow.sh

2. Type in the feh script we already used in the beginning, with an additional line at the beginning:

    #!/bin/bash
 
    feh \
    --recursive \
    --randomize \
    --fullscreen \
    --quiet \
    --hide-pointer \
    --slideshow-delay 6 \
    yourImgDir

3. Make the created shell-script executable by typing the following command in the terminal:

    chmod +x slideshow.sh

4. You can test the script by typing the following command in the terminal:

    ./slideshow.sh

5. To make the script start on boot-up, the LXDE autostart file needs to be edited:

    nano /home/pi/.config/lxsession/LXDE-pi/autostart

Add the following line to the bootom of the file:
    @/home/pi/slideshow.sh


To test if everythin works, reboot your Raspberry Pi and it should automatically start the slideshow. To exit press Esc.
