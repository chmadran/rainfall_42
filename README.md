# rainfall_42

![image](https://github.com/chmadran/rainfall_42/assets/113340699/4d25b608-bf98-4fc3-af10-457b4408859e)


<h2>PREAMBLE</h2>

This project is an introduction to cybersecurity. It consists in completing 15 CTF (Capture the Flag) challenges. 

<h2>HOW IT WORKS</h2>

To make this project, we will have to use a VM with an ISO that has been provided with the subject. If the configuration is right, we will get a simple prompt with an IP and a login request. We can always login with user:levelXX password:levelXX. But this user has limited permissions. 

Once logged-in, you will have to find a way to read the `.pass` file with the `levelX` user account of the next level (X = number of the next level).

<h2>HOW TO START</h2>

Step by step :
* Create your VM (64 bits), store it in `/sgoinfre`
* Download the iso that's in the subject
* Upon launching your VM for the first time, add the rainfall.iso image
* You should be prompted to enter a login, enter level00 as login and password
* Connect in ssh from your terminal, e.g `ssh level00@[vm's ip address] -p 4242` (see below for help on ssh set up)
* Good luck !


<h2>USEFUL INFORMATION</h2>
