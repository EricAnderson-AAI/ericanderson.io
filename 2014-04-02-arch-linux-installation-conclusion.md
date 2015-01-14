We have finally arrived at the part that you've all been waiting for, installing and interacting with the XFCE desktop environment.

### XFCE Setup

Logged in as your **user account** issue the following command:

* sudo pacman -S xfce4 xfce4-goodies gamin firefox
* Then press enter, enter, Y to install the packages and continue the installation

### Editing .xinitrc

We're almost ready to start our desktop environment but first we need to copy then edit the xwindow config file.

* cp /etc/skel/.xinitrc ~
* nano .xinitrc

Now **uncomment** the following line:

* exec startxfce4

Exit the file then save.

At this pont the graphical user environment is installed so let's boot it up with the following command:

* startx

Yay! Welcome to Arch Linux! A panel will pop up with two options, just select **use default config** and you'll be all set to go!

### Bonus - Login Manager

This isn't a requirement but if you are one who likes to have a login screen this part is for you. The first thing we need to do is **install slim**.

* sudo pacman -S slim slim-themes
* sudo systemctl enable slim.service

Then to set the theme for our login screen we have to edit the slim config file.

* sudo nano /etc/slim.conf

Look for the line that has:

* current_theme default

Change **default** to **Arch minimal** then exit and save then **reboot** the system:

* sudo reboot

There you go! you now have a login screen!

### Wrap Up

This concludes the Arch Linux series. If you have any questions or concerns leave a comment below!

### Additional Resources

* [Slim Themes](http://slim.berlios.de/themes01.php)

### Series Links

* [Arch Linux Series - Part One](http://evlsnoopy.com/arch-linux-installation-series)
* [Arch Linux Series - Part Two](http://evlsnoopy.com/arch-linux-installation-part-two)
* [Arch Linux Series - Part Three](http://evlsnoopy.com/arch-linux-installation-part-three)
* [Arch Linux Series - Part Four](http://evlsnoopy.com/arch-linux-installation-part-four)
* [Arch Linux Series - Part Five](http://evlsnoopy.com/arch-linux-installation-part-five)