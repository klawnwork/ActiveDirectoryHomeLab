<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of utilizing Oracle Virtual Box to launch virtual Windows servers and clients, configuringing DHCP, NAT, RAS, internal NICs, IP addresses and subnet masks, installing Active Directory services, and running a PowerShell script to create users. This project simulates basic networking with Active Directory and Windows.  
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle Virtual Box</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> 
- <b>Windows Server 2019</b>

<h2>Program walk-through:</h2>


<p align="center">
Launch Virtual Box and create a new machine with Server 2019 ISO. Configure second NIC to connect to our client internally: <br/>
<br />
<img src="https://imgur.com/ZeB9Pu4.png" height="80%" width="80%"/>
<br />
<br />
Upon launching VM there is annoying mouse lag and no auto-adjust when we change the screen size, so we add Guest Additions to normalize the experience: <br/>
<br />
<img src="https://imgur.com/IVak4Ld.png" height="80%" width="80%"/>
<br />
<br />
Configuring network properties for internal NIC, no default gateway because the Domain Controller acts as a defualt gateway. DNS comes with AD, which we install later, so the preferred DNS can be the DC IP address, or in this case the loopback address: <br/>
<br />
<img src="https://imgur.com/uEBBJy6.png" height="80%" width="80%"/>
<br />
<br />
Install Active Directory Domain Services and add a new forest which will be our domain: <br/>
<br />
<img src="https://imgur.com/VglxqJc.png" height="80%" width="80%"/> <img src="https://imgur.com/xmZ21bA.png" height="80%" width="80%"/> 
<br />
<br />
Create a new Organizational Unit and add ourself as an admin. Once the user is created, we have to assign it to Domain Admins: <br/>
<br />
<img src="https://imgur.com/CFdIcJR.png" height="80%" width="80%"/> <img src="https://imgur.com/mIKC0bn.png" height="80%" width="80%"/> 
<br />
<br />
Installing RAS (Remote Acess Server) allows our client to access the internet through the Domain Controller while remaining on the private network: <br/>
<br />
<img src="https://imgur.com/YpiDz5Z.png" height="80%" width="80%"/> 
<br />
<br />
Configure NAT: <br/>
<br />
<img src="https://imgur.com/nzuXhrk.png" height="80%" width="80%"/> 
<br />
<br />
Install DHCP and configure scope, lease duration, and exclusions. This allows the client on our internal network to receive an IP address: <br/>
<br />
<img src="https://imgur.com/a52PVF9.png" height="80%" width="80%"/> 
<br />
<br />
Execute script in PowerShell ISE to create 1,000 randomly generated users in Active Directory: <br/>
<br />
<img src="https://i.imgur.com/gF0lCV1.png" height="80%" width="80%"/> 
<br />
<br />
We can now see the users in Active Directory: <br/>
<br />
<img src="https://i.imgur.com/pkz6Dze.png" height="80%" width="80%"/> 
<br />
<br />
Now it's time to launch our Windows 10 client, but we must change the NIC from NAT to Internal Network first, as this allows us to connect to the corresponding NIC we configured earlier on the Domain Controller:
<br />
<img src="https://i.imgur.com/NSvjz8k.png" height="80%" width="80%"/> 
<br />
<br />
Once we launch the client VM, we use ipconfig to make sure our network is properly configured. We can see that the default gateway is 172.16.0.1, which is the IP address of the Domain Controller, which means we are connected and have access to the internet. To confirm this we ping an outside address of 9.9.9.9, and we see that we are connected: <br/>
<br />
<img src="https://i.imgur.com/qZJ9Wq6.png" height="80%" width="80%"/> 
<br />
<br />
Registering client as a member of mydomain.com: 
<br />
<img src="https://i.imgur.com/eIS96cS.png" height="80%" width="80%"/> 
<br />
<br />
Now we can go back to the Domain Controller and view the DHCP address leases, we can see our client, Client1, listed: 
<br />
<img src="https://i.imgur.com/d3GOZ4H.png" height="80%" width="80%"/> 
<br />
<br />
We can also see Client1 listed under computers in Active Directory: 
<br />
<img src="https://i.imgur.com/cFbeQAh.png" height="80%" width="80%"/> 
<br />
<br />
Finally, when we go back to the client we can log in with the credentials of any user that is listed in the Domain Controller Active Directory, confirming Client1 as a member of mydomain.com: 
<br />
<img src="https://i.imgur.com/FaTUfJ9.png" height="80%" width="80%"/> 
<br />
<br />
