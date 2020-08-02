# 'Jack-of-All-Trades' box writeup
## Jack of All Trades is a CTF box written by MuirlandOracle and available on the [TryHackMe platform](https://tryhackme.com).
## Read about [How to allow restricted ports](https://support.mozilla.org/en-US/questions/1083282#answer-780274)
## ![bg](images/backgroundjack.jpeg?raw=true "Title")
+ **We deploy the machine and start with an nmap scan for open ports**

``nmap -sV -sC -oN scan1 10.10.175.11``
      
+ **We can see 2 open ports with some services: ssh and http. The first strange thing is that the services are opened on reversed ports. Ssh is opened on the 80 ports and http on the 22 one**

![1](images/nmap_scan_jack.jpg?raw=true "Nmap_scan")

**Let's try to get to the http web-site on the 22 port**
