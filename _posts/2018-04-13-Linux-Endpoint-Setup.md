I am writing about network automation. but there is definitely a need
to use end hosts. So given that this entire lab is virtualised it is
easy to spin up end nodes as test targets. So this lab will prepare the groundwork by creating the linux machine hosting the ansible applicaiton, and the site linux endpoints...please refer to last weeks blog for details of the <a href="https://craigeowen.github.io/blog/2018/04/06/Lab-environment">lab environment</a><br>
<font size="5" color="blue">Linux Initial Setup</font>
<blockquote>
   <b>Ansible admin node</b><br>
   I started out by running ansible on an ubuntu linux distro. However I
   suffered endlessly from stability issues so switched to using RHEL7.
   This I sourced from RedHat using the free 
   <a href="https://developers.redhat.com/blog/2016/03/31/no-cost-rhel-developer-subscription-now-available/">RHEL Developers 
   subscription</a><br>
   <br>
   Follow the install guide and make sure to add the dependent repos.
   <br>
   I installed the following packages onto this node:
   <font size="3">
   <li>ansible - as this is the machine that will run ansible!</li>
   <li>putty - ssh client</li>
   <li>TeamViewer - allow remote control of the machine at a later date</li>
   </font>
   <br>
   <b>Ubuntu - site nodes</b> (as I had already deployed these)<br>
   Standard ubuntu install running either 16.04LTS or 17.10, <b>please make sure ssh is set up!</b><br>
   <br>
   I made sure to start out with all the linux nodes connected to the
   VM Network port group in order to carry out updates and quickly test
   ssh works between the ansible admin node and each of the site nodes. <br>
   <b>Once again! It is really important that ssh is working correctly as ansible will only communicate over ssh!!!</b><br>
</blockquote>
 
Now that these linux machines are set up and communicating on the VM Network Port Group, we can test that Ansible is working by doing some very basic tests. So it is time to create a first basic inventory file to allow basic testing between the RHEL7 ansible host machine and the ubuntu endpoints, as follows:<br>
<font size="5" color="blue">Create a host.yml file</font>
<blockquote>
<font color="red">"<i>ip address of host</i>"</font> ansible_user=<font color="red">"<i>username</i>"</font> ansible_password=<font color="red">"<i>password</i>"</font><br>
<b>change italic values between <font color="red">""</font> to your own settings</b><br>
<br>  
<i> Note: I will go into ansible in much more detial in future blog posts, so at this stage add an entry into the host file as follows(1 entry per server).</i><br>
<br> 
<!-- I should add a screeshot of hosts.yml -->
Example from my inventory (hosts.yml)<br>
<br>
172.16.20.2 ansible_user=test2 ansible_ssh_pass=p4ssw0rd!<br>
<!-- screenshot to add above -->
</blockquote>
<!-- blank -->
<font size="5" color="blue">Procedure to confirm ansible is working correctly</font>
<blockquote>
1. First ping to ip address to confirm connectivity
<br>    
2. Then ssh to linux host with username from above
<br>    
3. Return to Ansible host
<br>    
4. cd to /etc/ansible
<br>    
5. Then from the ansible host cli run the following: ansible all -m ping
</blockquote>
<br>
<font size="5" color="blue">Below is an example of the 5 step "Procedure to confirm ansible is working correctly".</font><br>

<b>1. Ping host</b><br>
[red1@localhost ~]$ ping 172.16.20.2<br>
PING 172.16.20.2 (172.16.20.2) 56(84) bytes of data.<br>
64 bytes from 172.16.20.2: icmp_seq=1 ttl=60 time=1.00 ms<br>
64 bytes from 172.16.20.2: icmp_seq=2 ttl=60 time=0.892 ms<br>
64 bytes from 172.16.20.2: icmp_seq=3 ttl=60 time=0.781 ms<br>
^C<br>
--- 172.16.20.2 ping statistics ---<br>
3 packets transmitted, 3 received, 0% packet loss, time 2001ms<br>
rtt min/avg/max/mdev = 0.781/0.893/1.008/0.098 ms<br>
<br>
<b>2. SSH to host</b><br>
[red1@localhost ~]$ ssh test2@172.16.20.2<br>
The authenticity of host '172.16.20.2 (172.16.20.2)' can't be established.<br>
ECDSA key fingerprint is SHA256:F3Zjwy3FUe0z2+6bck9Kr/SVWDL6UlDR60TVYdNkWTU.<br>
ECDSA key fingerprint is MD5:0f:22:41:33:50:ff:10:2f:5d:c8:5a:fe:02:af:ca:14.<br>
Are you sure you want to continue connecting (yes/no)? yes<br>
Warning: Permanently added '172.16.20.2' (ECDSA) to the list of known hosts.<br>
test2@172.16.20.2's password: <br>
Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-38-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

81 packages can be updated.<br>
0 updates are security updates.

Last login: Wed Apr 18 08:49:54 2018 from 172.16.10.20<br>
<br>
<b>3. Return to Ansible host</b><br>
test2@test2:~$ exit<br>
logout<br>
Connection to 172.16.20.2 closed.<br>
[red1@localhost ~]$ <br>
[red1@localhost ~]$ <br>
<br>
<b>4. Now change directory to /etc/ansible</b><br>
<br>
[red1@localhost ~]$ cd /etc/ansible<br>
<br>
<b>5. Run ansible ping</b><br>
<br>
[red1@localhost ansible]$ ansible all -m ping<br>
172.16.20.2 | SUCCESS => {<br>
    "changed": false, <br>
    "ping": "pong"
}<br>
[red1@localhost ansible]$<br> 
[red1@localhost ansible]$ <br>
<br>
<b><font size="10" color="blue">Success!!!</font></b>
<br>
