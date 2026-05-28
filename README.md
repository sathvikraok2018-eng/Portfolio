# Portfolio
##Project 1: Network reconnaissance using Nmap 
In this lab , I setup a virtual environment to practice scanning on a vulnerable machine 

###Commands Used
***nmap <target ip> -Pn -n -sV -O
-Pn-this flag treats the target as alive and forces nmap to scan the ports which are open in the target machine
-n-normally nmap does the reverse DNS lookup which means it scans and teels the domain name in the target network.
   but this flag tells nmap to skip searching for domain names which eventually saves a lot of time.
-O-this helps to find the operating system of the target which helps to collect further information about those operating systems.

***nmap <target ip> -p -sV
-p
-sV-this flag helps me to find the exact versions of the ports which are open


