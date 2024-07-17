# Quick Writeup - Agent Sudo TRYHACKME You found a secret server located

## Under the deep sea, your task is to hack inside the server and reveal the truth.

### ENUMERATION: 
### 3 ports from nmap scan 'nmap -A -O -T4 10.10.158.70'

Ports: ftp - 21 ssh - 22 http - 80

Hint is user agent:
We then open Burp, we know the agents go by letters
so I try all the letters within the user agent We get back that C yields
a different responce, upon opening in browser we are given the name
"chris" The next step is to get the FTP password, with the name chris
we launch hydra, with chris and rockyou against the FTP We get the
password crystal

We login to the FTP using these creds. chris-crystal

We are in!
</br>
Time to look around... We download everything to inspect on our rig.
mget \*
</br>
Within the note left we are told our password is stored inside the
images we got to we extract the data from the images I used: exiftool,
this didnt work so I went to find help and was pointed at binwalk.

Binwalk extracts a zip that is locked, we use zip2john then bruteforce
the zip. Password: alien

inside this zip is another message with a name: QXJLYTUx Decoded from
base64 gives us our next flag: area51

Now to extract info from our other image: we use steg

steghide --- extract -sf cute-alien.jpg

We are prompted for a password which is Area51 from before.

We extract a message with a few new names, and the password hackerrules!
</br>
As advices by our box, this is the password for SSH.

Using our credentials we can now ssh into the machine. Right away our
user flag is sitting here. b03d975e8c92a7c04146cfa7a5a313c7

We also take the image file: scp james@10.10.25.88:Alien_autospy.jpg
/home/

reverse image search and we have our flag: Roswell alien autopsy
</br>
Now to escalate privs First... 
whoami 
sudo -l 
cat /etc/os-release 
uname \-r

We find in interesting version of sudo... google time

The CVE that takes advantage of this version of sudo (sudo 1.7xx) is 2019-14287
It is a simple command we enter into our SSH session to get root:
sudo -u#-1 /bin/bash

We are now root, and can CD /root and cat the root.txt to get our last two flags.
