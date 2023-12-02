# SOC-Lab
Comprehensive documentation for setting up a Security Operations Center (SOC) lab. 

### Components

- [Proxmox HomeLab](https://github.com/kyhomelab/HomeLab)

### Software Used
- [Windows Server 2022](https://www.microsoft.com/en-US/evalcenter/evaluate-windows-server-2022)
- [PfSense](https://www.pfsense.org/)
- Windows 10 VM
- [BadBlood](https://github.com/davidprowe/BadBlood)
  <br>
  
## PfSense
1. Downloaded the [Iso](https://www.pfsense.org/download/)
2. Created a new VM Using the PfSense iso
3. Went through and followed [documentation](https://docs.netgate.com/pfsense/en/latest/recipes/virtualize-proxmox-ve.html "After the fact of trying to get this set up for TWO DAYS, because I had to get a new Nic with ethernet ports since only one would not work :( ") ensuring to create a new Linux Bridge and adding that to the Hardware of the VM
4. Finished going through installation and now I have a WAN and a LAN configured for my PfSense
![PfSense VM](https://i.imgur.com/K4lojWW.png)
5. Im able to access the GUI within my Windows Server 2022
<br>

## Windows Server 2022
1. Download the [Iso](https://info.microsoft.com/ww-landing-windows-server-2022.html "After signing up because whyyyyyy?")
2. Made sure to use the PfSense Network Device which I have configured as vmbr1
3. After installation I now I had access to Windows Server Manager and able to access PfSense
4. I now have a closed network seperate from my own in order to manage and configure Active Directory and monitor and create
   ![Server 2022 Dashboard](https://i.imgur.com/gMunQzw.png)
5. Under Server Manager I went to Roles and Features > Select Server Roles > Select Active Directory Domain Services > Add Features
6. After the installation completed I Promoted the server to a domain controller
7. Created a New Forest named soc.lab and after a restart I now have an Active Directory Domain, now we need to populate it
8. I created a user for myself to join the AD on Win10
<br>

## BadBlood
In order to populate my AD, im going to use [BadBlood](https://github.com/davidprowe/BadBlood "BadBlood by Secframe fills a Microsoft Active Directory Domain with a structure and thousands of objects. The output of the tool is a domain similar to a domain in the real world. After BadBlood is ran on a domain, security analysts and engineers can practice using tools to gain an understanding and prescribe to securing Active Directory. Each time this tool runs, it produces different results. The domain, users, groups, computers and permissions are different. Every. Single. Time.")
1. On the Win Server I installed [Git](https://git-scm.com/ "This took me waaayyyy to long to figure out I needed to download this")
2. After installation, I opened Active Directory Module for Windows PowerShell and ran the commands
```bash
# clone the repo
git clone https://github.com/davidprowe/badblood.git
#Run Invoke-badblood.ps1
./badblood/invoke-badblood.ps1
```
3. Once BadBlood finished running, it generated 2500 Users, 500 Groups, and 100 Computers
![BadBlood](https://i.imgur.com/1LJDDT9.png)
![BadBlood](https://i.imgur.com/fnsg7vE.png)

## Windows 10 VM (22H2)
1. Downloaded the [Windows 10 Iso](https://www.microsoft.com/en-us/software-download/windows10)
3. Created a VM utilizing Windows 10 Pro, once loaded in, changed the PC name to SOC-WIN10
4. Went to Settings > System > About > Rename this PC (advanced) > Computer Name > Change (Domain) > Member of Domain: soc
   ![Network Path](https://i.imgur.com/5gsv6QO.png)
   ![Network Path](https://i.imgur.com/4MmFKKL.png)
```bash
Unforntanetly I kept getting either a DNS error or a Network Path error.
Took me a few hours to resolve this issue.
Pinging the server IP or the DNS from Win10 was always successful.
After a while, I tried pinging the Win10 from the WinServer with all requests timing out. Even did tracert to no avail
I disabled the firewall for both Win10 and WinServer since the communication was being blocked from WinServ to Win10
After doing that tracert was able to ping successfully at 1
----------------
Update: After 2 more hours of troubleshooting...the issue was the IPv6...
```
