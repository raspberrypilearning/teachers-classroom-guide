# Getting Starting with Raspberry Pi

You've got a Raspberry Pi, congratulations! I'm sure that your are bursting with excitement to get going to create your own projects. Then what are you waiting for? Follow all the steps below.

## Step 1: Make sure that you have everything that you need
Before you begin, check that you have all the parts that your need:

### Required items:

- A Raspberry Pi (Either a [Model B](http://www.raspberrypi.org/product/model-b/) or [Model B+](http://www.raspberrypi.org/product/model-b-plus/))

- SD Card
	- We recommend an 8GB class 4 SD card, ideally preinstalled with NOOBS. 
	- You can [buy a card with NOOBS pre-installed](http://swag.raspberrypi.org/collections/frontpage/products/noobs-8gb-sd-card), or you can download it for free from our downloads page.
	
	![](images/SD-card.png)
	
- Display and connecting cables
	- Any HDMI/DVI monitor or TV should work as a display for the Pi . 
	- For best results, use one with HDMI input, but other connections are available for older devices. 
	
	![](images/display-options.png)
	
- Keyboard and mouse
	- Any standard USB keyboard and mouse will work with your Raspberry Pi.
	
	![](images/keyboard-and-mouse.png)
	
- Power supply
	- Use a [5V micro USB power supply](http://swag.raspberrypi.org/collections/pi-kits/products/raspberry-pi-universal-power-supply) to power your Raspberry Pi. Be careful that whatever power supply you use outputs at least 5V; insufficient power will cause your Pi to behave in strange ways.
	
	![](images/powersupply.png)

You can purchase a kit from the Raspberry Pi Swag Store to help get started [here](http://swag.raspberrypi.org/collections/frontpage/products/b-raspberry-pi-starter-kit)

### Not essential but helpful to have:

- Internet connection
	- To update or download software, we recommend that you connect your Raspberry Pi to the internet either via and ethernet cable or a wifi adapter.
	- If you have a wifi adapter see our [wifi setup guide here](http://www.raspberrypi.org/documentation/configuration/wireless/README.md).
	
	![](images/internet-connections.png)

- Sound
	- Headphones, earphones or speakers with a 3.5mm jack will work with your Raspberry Pi.
	
	![](images/sound-jack.png)
		
	
## Step 2: Plugging in your Raspberry Pi
Now you have all the parts you need to get up and running, let's start plugging in!

1. Begin by slotting your SD card into the SD card slot on the Raspberry Pi, which will only fit one way. *Note that the software NOOBS should already be on this card. If it isn't then follow the [NOOBS guide here](http://www.raspberrypi.org/help/noobs-setup/).*
1. Next, plug in your USB keyboard and Mouse into the USB slots on the Raspberry Pi.
Make sure that your monitor or TV is turned on, and that you have selected the right input (e.g. HDMI 1, DVI, etc)
1. Then connect your HDMI cable from your Raspberry Pi to your monitor or TV.
1. If you intend to connect your Raspberry Pi to the internet, plug in an ethernet cable into the ethernet port next to the USB ports, otherwise skip this step.
1. When you are happy that you have plugged in all the cables and SD card required, finally plug in the micro usb power supply. This action will turn on and boot your Raspberry Pi.
1. If this is the first time your Raspberry Pi and NOOBS SD card have been used, then you will have to select an operating system and configure it. [Follow the NOOBS guide to do this](http://www.raspberrypi.org/help/noobs-setup/).

## Step 3: Logging into your Raspberry Pi
Hurrah, Raspbian has loaded, your Raspberry Pi is up and running, now what? Let's log in to find out:

1. Once your Raspberry Pi has completed the boot process, a login prompt will appear. The default login for Raspbian is username `pi` with the password `raspberry`. Note you will not see any writing appear when you type the password. This is a security feature in Linux.
1. After you have successfully logged in, you will see the command line prompt `pi@raspberrypi~$`
1. To load the graphical user interface, type `startx` and press **Enter** on your keyboard.
	
	![](images/desktop.png)
	
## What next?

If you are new to using Linux, then you may wish to learn a little about how to use commands to navigate around your Raspberry Pi, create files and run applications using simple text.

Then it's time to get learning or making using our [Raspberry Pi Learning Resources here](http://www.raspberrypi.org/resources/).

