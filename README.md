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

  






