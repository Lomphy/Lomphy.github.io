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
    yourPicDir

The parameters can be adjusted and customized to your personal desires. More infos can be found in the [feh manual pages](https://linux.die.net/man/1/feh).