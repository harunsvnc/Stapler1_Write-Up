hello, I will summarize how to finish Stapler1 vulnerable machine hacking.
step1: After importing machine file to virtual box, first i used arp-scan tool to find out the ip address of the vulnerable machine.

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/0d0e6e34-babc-4c78-8e77-717a725beb87)

as shown the above picture our target machine's ip address is 10.0.3.5. Hence i started a comprehensive nmap scan to see the available ports and services.

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/e2bbfa22-c823-462c-8bf8-d0d7d3a8e6bc)

then it seems that some services are open, i tried ftp, smb and i couldn't find anything critical, just got some usernames on the system. Hence i decided to move forward through http. Over port 80, it didn't
give me any thing even i scanned with nikto and broute forced with dirbuster. When i connect to machine over port 12380 also it gives a strange thing to me. I run nikto to scan the target and nikto found
2 folders exist in robots.txt file. targetmachine/blogblog and targetmachine/admin112233 folders. second one gives nothing to achive our goal to me but the first one brought a wordpress page to me. here again
some user account names and company name were learnt. Hence i used wpscan tool to get some information about the target.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/196fb2a4-8d86-4ed7-b84c-dc24f0364e18)
it gave some available folder names here. i focused on this folder and found that in the plugins folder there is a vulnerable app here.

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/997d4026-2156-4636-83c1-b8188033474f)
after googling the app, found an exploit code to read files on the system via unauthorized way. whatever you type at the end of the url, if there is no permission related problem, its giving you out put of 
this file as jpeg format in the uploads folder on this machine.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/97c41316-b1b2-4d62-af2d-f6ed9e560d5c)
first i tried to get /etc/passwd and got the content of the file and got available users on the system.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/f9196582-8db3-4953-b085-db7f95f43ac2)
hence i extracted username to the usernames.txt file.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/773ded00-07b7-471e-8b64-303de037e748)
then i wanted to view wp-config.php file to find a chance to get some critical data with the same way.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/8938e91f-86c6-4f4a-a22d-2ca6d81d4e7d)
here i shows some credentials for the db connection.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/1a5f5fc8-e973-411f-af63-6df66fb3255b)

Nikto told me also phpmyadmin page is accessible in the beginning so i succeeded to login with this credentials.


![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/a8c0d898-896a-43a5-844f-2d79ec911882)

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/80db47f4-4da2-457f-a480-4a27accd4ac2)
 But i tried something different. i got a password that is "plbkac" also i had a usernames.txt retrieved from passwd file.
 i used hydra to brute force ssh service with this password. with this spraying attack, i found a username uses this password.

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/b3b419fe-d24a-41d5-a039-9d38c91f79c7)

and tried an ssh connection and done!. i was in.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/d3d3f7b5-ae07-4720-befc-d8757cb2795e)

but now i had a restricted user so it was time to escalate my privileges.
under /home folder, there were a lot usernames and found the bash_history file, maybe we could find some critical data on this files.i wrote simple bash
script to navigate everyone's home folder and retrive the content of the their personal .bash_history file. it saved a lot of time.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/105983b4-1b49-431b-a94f-460cd233a6d9)
this command will create an o.txt file on my desktop .

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/7113317c-e2e3-45fb-9769-0c8616fb5186)

i tried peter's password and it worked.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/bc3ba2b3-6c9a-452c-a6b1-5330d9ae07b1)
and it seems peter was  sudo group member.
![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/bf7a3e7a-780d-406d-8493-c0bbda7e25ae)
i knew the peter's password so just typed sudo su to switch the root account.

and finally, i was root.

![image](https://github.com/harunsvnc/Stapler1_Write-Up/assets/75423540/6e149391-ed20-472d-a7cc-14ff179dac97)












