In this post we're going to be tying up some loose ends. So the first thing we need to do before we get started is login as root, give it a password then change our hostname.

### Root Password Setup

To set a password for the root account use the following:

* passwd <i>EnterPasswordHere</i>
* Then type exit

Now **login as root** and type in the password you just created. Success!

### Hostname Setup

Changing the hostname is extremely easy:

> It is reccomended to have all lowercase letters for your hostname.

* hostnamectl set-hostname <i>EnterHostNameHere</i>

Logout to test if the hostname was set properly:

* exit

Login again as **root**. You should now see your new hostname!

### Network Setup

Issue the following comand to set up network access:

* dhcpcd

Test it out by pinging google:

* ping www.google.com
	
Moving forward for this tutorial I will be using the **xfce desktop environment** but you can use whatever desktop environment you want; like **gnome** or **kde**. Also, I'm going to walk you guys through installing things that are necessary for most desktop environments.

### Audio Setup

First we need to download **alsa-utils**:

* pacman -S alsa-utils
	
Now type `exit` to get out of the **root account** and then **login with your user account** that we created in **part four** of this tutorial.

Once you're logged in with your user account issue the following command:

* alsamixer
	
You can see that **sound is completely muted** for the system. In order to pump up the volume, **use the arrow keys to navigate** to the following channels:

* Master
* Master M
* PCM

Then **raise the gain** on each of those channels to **0.00db**. You can view the gain level at the top of the screen where it says **item**.

Once you're done **press the escape key** to exit and save your settings.

### Setting Up GUI Packages

Type the following command:

* sudo pacman -S xorg-server xorg-xinit xorg-server-utils mesa ttf-dejavu samba smbclient networkmanager networkmanager-vpnc networkmanager-pptp networkmanager-openconnect network-manager-applet gvfs gvfs-smb sshfs

Thanks to **sudo** you will be asked to enter your password so you can run the command. After doing so you will be asked to specify which provider to choose for **libgl**. Since **I have a nvidia card**, I chose it then **pressed enter, then Y** to proceed with the installation.

### Video Card Driver Installation

Alright, so if you don't happen to know what video card you have in your machine here's a command that will tell you exactly what card you have:

* lspci -k | grep -A 2 -i "VGA"
	
Now that you know what video card you have. **Use one of the following commands** that corresponds with your video card:

#### VirtualBox Graphics Adapter:

* sudo pacman -S virtualbox-guest-utils
	
#### Intel Graphics:

* sudo pacman -S xf86-video-intel

#### ATI Graphics:

* sudo pacman -S xf86-video-ati
	
#### Nvidia Graphics:

* sudo pacman -S nvidia
	
After you issue your chosen command, follow the prompts to install your video driver. Driver installation takes just a few seconds to complete.

### Enabling the Network Manager

The last thing we want to do now is **enable the network manager to start up as soon as the computer boots** so that we'll have internet access once Arch has been booted.

* sudo systemctl enable NetworkManager
	
Now reboot:

* sudo reboot
	
Once you're back at the login command prompt, **login with your user account**. 

To test out what we just did we will issue a ping command:

* ping www.google.com
	
**Success!** 

### Wrap Up

So with everything finally setup, in the next part we will install the GUI so that we can interact with our computer via a mouse, play with an actual desktop and browse the internet.

### Series Links

* [Arch Linux Series - Part One](http://evlsnoopy.com/arch-linux-installation-series)
* [Arch Linux Series - Part Two](http://evlsnoopy.com/arch-linux-installation-part-two)
* [Arch Linux Series - Part Three](http://evlsnoopy.com/arch-linux-installation-part-three)
* [Arch Linux Series - Part Four](http://evlsnoopy.com/arch-linux-installation-part-four)