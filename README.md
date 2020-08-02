# 'Jack-of-All-Trades' box writeup
## Jack of All Trades is a CTF box written by MuirlandOracle and available on the [TryHackMe platform](https://tryhackme.com).
## Read about [How to allow restricted ports](https://support.mozilla.org/en-US/questions/1083282#answer-780274)
## ![bg](images/backgroundjack.jpeg?raw=true "Title")
+ **We deploy the machine and start with an nmap scan for open ports**

``nmap -sV -sC -oN scan1 10.10.252.248``
      
+ **We can see 2 open ports with some services: ssh and http. The first strange thing is that the services are opened on reversed ports. Ssh is opened on the 80 ports and http on the 22 one**

![1](images/nmap_scan_jack.jpg?raw=true "Nmap_scan")

+ **Let's try to get to the http web-site on the 22 port. We see an browser error: seems like Firefox has canceled our request for kind of security. That's because the unusual use of 22 port for the http service**

![2](images/restrict.jpg?raw=true "restrict")

**We can allow this restricted port making some configuration inside the mozilla browser. [Allow restricted ports](https://support.mozilla.org/en-US/questions/1083282#answer-780274). Go into the about:config page in the url, search for the ports and add the network.security.ports.banned.override string, with the 22 value**

![3](images/add_string.png?raw=true "add_string")

![4](images/welcome.png?raw=true "welcome")

**We can see our main page, with the box title and some images in there. Let's scan with gobuster too**

``gobuster dir -u http://10.10.252.248:22/ -w /usr/share/wordlists/dirb/common.txt``

![5](images/gobust.jpg?raw=true "gobust")

+ **Let's take a look into our gobuster output. Let's visit the assets page; we can see some *jpg* files, one of them called** stego.jpg **so we can think about an encrypted image with the help of steganography**

![6](images/assets.jpg?raw=true "assets")

+ **We can try to extract the stego image to see de hidden data, so we're gonna use steghide**

``steghide --extract -sf stego.jpg``

**A passphrase is requested, so we cannot immediately decrypt the image, but we can continue to enumerate the http page. Let's take a look into the source code of the page**

+ **We can spot a message left in the source code of the page: a recovery message which tells us we can connect on the /recovery.php page and there's also a base64 encoded message**

![7](images/base64.jpg?raw=true "base64")

**Let's try to decrypt our message**

``echo "UmVtZW1iZXIgdG8gd2lzaCBxxxxxxxxxx" | base64 -d``

![8](images/decrypt.jpg?raw=true "base64")

**We got a message and a password too! Let's use it to decrypt the image with steghide**

![9](images/first_steg.jpg?raw=true "first_steg")

+ **A creds.txt file was hidden inside, but the stego.jpg wasn't the good path. Let's download the other images from the assets page and extract them**

![10](images/real_steg.jpg?raw=true "real_steg")

``steghide --extract -sf header.jpg``

**Bingo! We got a username and a password inside the header.jpg image. Let's go to the /recovery.php page and try to login with the credentials**

+ **Logging in with our credentials on the page, we are redirected to a page with the message:**

``GET me a 'cmd' and I'll run it for you Future-Jack.``

![11](images/login.jpg?raw=true "login")

**Now, let's go and try to get some system commands inside the url:**

``http://10.10.252.248:22/nnxhweOV/index.php?cmd=whoami``
















