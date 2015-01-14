Now that the base distribution has been installed we can now continue the installation process by creating an **fstab file**.

### What Is An FStab File

For a short explaination, an **fstab file** is basically a text file that **holds all of the partitions** that we have. 

Diving deep into what an fstab file is and does is beyond the scope of this post, but I will put a link below in the **additional resources** section for those of you who would like to learn more.

### Generate The FStab file

To generate our fstab we need to issue the following command:

* genfstab -U -p /mnt >> /mnt/etc/fstab

### Setting The Locale

First we need to **simulate** using our install in a final state before it's finalized in order to make the changes to the locale:

* arch-chroot /mnt
	
Next, using **nano** we need to set up the locale that **suits our needs**:

* nano /etc/locale.gen

Inside the **locale.gen file** and using the **up/down arrow keys** to navigate, we need to **uncomment the locale** we would like to use. Since I'm in the U.S. I will uncomment, meaning **deleting the # signs**, in front of the following locale: 

* en_US.UTF-8 UTF-8 

> Write down the locale you uncommented because we are going to use it in the next two commands!
	
Once we've finished making our change hold the **control key** then **press o** then **press enter to save** it then hold the **control key** again then **press x** to **exit** the file.

Now we need to save the locale we just chose. Here is where you will use the locale that you **wrote down** earlier:

* echo LANG=en_US.UTF-8 > /etc/locale.conf 
	
Ok, so even though we have saved our locale it is **still not active** yet because we haven't rebooted into the new system yet. So to bypass that we issue the following command:

* export LANG=en_US.UTF-8

### Setting The Time Zone

To set the time zone we need to change into the zoneinfo directory:

* cd /usr/share/zoneinfo

To see what zones are in the directory run the following:

* ls
	
Once again since I'm in America I will change into that directory:

* cd America

Then run `ls` again to list the time zones in America.

At this point I will **select Chicago** for my time zone. In order to set it as my time zone I need to **create a link** by issuing the following commands:

* ln -s /usr/share/zoneinfo/America/Chicago /etc/localtime
* hwclock --systohc --utc
    
### Wrap Up

In the next part we will finish installing Arch!

### Additional Resources

* [FStab Information](https://wiki.archlinux.org/index.php/fstab)

### Series Links

* [Arch Linux Series - Part One](http://evlsnoopy.com/arch-linux-installation-series)
* [Arch Linux Series - Part Two](http://evlsnoopy.com/arch-linux-installation-part-two)
* [Arch Linux Series - Part Four](http://evlsnoopy.com/arch-linux-installation-part-four)
* [Arch Linux Series - Part Five](http://evlsnoopy.com/arch-linux-installation-part-five)