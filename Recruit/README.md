RECRUİT
![recruit(./images/recruit.png)
Recruit has just launched its new recruitment portal, allowing HR staff to manage candidate applications and administrators to oversee hiring decisions. While the platform appears functional, management suspects that security may have been overlooked during development. Your task is to assess the application like a real attacker, mapping its structure, abusing exposed functionality, and exploiting vulnerabilities.

Can you gain an initial foothold, escalate your access, and ultimately log in as the administrator?

![nmap scan](./images/nmap.png)
Our first task is to use Nmap.
nmap -p- -A -sS 10.113.165.77 
As you can see, ports 22/tcp, 53/tcp, and 80/tcp are open.

![Homepage](./images/homepage.png)
Let’s visit the website. Let’s take a look at the Account API at the bottom of the page.

![API List](./images/api%20list.png)
The Account API exposes a filLet’s keep this here and return to the terminal.
Here, it gives us an API. Let’s keep this here and return to the terminal.

![Gobuster Enumeration](./images/gobuster.png)
I’m running Gobuster.
gobuster -dir -u hhtp://10.113.165.77 -w /usr/share/wordlists/dirb/common.txt
Let’s visit the /mail endpoint.

![Mail](./images/mail.png)
Let’s take a look at mail.log.

![Mail Inbox](./images/mailin.png)
As discussed during deployment:
- HR login credentials (username: hr) are currently stored in the application
  configuration file (config.php) for ease of access during
  the initial rollout phase.
- Administrator credentials are NOT stored in the application
  files and are securely maintained within the backend database.
  Here, we can see the username: `hr`.
Next, we have config.php. We’ll try to access config.html using the `file.php?cv=<URL>` endpoint it gave us. Let’s go back to the website.

![File 1](./images/file1.png)
We got an "Only local files are allowed" error.
In PHP, there is a URL scheme called file://. It is used to read local files on the server.
Let’s try to read the file this way. (For more information on this topic, see: LFI (Local File Inclusion) and SSRF.)

