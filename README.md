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

**We can se our main page, with the box title and some images in there. Let's scan with gobuster too**

``gobuster dir -u http://10.10.252.248:22/ -w /usr/share/wordlists/dirb/common.txt``

![5](images/gobust.jpg?raw=true "gobust")

**Let's take a look into our gobuster output. Let's visit the assets page; we can see some *jpg* files, one of them called** stego.jpg **so we can think about an encrypted image with the help of steganography**

![6](images/assets.jpg?raw=true "assets")







