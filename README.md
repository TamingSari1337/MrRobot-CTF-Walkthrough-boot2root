# MrRobot-CTF-Walkthrough-boot2root

## Finding Box IP
So first of all I will find the ip address of the Mr Robot box using the arp-scan tool. Here I will enter the command arp -scan -l to find the ip address box. so here I have found the ip address box which is 192.168.201.132
![1](https://user-images.githubusercontent.com/106005322/176997143-5d741716-3dce-4814-af9e-1c98bcca80eb.png)


## Enumeration
After I find the ip address box I will do the enumeration using the nmap tool. so here we can see ssh port 22 is closed while port 80/443 is open, this shows that this box is running a website
![2](https://user-images.githubusercontent.com/106005322/176997333-2fa74fce-175a-4dbb-a7af-8345fbacf936.png)

## Viewing target site and try to find any clue
so here I have opened the website from this box
![3](https://user-images.githubusercontent.com/106005322/176997340-66c18fbf-26b2-4257-8268-c3cbe3bfa966.png)

after i tried a few times with several ways to find a hint but i didn't find anything
![4](https://user-images.githubusercontent.com/106005322/176997345-699f0e45-9adb-4d45-a15a-42e729b9604d.png)

## robots.txt hidden file
so after I tried many ways, I remembered if dealing with web based CTF is to check the robots.txt file.
![5](https://user-images.githubusercontent.com/106005322/176997361-fdc0463c-4138-4bc1-a4d6-9523479643a3.png)

## 1st flag
so this is the first flag I've been looking for
![6](https://user-images.githubusercontent.com/106005322/176997364-4b694208-7035-4281-92e7-f5f8fe76b937.png)

## Download fsocity.dic file and use a dictionary
then here also shows there is a file fsocity.dic in the form of a dictionary, this gives a sign that I need to do brute force in the next step
![7](https://user-images.githubusercontent.com/106005322/176997369-2464b94e-4211-446d-968c-f9c714b6225e.png)

## 80,000 Wordlist
so here shows a dictionary file that has 80,000 words
![8](https://user-images.githubusercontent.com/106005322/176997374-38ed8146-1ab1-42c4-95da-6e2e9faa3411.png)

## WordPress password bruteforce (wpscan)
so previously I have done vulnerability analysis using the gobuster tool. I found the wordpress path, so here it can be known that I am dealing with wordpress cms
![9](https://user-images.githubusercontent.com/106005322/176997405-c5e686f3-5748-49e8-a853-26ad57c9cce1.png)

## Exploit
after that I thought that the dictionary file should be used, so I did bruteforce to the wordpress login page, in this picture I took 40 minutes but in fact before that I took 4 hours to get the username and password
![10](https://user-images.githubusercontent.com/106005322/176997408-349445c0-2f71-47b1-93aa-450c172241ad.png)

## Login into WordPress
after getting the username and password, I logged in to see the content in the dashboard but there was nothing
![11](https://user-images.githubusercontent.com/106005322/176997412-86b22f16-16b6-4d68-8c72-a45b21726c8e.png)

## Inserting PHP Backdoor
so here I will do an exploit by inserting PHP Reverse Shell in the 404 template, Here I use a shell from https://github.com/pentestmonkey/php-reverse-shell
![12](https://user-images.githubusercontent.com/106005322/176997415-ec2853da-8388-4289-a259-a270b38eed0d.png)

## Shell Listening
then open netcat to listen to the shell on port 1234 (default)
![13](https://user-images.githubusercontent.com/106005322/176997420-c725fa27-9ce3-4c02-9a8b-c945cdb5f051.png)

## showing that im still not a root user
so here shows I'm still not a root user
![14](https://user-images.githubusercontent.com/106005322/176997423-eb6c726f-4ec2-4a8d-bd4e-34512094f6ca.png)

when i go into the robot directory here i have found the second flag but it is not accessible because i am not the root user. but here I have found a password that is encrypted in the form of md5 hash
![15](https://user-images.githubusercontent.com/106005322/176997424-cb9d277b-658e-4408-a0e2-3a83a1b4efdf.png)

## Get MD5 hash and decrypt it
After that I use the online tool to decrypt the password
![16](https://user-images.githubusercontent.com/106005322/176997426-39d17911-78f2-40ce-a67a-86a138d7b39c.png)

## Import tty-shell
If you have a non-tty-shell, there are certain commands and things you can't do. This can happen if you upload a reverse shell on a web server, so that the shell you get is by www-user data, or similar. These users are not meant to have shells because they do not interact with the system as humans do.

So if you don't have a tty-shell you can't run su, sudo for example. This can be annoying if you manage to get the root password but you can’t use it. because I use non-tty-shell here I will enter a command like python -c 'import pty; pty.spawn ("/bin/sh") 'or echo os.system ('/bin/bash ') to spawn tty shell

after that I will try to access the root user by entering the password that was encrypted earlier and here shows I succeeded
![17](https://user-images.githubusercontent.com/106005322/176997432-3d160050-c80f-461e-a796-af88486a23fc.png)

## Privilege escalation
Now I need to find the third and last key. So before I tried to enter the /root folder but access was denied. Then I decided to check out a program that has SUID bit, here I can see where nmap is as a SUID bit.

Nmap has SUID bit set. A lot of times administrators set the SUID bit to nmap so that it can be used to scan the network efficiently as all the nmap scanning techniques does not work if you don’t run it with root privilege.

However, there is a functionality in nmap older versions where you can run nmap in an interactive mode which allows you to escape to shell. If nmap has SUID bit set, it will run with root privilege and we can get access to ‘root’ shell through it’s interactive mode.
![18](https://user-images.githubusercontent.com/106005322/176997434-e966a079-3e57-43b8-b1cf-1b748ba76f3f.png)

then I will execute nmap with interactive mode, don't forget to enter the "!sh" command to allow escape jail shells. 

then I will go into the root directory where previously my access was denied because I am not a root user. but now I'm in root user where this is good because I can find the third flag.

so now I have found the third flag on the root directory, so here shows that I have successfully completed the task from this box
![19](https://user-images.githubusercontent.com/106005322/176997461-ba813a06-d6b0-4ec8-9020-af2d5bd36af6.png)

## List of flags
the following is a list of the three flags I got from this CTF box
![20](https://user-images.githubusercontent.com/106005322/176997457-182a0891-0bb3-41b8-b3d1-99c9d909402a.png)

