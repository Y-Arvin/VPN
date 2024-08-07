# VPN

## Objective

The VPN project aims to establish a secure and controlled connection between a Windows Server and a Windows 10 client. It allows users to access resources using an encrypted connection from a remote location. Configure, manage, and troubleshoot VPN connections. The hands-on experience is designed to provide practical knowledge of VPN configuration, security practices, and network management.

### Skills Learned

- Knowledge of VPN Protocols: Understand various VPN protocols such as SSTP, IKE, L2TP
- VPN Server Setup: Configure VPN server setting incluuding IP address assignment, authenicaton methods and encryption setting.
- Connection Testing: Verifying the VPN connection is working correctly between the client and server.

### Tools Used

- Hypervisor Software: [Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads) Utilized for the creating, managing, and running virtual machines on a host system.
- Operating Systems: Windows 10, Windows Server 2022

## Walkthrough
**Enable Rule** <br>
Open `Windows Defender Firewall With Advanced Security` click inbound rules on the left. Locate `Secure Socket Tunneling Protocol(SSTP-IN)` right-click it and press enable rule.

<img src="https://i.imgur.com/wtiysoY.png" width="600"> <br> <sup>Ref 1: Enable SSTP-IN rule in windows defender firewall </sup>

**Add VPN Feature On Server** <br>
Open the Server Manager dashboard and click `Add Roles and Features`. Make sure to click the checkbox of `Remote Access` After check the box `DirectAccess and VPN (RAS)` and `Routing`, finish the install 

<img src="https://i.imgur.com/WlNpn27.png" width="400"> <img src="https://i.imgur.com/oTMssZ9.png" width="400"> <br> <sup>Ref 2: Add features </sup>

**Finishing Setup Wizard** <br>
Click on `Open the Getting Started Wizard` after `Deploy VPN only` 
Right-click server `ADDC01` and press `Configure and Enable Routing and Remote Access` Select `Custom configuration` Check the box VPN access and NAT 

<img src="https://i.imgur.com/suoCvpc.png" width="400"> <img src="https://i.imgur.com/QJMuOfm.png" width="400" height="280"> 
<img src="https://i.imgur.com/KHfUt4p.png" width="400"> 
<br> <sup>Ref 3: Setup Wizard </sup>

**Configure Settings For VPN Service** <br>
Right-click server `ADDC01` and go to properties <br>
In general tab, check the boxes that say IPv4 router, LAN and demand-dial routing, and IPv4 remote access server. In the IPv4 tab, click static address pool, then add the pool of IPv4 addresses.`192.168.10.20 - 192.168.10.30` 

<img src="https://i.imgur.com/EenQDXL.png" width="400"> <br> <sup>Ref 4: Shared Folder to VM </sup>

Expand the IPv4 tab and right-click on `NAT` Select the new interface. 
On the `NAT` tab press public interface connected to the internet and the checkbox enable NAT. Press apply, 
On the `Service and Ports`tab select the following `IP Security(IKE)`, `IP Security(IKE NAT Traversal)`, `Remote Desktop`, `Secure Web Server(HTTPS)`, `VPN Gateway(L2TP)`, `VPN Gateway(PPTP)`, `Web Server(HTTP)` in the pop-up input the loopback address `127.0.0.1`
Right-click server `ADDC01` and all tasks and restart 

<img src="https://i.imgur.com/uTFEcH9.png" width="400"> <img src="https://i.imgur.com/pyxXF7g.png" width="400" height="340"> <br> <sup>Ref 5: Shared Folder to VM </sup>

**Configure User** <br>
Find the user in `Active Directory Users and Computers`, right-click the user, and go to properties. 
On the `Member Of` tab click add. Click the Advance button, `Find Now` button. Location `Remote Desktop Users` then add the user to that group.
On the `Dial-in` tab check `allow access` and apply the changes 

<img src="https://i.imgur.com/ifQMFrI.png" width="400" height="455" > <img src="https://i.imgur.com/4tncykf.png" width="400"> <br> <sup>Ref 6: Shared Folder to VM </sup>

**On Windows 10 Add VPN connection** <br>
Open Network & Internet settings. Click the VPN tab on the left, and add a VPN connection. Connect to the VPN and use the login information for the user `mdoe`

<img src="https://i.imgur.com/3n17TTX.png" width="500"> <br> <sup>Ref 7: VPN configuration; Name, IP of VPN server. </sup>

**Verify VPN connection** <br>
On the Windows 10 machine, `ipconfig` is ran in command prompt and it shows that the IPv4 address is now `192.168.10.21` which is an IP from the pool.
On the Server machine, in routing and remote access client it now shows the user `mdoe` is being connected.

<img src="https://i.imgur.com/8AuHcF5.png" > <br> <sup>Ref 8: Connectivity to the VPN </sup>

-------------------------------------------------------------------
