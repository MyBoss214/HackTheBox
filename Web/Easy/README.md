# Challenge
Gateway crash

# Description


# Vulnerability

Through inspecting the code we determine that the web is vulnerable to CSRF
when you attemp to log in it says browser not supported
# Exploit

Open Burpsuite to capsture the request. Send the request to repeater and change the mozilla/5 to mozilla/7 and you will get the flag
![image](https://github.com/MyBoss214/HackTheBox/assets/149683905/4bb5d83c-fed1-4b38-9abb-ec84d76b01e0)

`HTB{d0_th3_d45h_0n_th3_p4r53r}`
