Now you should have the Linux hosts set up and working (1 x Ansible node and 4 x Site endpoints), connected to the VM Network port group.
So we are ready to provision the first router. For that I am going to use Cisco CSR1000v, as it can be spun up directly into VMWare esxi.
The ova is avialable from Cisco, but in order to download it you need Cisco partner cco login privileges.
The gotcha with the CSR router is that it requires 4Gb RAM, but so far these have been very stable in my virtualised lab.

Natively ansible connects to a host on ssh and then utilises the python interpreter to compile and run the plays. 
However this is not possible on a Cisco router so on the one hand it is less elegent and has a more limited set of modules, 
on the other I think it makes for more streamlined and easier playbooks.
But it does mean a more hands-on approach to the intial router setup...before we can get to work using Ansible with the routers.
<font size="5" color="blue">Provisioning the CSR1000v in esxi</font>
<blockquote>
   Link to <a href="https://www.cisco.com/c/en/us/td/docs/routers/csr1000/software/configuration/b_CSR1000v_Configuration_Guide/b_CSR1000v_Configuration_Guide_chapter_011.pdf">Cisco CSR1000v Install Guide for esxi Environment</a>
</blockquote>
 In order to set up the router with the minimum configuraiton to allow ssh communicaiton from the Ansible Host machine, the configuration below must be applied to each CSR1000v router.<br>
My appraoch to provisioning the routers is to do them on a site by site basis as follows:
<li>Site 1</li>
<li>WAN100</li>
<li>WAN200</li>
<li>Site 2</li>
<li>WAN300</li>
<li>Site 3</li>
So for this lab I will be provisioning and testing CSR10 and CSR11.
<br>
<font size="5" color="blue">CSR1000v Base configuration</font>
<blockquote>
console
 - configure hostname<br>
   router(config)# hostname <font color="red">"<i>csr10</i>"</font><br>
 - configure domain<br>
   csr10(config)# ip domain-name <font color="red">"<i>test.com</i>"</font><br>
 - configure username and password<br>
   csr10(config)#username "cisco" privilege 15 secret <font color="red">"<i>p4ssw0rd!</i>"</font><br>
 - configure the enable secret password<br>
   csr10(config)#enable secret <font color="red">"<i>p4ssw0rd!!!</i>"</font><br>
 - configure the ethernet interface<br>
   csr10(config)#interface gigabitethernet1<br>
   csr10(config-if)#ip address <font color="red">"<i>x.x.x.x 255.x.x.x</i>"</font><br>
   csr10(config-if)# no shut<br>
 - configre rsa crypto key<br>
   csr10(config)#crypto key generate rsa<br>
 - configure ssh access to vty<br>
   csr10(config)#line vty 0 4<br>
   csr10(config-line)#transport input ssh<br>
   csr10(config-line)#login local<br>
 - ssh<br>
   csr10(config)#ip ssh version 2<br>
   csr10(config)#ip ssh authentication-retries 2<br>
   csr10(config)#ip ssh time-out 60<br>
   <br>
<b>change italic values between <font color="red">""</font> to your own settings</b><br>
</blockquote>
<!-- blank -->
The final desing has the RHEL7 ansible host in the VLAN10 port group together with CSR10 & 11. So lets move the RHEL7 machine into the VLAN10 port Group. So now lets test connectivity.
<br>
<font size="5" color="blue">Procedure to confirm Cisco connectivity</font>
<blockquote>
1. First ping to ip address to confirm connectivity<br>
<img src="/assets/lab2-ping.PNG" alt="Router Ping" style="width:400px;height:150px;">
<br>
2. Then ssh to the routers<br>
<img src="/assets/lab2-ssh.PNG" alt="Router SSH" style="width:400px;height:150px;">     
</blockquote>
Once ssh access works, then the router is ready.

We will start with Site 1 and build out, using ansible as much 
as possible as we go. Sometimes there will be a need to log into the 
router instance via VMWare console, but I will minimise this as much 
as possible.

