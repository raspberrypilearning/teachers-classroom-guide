# Remote desktop to a Raspberry Pi through your Web Browser 

This guide is a slight variant on using remote desktop, such as VNC to connect to a Raspberry Pi. 

The basic premise is to use another computer as the input console for the Raspberry Pi without needing to get a separate screen, keyboard and mouse on the Pi. Ideal if you've got a room full of laptops, chrome books or iMacs for example.

This way differs from the previous in that you don't need to install any software on the host computer/laptop. You can set the Raspberry Pi up so that it provides everything the host computer needs through the web browser. It makes it as easy as typing an address in.

Here is an example of the end product on Windows / IE:

![](images/vnc_ie.png)

### What will it work on?
This system will work on any HTML5 compliant web browser in particular these browser vesions or later:

- Chrome 8
- Firefox 4
- Safari 5
- iOS Safari 4.2
- Opera 11
- IE 9

## Step 1: Setting up the server on a Raspberry Pi

1. Start with a fresh install of Raspbian and log in as the default `pi` user. You can use the command line interface for this guide.
1. Install a the following packages (TightVNC server and screen). Run these commands from the terminal:

	```
	sudo apt-get update
	sudo apt-get install tightvncserver screen -y
	```

1. Next run TightVNC server which will prompt you to enter a password and an optional view-only password by typing `tightvncserver` and pressing **enter** on the keyboard.

1. Now let's get the HTML5 VNC client. Enter the following commands and press **enter** at the end of each line:

	```
	cd /usr/local/share/
	sudo git clone git://github.com/kanaka/noVNC
	```

1. We just need to make a small adjustment to some of the files here. The folder we've just downloaded will be served out as a http root for the Pi so we just need to ensure there is an index page. This will allow the host computer to access the VNC client software.
	
	```
	cd noVNC
	sudo cp vnc_auto.html index.html
	```

1. Next we need to set everything to start automatically since you're probably going to want to use the Raspberry Pi in headless mode. To do this we just need to create a few scripts inside init.d, enter the following commands:

	```
	cd /etc/init.d/
	sudo nano vncboot
	```

1. You will now be editing a blank file. Copy and paste or type the following:

	```
	## BEGIN INIT INFO
	# Provides: vncboot
	# Required-Start: $remote_fs $syslog
	# Required-Stop: $remote_fs $syslog
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Start VNC Server at boot 	time
	# Description: Start VNC Server at boot time.
	### END INIT INFO

	#!/bin/sh
	#/etc/init.d/vncboot

	export USER='pi'

	eval cd ~$USER

	# Check the state of the command: this will either be start or stop
	case "$1" in
  		start)
    		# if it's start, then start vncserver using the details below
    		echo "Starting VNC for $USER..."
    		su $USER -c '/usr/bin/tightvncserver :1 -geometry 1280x800'
    		echo "...done (VNC)"
    		;;
  		stop)
    		# if it's stop, then just kill the process
    		echo "Stopping VNC for $USER "
    		su $USER -c '/usr/bin/tightvncserver -kill :1'
    		echo "...done (VNC)"
    		;;
  		*)
    		echo "Usage: /etc/init.d/vncserver {start|stop}"
    		exit 1
    		;;
	esac
	exit 0
	```

	*Note the line that says `-geometry 1280x800`. This sets the screen resolution for the remote desktop, so you may wish to alter this to suit the screen size of the host computer. Ideally this should be set slightly lower to avoid having scroll bars.*

1. Press **Ctrl** and **O** followed by **Enter** to save, then **Ctrl** and **X** to quit from editing.

1. The script you have just created basically makes VNC part of the background services that Linux is controlling. We next need to register the script, enter the following commands:

	```
	sudo chmod 755 vncboot
	sudo update-rc.d vncboot defaults
	```

## Step 2: Setting up the client
With the server side completed, you now need to setup a similar script for the HTML5 client.

1. Type `sudo nano vncproxy` and pres **Enter**. You will now be editing a blank file. Copy and paste or type the following:

	```
	### BEGIN INIT INFO
	# Provides: vncproxy
	# Required-Start: $remote_fs $syslog
	# Required-Stop: $remote_fs $syslog
	# Default-Start: 2 3 4 5
	# Default-Stop: 0 1 6
	# Short-Description: Start VNC http Proxy at 	boot time
	# Description: Start VNC http Proxy at boot 	time.
	### END INIT INFO

	#!/bin/sh
	#/etc/init.d/vncproxy

	USER=root
	HOME=/root

	export USER HOME

	case "$1" in
 	  start)
  		echo "Starting VNC Proxy..."
  		screen -S noVNC -dms noVNC /usr/local/share/noVNC/utils/launch.sh --vnc localhost:5901 --listen 80
  		echo "...done (VNC Proxy)"
  		;;

 	  stop)
  		echo "Stopping VNC Proxy..."
  		screen -x noVNC -X stuff "^C"
  		echo "...done (VNC Proxy)"
  		;;

 	  *)
  		echo "Usage: /etc/init.d/vncproxy {start|stop}"
  		exit 1
  		;;
	esac
	exit 0
	```

1. Press **Ctrl** and **O** followed by **Enter** to save, then **Ctrl** and **X** to quit from editing.

1. Then register this script with Linux by typing:

	```
	sudo chmod 755 vncproxy 
	sudo update-rc.d vncproxy defaults 98
	```

1. Now reboot your Raspberry Pi by typing `sudo reboot` and both services will start up automatically. When the Pi has rebooted you should now be able to enter the IP address of the Raspberry Pi into the web browser of the host computer. You will be prompted for the password that you specified when setting up the VNC server.

	*Note: I have occasionally observed an error on the first time that you try to connect, I think this is being caused by the proxy getting started before the VNC server socket is open.
If you see this the top bar goes red. Just hit refresh (F5), enter the password again and it should work.*

## Step 3: Master and Slave mode
There is another trick you can do here if you want to really have things sown up. 

Using the following instructions each Raspberry Pi will be directly connecting to the host computer using a single Ethernet cable, thus making a completely isolated point to point network between the two and therefore your network administrators shouldn't have any cause to complain. Note: you don't need a cross over cable for this, a standard cable will work because the Pi Ethernet port auto-switches the transmit and receive pins.

Firstly we'll need to install some more software on the Pi. We’re going to make the Pi Ethernet port behave in a similar way to a home router. This means assigning a static IP address to it and installing a DHCP service (dnsmasq) that will respond to address requests from the host computer.

1. Enter these commands:

	```
	sudo apt-get install dnsmasq -y
	```

1. It’s a good idea to use an IP address range that is very different to your main network, so let’s use `10.0.0.X`. To configure this we must edit the network interfaces file, enter the following command:

	```
	sudo nano /etc/network/interfaces
	```

1. Find the following line `iface eth0 inet dhcp` and add a hash symbol at the start of the line to disable it and then add the other four lines shown below.

	```
	# iface eth0 inet dhcp
	auto eth0
	iface eth0 inet static
	address 10.0.0.1
	netmask 255.255.255.0
	```

1. Press **Ctrl** and **O** followed by **Enter** to save, then **Ctrl** and **X** to quit from editing. The Raspberry Pi will now have a static address of `10.0.0.1`

1. Next configure `dnsmasq` (that you installed earlier) to give out IP addresses. I am going to explicitly specify a configuration file for the dnsmasq service so let’s first make a backup of the default config file and then save my one in its place:

	```
	cd /etc
	sudo mv dnsmasq.conf dnsmasq.default
	sudo nano dnsmasq.conf
	```

1. You should now be editing a blank file. Type the following into it:

	```
	interface=eth0
	dhcp-range=10.0.0.2,10.0.0.250,255.255.255.0,12h
	dhcp-option=3,10.0.0.1
	dhcp-option=6,10.0.0.1
	```

	The first line tells dnsmasq to listen for DHCP requests on the Ethernet port of the Pi. The second line is specifying the range of IP addresses that can be given out. The third and fourth lines tell the host computer what it's default gateway and DNS server settings are.

1. Next make a small edit to the hosts file. This will allow the user to type in pi into the browser instead of `10.0.0.1`. Enter the following `sudo nano /etc/hosts` to edit the hosts file.

1. Find the line that says `127.0.0.1		raspberrypi` and add the following line:

	`10.0.0.1		pi`

1. Press **Ctrl** and **O** followed by **Enter** to save, then **Ctrl** and **X** to quit from editing. Next disconnect the Pi from the LAN and reboot by typing `sudo reboot`.

1. You can go ahead and plug in the single Ethernet cable directly from the Pi to the host computer.
The host computer should then be given an IP address which will be 10.0.0.X where X is a random number between 2 and 250.

One thing to try is to open up a command prompt on the host computer (a Terminal on OSX and Linux) and enter the following command;

```
ping 10.0.0.1
```

If you see `reply, reply, reply` then it's working. If you see request timed out then something is wrong and you'll need to go back and double check everything.

You should now be able to open up a web browser on the host computer and enter `pi` into the address bar to get to the VNC client page. 

*Note: Windows users:* This may not work properly on Windows (you'll still need to use `10.0.0.1`) but if you install a package called `winbind` you'll be able to type the Raspberry Pi hostname into the browser.  Usually this is `raspberrypi`.  The winbind package can be installed with the command below:

`sudo apt-get install winbind`

It should be fine on OSX, Ubuntu and any flavour of Linux though.

![](images/vnc_firefox.png)

You will be prompted for the password that you specified when setting up the VNC server.

*Note: I have occasionally observed an error on the first time that you try to connect, I think this is being caused by the proxy getting started before the VNC server socket is open.
If you see this the top bar goes red. Just hit refresh (F5), enter the password again and it should work.*
