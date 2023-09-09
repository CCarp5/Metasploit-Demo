<h1>Basic Metasploit Demo</h1>


<h2>Description</h2>
In this project I will be using Nmap and Metasploit to take advantage of an outdated version of Windows on a VM. All of this is done on VM's within my own network.
<br />


<h2>Utilities Used</h2>
 
- <b>Metasploit</b>
- <b>Nmap</b>

<h2>Environments Used </h2>

- <b>Kali Linux</b> 
- <b>Windows 10</b> 

<h2>Program walk-through:</h2>

We're going to begin by looking for a target machine.
 <br />
  <br />
Since this is a homelab project we'll be scanning our own subnet for our target.
 <br />
  <br />
We can do this in Nmap by entering the command <b>sudo nmap -sS 10.0.2.0/24</b>.
 <br />
  <br />
This will conduct a stealth scan of our subnet and identify potentially exploitable machines with open ports.

<p align="center">
Scanning the subnet using nmap:  <br/>
<img src="https://i.imgur.com/QeiLXfD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  Here we identify the IP address 10.0.2.6. This will be our target machine.
   <br />
  <br />
  We'll use a built in script on Nmap to scan this machine for vulnerabilites.
   <br />
  <br />
  We'll run this script by using the command <b>sudo nmap --script vuln X.X.X.X</b> (replacing the X's with the target machines IP).
  
<br />

<p align="center">
Conduct a vulnerability scan on our target machine using nmap:  <br/>
<img src="https://i.imgur.com/wYfJxSi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  Here we see the scan was able to identify a vulnerability. We want to make note of the Microsft Security Bulletin number "ms17-010"

  We now have the information we need to begin attempting to exploit the machine.

  Prior to opening Metasploit and begining to run our exploits we'll need to start its auxiliary database.
<br />
<br />
  We'll do this by running the command <b>service postgresql start</b>.
 <br />
 <br />
  After successful authentication we'll be ready to start Metasploit, using the command <b>sudomsfconsole</b>.

<br />

 <p align="center">
Starting the database and opening Metasploit:  <br/>
<img src="https://i.imgur.com/14t03AW.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  Next we are going to search Metasploit for potential exploits using the Microsoft Security Bulletin number we noted from our Nmap vulnerability scan.
  <br />
  <br />
  We'll do this by entering the command <b>search ms17</b> in the Metasploit console.
  <br />
  <br />

   <p align="center">
Searching for and selecting an exploit:  <br/>
<img src="https://i.imgur.com/J9Ex4IW.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  After looking at our search results we decide to start by trying exploit #1.
  <br />
  <br />
  We'll select this exploit by entering the command <b>use1</b>.
   <br />
   <br />
   We now need to see if we need to do any additional configuration prior to running our selected exploit.
    <br />
    <br />
    We'll do this by entering the command <b>options</b>.

   <p align="center">
Configuring the exploit:  <br/>
<img src="https://i.imgur.com/3zCAesD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  Here we are looking for required options that are not pre-filled.
  <br />
   <br />
  We can see that the only required option not already filled is "RHOSTS".
  <br />
   <br />
  This option is referring to the target host address, which we have from our earlier scan of our subnet on Nmap.
    <br />
   <br />
   We'll provide this address by entering the command <b>set RHOSTS X.X.X.X</b> (Once again replacing the X's with the target machine's IP).
   <br />
   <br />
   We are now ready to run the exploit.
     <br />
   <br />
   We'll do this by running the command <b>exploit</b>.

   <p align="center">
Running the exploit and verifying success:  <br/>
<img src="https://i.imgur.com/CGrMpKV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

The exploit has opened a meterpreter shell, giving us access to the target machine.
 <br />
   <br />
   Here we can run the command <b>sysinfo</b> to verify that we are in the target machine.
    <br />
   <br />
   To show one of the many things we can do with this meterpreter shell we are going to run a keyscanner on our target machine.
    <br />
   <br />
   To begin we'll need to migrate to a process currently running on our target machine.
    <br />
   <br />
  We'll do this by first opening a list of the processes running by using the command <b>ps</b> in our meterpreter shell.

   <p align="center">
Opening the process list:  <br/>
<img src="https://i.imgur.com/SO3Ttbx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

For this lab we'll go ahead and use the OneDrive.exe process.
 <br />
   <br />
We'll migrate to this process using the command <b>migrate XXXX</b> (Replacing the X's with the PID number associated with our selected process).

 <p align="center">
Migrating processes and starting the keylogger:  <br/>
<img src="https://i.imgur.com/2wSbjpf.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>

We can now start the keyscanner by entering the command <b>keyscan_start</b>.
<br />
   <br />
To verify our keyscanner is operational, we'll move over to our target machine and type something in NotePad.

 <p align="center">
Typing on the target machine to verify keyscanner success:  <br/>
<img src="https://i.imgur.com/X0wj0re.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Now we'll go back to our Kali Linux machine and dump the keystrokes.
<br />
   <br />
We'll enter the command <b>keyscan_dump</b>.
<br />
   <br />
This will stop the keyscanner and display the results from our scan.

 <p align="center">
Stopping the keyscanner and viewing the results:  <br/>
<img src="https://i.imgur.com/dnHQFbR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<br />
As we can see, the scanner was able to capture all keystrokes, including typing "notepad" to search for the application.
<br />
   <br />
   This will conclude this lab. As a reminder all of this was done on my own virtual machines. 
    <br />
   Any attempt to recreate this or a similar lab should be conducted in your own environment using virtual machines. Thank you for taking the time to view this lab!
   
  ****
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
