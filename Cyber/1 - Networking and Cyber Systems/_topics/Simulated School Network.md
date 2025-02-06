---
tags:
  - Cyber
  - yr11
---

This network is a simulated network for a school, with two subnets depicted - one for students, and one for the staff. The networks will have shared infrastructure, however shouldn’t be able to communicate with each other.

# Add Devices

Add the various devices to create two subnets, one for students and one for staff, that are both connected to a single router.
![[simulatedSchoolPacketTracer.png]]
	
Note: You will need to add more Fast Ethernet ports to **both** of the switches. Turn the switches off, and add more “PT-SWITCH-NM-1CFE” modules.
![[simulatedSchoolPacketTracerNICs.png]]

# Configure Servers
	
❓ For each of the servers, you will need to complete the following steps. The only difference for the servers is the IP Addresses used.
For the student network, we’ll use `192.168.2.x`
For the staff network, we’ll use `192.168.1.x`
Both networks will be configured with the subnet mask: `255.255.255.0`

Set the name of the server. 
![[simulatedSchoolPacketTracerServerName.png]]

Change to the FastEthernet0 interface, and set the IP Address of the server.

The student server is to be set as `192.168.2.1`. The staff server is `192.168.1.1`.
![[simulatedSchoolPacketTracerSetIP.png]]


Enable DHCP in the Services tab and set the Start IP Address to be `192.168.x.10`. 

This will configure DHCP for the clients on the specific network (student or staff) to be allocated IPs from `192.168.x.10` through to `192.168.x.254`.
![[simulatedSchoolPacketTracerGateway.png]]


The default gateway and DNS IPs will need to be set after the router is configured. 
	
# Configure PCs
	
After the DHCP servers have been configured, enable DHCP on each PCs FastEthernet port.

Click on a PC, and then the Config tab. Next click the FastEthernet0 interface, and enable DHCP for IP Configuration.

After a moment, the device should be allocated an IP address.

> You may need to wait a minute or two for the IP to be allocated.
![[simulatedSchoolPacketTracerDHCP.png]]

Repeat this for each PC on the network.
	
# Configure Router
	
In Both `FastEthernet0/0` and `FastEthernet1/0`:

1. Turn the Port Status ON
2. Configure the IP address to be `x.x.x.2` for the student and staff network
![[simulatedSchoolPacketTracerRouterConfig.png]]

	
# Attempt communication
	

Once the router configuration has been set, click on one of the PCs → Desktop tab → Command Prompt and attempt to ping an IP on the other subnetwork.
![[simulatedSchoolPacketTracerPing.png]]

	
# Restrict Access
	
At this stage, the two separate subnets are completely open to each other - devices can access devices on the other network. This is not necessarily ideal, as in our example, you wouldn’t want students to access staff computers. For this security to be implemented, you would want to add a **Access Control List (ACL)**.

Click on the Router and enter the `CLI` (Command Line Interface) tab.

💡 If you don’t have the prompt `Router>`, type exit and hit return. Continue doing this until you end with that prompt.
![[simulatedSchoolPacketTracerACLCLI.png]]


Enter the following commands, pressing the enter key after each command.

```
enable
configure terminal
ip access-list standard BlockStudents
deny 192.168.2.0 0.0.0.255
permit any
exit
interface FastEthernet1/0
ip access-group BlockStudents out
exit
exit
```

**Note**: It’s important to ensure that the details are correct for *your* network. The IP Address, subnet mask and Interface details shown in this example are for the exact network created thus far. If any of the IP addresses are changed, for instance, you will need to replace the above with your specific implementation.
![[simulatedSchoolPacketTracerACLConfig.png]]


Try and ping from a PC on the student network to a PC on the staff network.

The result of the Ping should be `Destination host unreachable` which is what is intended - the student PCs **cannot** access the staff devices.

If you run the ping through the simulation, you’ll notice that the packet is blocked at the router, which is the ACL performing it’s job.
![[simulatedSchoolPacketTracerBlockedNetwork.gif]]

	
# Problem?

	
Previously all network traffic from the student network entering the staff network is blocked, however this has an unintended side-effect. If a staff device tries to ping a student device it appears the traffic is also blocked. However, when you view the operation in simulation mode, as can be seen here, you’ll notice that the ping packet reaches its destination, **but the reply is blocked**. This is correct as the reply packet is originating from the student network, meaning that the ACL is correctly implementing the rule.
![[simulatedSchoolPacketTracerNetworkReplyBlocked.gif]]

	
# Allow traffic again
	
To remove an entry in the ACL, such as `deny 192.168.2.0 0.0.0.255`, first enter the CLI for the router.

Either type configure or enter exit() commands until you are at the root # command line.

![[simulatedSchoolPacketTracerACLReturn.png]]


Once you’re at the correct level, enter the `show access-lists` command.

![[simulatedSchoolPacketTracerShowAccessLists.png]]


*In this case* the first entry needs to be removed, identified by the line `10`. 

Enter `configure terminal`

Enter `ip access-list standard BlockStudents`

![[simulatedSchoolPacketTracerBlockStudents.png]]


The command to remove entries in an ACL is `no <line number>`. In this case, the command is `no 10`. Enter that command and hit enter.

![[simulatedSchoolPacketTracerNo10.png]]

Enter `exit()` commands until you’re back at the root `#` level command line.

![[simulatedSchoolPacketTracerExit.png]]


To confirm that the command worked, enter the `show access-lists` command again.

The output should show that the ACL entry has been removed.
![[simulatedSchoolPacketTracerShowBlockStudents.png]]
