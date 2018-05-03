I am writing about network automation. but there is definitely a need
to use end hosts. So given that this entire lab is virtualised it is
easy to spin up end nodes as test targets. 

<b>Ansible admin node</b><br>
I started out by running ansible on an ubuntu linux distro. However I
suffered endlessly from stability issues so switched to using RHEL7.
This I sourced from RedHat using the free 
<a href="https://developers.redhat.com/blog/2016/03/31/no-cost-rhel-developer-subscription-now-available/">RHEL Developers 
subscription</a>
<br>
Follow the install guide and make sure to add the dependent repos
   
I also installed the following packages onto this node:
<li>ansible</li>
<li>putty</li>
<li>TeamViewer</li>
    
<b>Ubuntu</b> - site nodes (as I had already deployed these)<br>
Standard ubuntu install running either 16.04LTS or 17.10, <br>
please make sure ssh is set up.

I made sure to start out with all the linux nodes connected to the
VM Network port group in order to carry out updates as well as ensuring
ssh works between the ansible admin node and each of the site nodes. <br>
<b>This is really important as ansible will only work over ssh.</b>
 
 It is also a really good idea to test ansible against each node, as
 follows:
 
 <i> Note: I will go into ansible in much more detial in blog posts, so at this stage 
 add an entry into the host file as follows (1 per server).</i>
 
 "<i>ip address of host</i>" ansible_user="<i>username</i>" ansible_password="<i>password</i>"<br>
   
 <i>change valuses between "" to your own settings</i><br>
   
 <br>   
 172.16.20.2 ansible_user=test2 ansible_ssh_pass=p4ssw0rd!<br><br>
   
    First ping to ip address to confirm connectivity<br>
    
    Then ssh to linux host with username from above<br>
    
    Return to Ansible host<br>
    
    cd to /etc/ansible<br>
    
    Then run the following: ansible all -m ping<br>

<b>Example</b><br>

<b>Ping host</b><br>
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
<b>SSH to host</b><br>
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
<b>Return to Ansible host</b><br>
test2@test2:~$ exit<br>
logout<br>
Connection to 172.16.20.2 closed.<br>
[red1@localhost ~]$ <br>
[red1@localhost ~]$ <br>
<br>
<b> Now change directory to /etc/ansible</b><br>
<br>
[red1@localhost ~]$ cd /etc/ansible<br>
<br>
<b>Run ansible ping</b><br>
<br>
[red1@localhost ansible]$ ansible all -m ping<br>
172.16.20.2 | SUCCESS => {<br>
    "changed": false, <br>
    "ping": "pong"
}<br>
[red1@localhost ansible]$<br> 
[red1@localhost ansible]$ <br>
<br>
<b>Success!!!</b>
<br>
