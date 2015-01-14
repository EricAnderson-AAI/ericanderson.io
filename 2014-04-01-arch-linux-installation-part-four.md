Picking up from part three, now would be a great time to **enable networking**. 

### Network setup

We'll start off enabling the network for a wired connection by entering the following command:

* systemctl enable dhcpcd&commat;eth0.service
	
> If you don't have a wireless card you can skip the following

For enabling wireless, run the following:

* pacman -S wireless&lowbar;tools wpa&lowbar;supplicant wpa&lowbar;actiond dialog
* press Y to proceed with the installation
	
Now we need to **edit** our **pacman.conf** file to **enable** the **multilib package repository**.

### Enabling Multilib

The reason why we need to enable multilib is so we can run 32-bit applications on our x86_64 system. We do this by **editing** our **pacman.conf**.

* nano /etc/pacman.conf
	
In the **pacman.conf** look for the **[multilib] section** and **uncomment** the following two lines:

* [multilib]
* Include = /etc/pacman.d/mirrorlist

Once that's done **press control-o, enter, then control-x** to **save** then **exit** the pacman.conf.

Next we need to **synchronize our package databases**:
	
* pacman -Sy

### Creating a user account

So far in this tutorial we've been running as root but it's recommended to create a user account and use the sudo command to run commands as root.

* useradd -m -g users -G wheel,storage,power -s /bin/bash <i>EnterUserNameHere</i>
	
Then create a **password** for our user:

* passwd <i>EnterUserNameHere</i>

### Installing sudo

To run root commands as a normal user we need to install sudo:

* pacman -S sudo
* Then press Y, enter to install the package

Not every single user can execute a root command it has to be allowed. We will enable this with the following command and process.

* EDITOR=nano visudo
	
The **/etc/sudoers.tmp file** will open up in nano. The **sudoers file** is where you **configure** who can run the sudo command.

Inside the sudoers file we need to find the line that has **%wheel ALL=(ALL) ALL** and **uncomment it**. Doing this will allow members of **group wheel to execute any command**. If you look back at the command where we added our user, wheel was one of the groups our user was assigned to.

Once the line is uncommented **press control-o, enter, then control-x** to save and exit the file.

### Installing grub

The next package we need to install is the **grub-bios** (grand unified boot loader) used to boot into our system:

* pacman -S grub-bios
* Then press Y, enter to proceed with the installation.

Now we need to install grub to the hard disk:

* grub-install --target=i386-pc --recheck /dev/sda

Next we need to copy our locale into grub:

* cp /usr/share/local/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

Lastly, we need to create the grub configuration file:

* grub-mkconfig -o /boot/grub/grub.cfg
	
### Congratulations!

So now the Arch Linux installation process is done! Congratulations! so lets test it out. 

Type `exit` to leave the **chroot environment**. Then unmount our partitions:
	
* umount /mnt/home 
* umount /mnt
	
Now reboot:

* reboot

Once you've rebooted your vm remove the Arch Linux iso:

* go to Devices -> CD/DVD Devices -> Remove disk from virtual drive

Then reset the vm:

* go to Machine -> Reset
	
Once Arch is booted up you'll be welcomed by a **login command prompt**. This is when the fun begins. We can start molding Arch Linux to be what we want it to be! 

### Wrap Up

In the next tutorial we will start molding Arch into something useful!
	
### Additional Resources

* [Arch Linux Locale](https://wiki.archlinux.org/index.php/locale)

### Series Links

* [Arch Linux Series - Part One](http://evlsnoopy.com/arch-linux-installation-series)
* [Arch Linux Series - Part Two](http://evlsnoopy.com/arch-linux-installation-part-two)
* [Arch Linux Series - Part Three](http://evlsnoopy.com/arch-linux-installation-part-three)
* [Arch Linux Series - Part Five](http://evlsnoopy.com/arch-linux-installation-part-five)