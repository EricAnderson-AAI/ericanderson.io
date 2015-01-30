Lately I have been faced with a strange error that only seems to occur in Google's Chrome browser. For example, I would go to a website and all of the images would be broken or the site just wouldn't load up. Opening Chrome's developer tools by pressing **F12** and clicking on the **Console tab** the problem revealed itself: `Error 105 (net::ERR_NAME_NOT_RESOLVD): Unable to resolve the server's DNS address`.

### What's Error 105?

Receiving an Error 105 means a DNS resolution error has occurred. Basically what happens when you enter a URL of a website and go to it, Chrome will try to resolve the IP address of the website using the DNS (Domain Name System) protocol. If Chrome fails to reach the DNS server, the name resolution fails and the glorious Error 105 is returned.

### Solution

So what I ended up doing to solve this problem was to add a manual DNS address to Windows so that Chrome will resolve DNS queries.

* Press **Windows Key + R** to open the Run dialog.
* Once the Run dialog box is available type **ncpa.cpl** then click ok.
* Next, choose your **network adapter** then **right-click** on it and click **Properties**. I chose my Wi-Fi adapter.

{<1>}![Network adapter](https://raw.githubusercontent.com/EricAnderson-AAI/evlsnoopy.com/master/images/2015-01-30-google-chrome-error-105-network.PNG)

* Now click on **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.

{<2>}![Wireless Network Connection Properties](https://raw.githubusercontent.com/EricAnderson-AAI/evlsnoopy.com/master/images/2015-01-30-google-chrome-error-105-wifi.PNG)

* At this point what you want to do is select the **Use the following DNS server addresses** option and enter in the exact values that are shown in the image below.

{<3>}![Internet Protocol Version 4 (TCP/IPv4)](https://raw.githubusercontent.com/EricAnderson-AAI/evlsnoopy.com/master/images/2015-01-30-google-chrome-error-105-ipv4.PNG)

* **Click OK** then close and you're done!

Now try revisiting the website that gave you trouble before and this time all of the content should load as expected and if you check the console in Chrome's developer tools **(F12)** you shouldn't see the Error 105!

That's it for this post! If you're still receiving Error 105 or see some typo's or other mistakes in this post, leave a comment below or [submit a pull request](https://github.com/EricAnderson-AAI/evlsnoopy.com). 
