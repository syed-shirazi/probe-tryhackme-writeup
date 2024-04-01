# TryHackme - Probe - Write-up

![probe](/Images/accueil.png)

[Probe](https://tryhackme.com/r/room/probe)

Use your baseline scanning skills to enumerate a secure network.

I'm gonna explain you how i did to complete this room, step by step, it's very easy.

_Note that you have different ways to solve this room_

Tools you need for this room :

- Nmap
- Gobuster
- Nikto
- Wpscan

So let's go !

### So first start the Machine and the Attack Box and get your IP

1. What is the version of the Apache server ?

So for this answer you need to use : Nmap

> nmap -A -p- [yourmachineIP] -T5

![result](/Images/reponse1.png)

> Answer : **2.4.41**

2. What is the port number of the FTP service?

Again for this question we will refer to our previous nmap scan

![reponse2](/Images/reponse2.png)

> Answer : **1338**

3. What is the FQDN for the website hosted using a self-signed certificate and contains critical server information as the homepage?

Still with the Nmap scan we have the answer for this question

![result3](/Images/reponse3.png)

> Answer : **dev.probe.thm**

4. What is the email address associated with the SSL certificate used to sign the website mentioned in Q3?

So for this question, we need to connect to the port **1443**, but read carefully the nmap scan, it's written there is a ssl certificate, so in your nav bar do not forget to type **HTTPS**

> https://[yourmachineIP]:1443

You will see this page :

![cap1](/Images/cap1.png)
![cap2](/Images/cap2.png)
![cap3](/Images/cap3.png)
![cap4](/Images/cap4.png)

> Answer : **probe@probe.thm**

5. What is the value of the PHP Extension Build on the server?

You have the answer in the previous screenshot

![cap5](/Images/cap5.png)

> Answer : **API20190902,NTS**

6. What is the banner for the FTP service?

So for this question, we can use Nmap again, remember the ftp server is running on the port 1338 (look the scan in question 1&2)

I'm gonna use the script engine of nmap

> nmap --script=banner -p1338 [yourmachineIP]

![result](/Images/reponse5.png)

> Answer : **THM{WELCOME_101113}**

7.  What software is used for managing the database on the server?

So far, we only used nmap, now we will use another tool : **Gobuster**

> gobuster dir -u http://[yourmachineIP]:8000 -w /usr/share/wordlists/dirb/big.txt

![result7](/Images/gobuster.png)

We can see a phpmyadmin page, and if we enter this url in the browser we get this page :

![phpmyadmin](/Images/phpmyadmin.png)

> Answer : **phpmyadmin**

8. What is the Content Management System (CMS) hosted on the server?

We can use Nmap for this question

> nmap -A -p 9007 [yourmachineIP]

![wordpress](/Images/wordpress.png)

> Answer : **Wordpress**

9. What is the version number of the CMS hosted on the server?

You have the answer in the previous screenshot

> Answer : **6.2.2**

10. What is the username for the admin panel of the CMS?

Now we will use another tool called : **WPSCAN**

> wpscan --url https://[yourmachineIP]:9007 --disable-tls-checks -e u

![wpscan](/Images/wpscan.png)
![joomla](/Images/joomla.png)

> Answer : **Joomla**

11. During vulnerability scanning, OSVDB-3092 detects a file that may be used to identify the blogging site software. What is the name of the file?

Now we will use **NIKTO**

> nikto -h [yourmachineIP]:9007 -ssl

![nikto](/Images/osvdb-3092.png)

> Answer : **license.txt**

12. What is the name of the software being used on the standard HTTP port?

You have the answer in the first scan with nmap

![lighttpd](/Images/lighttpd.png)

> Answer : Lighttpd

13. What is the flag value associated with the web page hosted on port 8000?

When we connect to the port 8000, we get an empty page

![empty](/Images/port8000.png)

So may be we have to use gobuster again to find a hidden directory.

> gobuster dir -u http://[yourmachineIP]:8000 -w /usr/share/wordlists/dirb/big.txt

![gobuster](/Images/gobuster2.png)

let's look the result, we have some hidden directories, some have the 403 status, it means not available, and some have the 301 status.
Let's connect to the page Contactus

> [yourmachineIP]: 8000/contactus

![flag](/Images/flag.png)

> Answer : THM{CONTACT_US_1100}

And that's it, we have finished this room, CONGRATULATIONS 😎 🥳

## CONCLUSION

It was an easy room, hope you learned something new and enjoyed it, please give me star on my Github.
