Now you should have the Linux hosts set up and working (1 x Ansible node and 4 x Site endpoints), connected to the VM Network port group.
So we are ready to provision the first router. For that I am going to use Cisco CSR1000v, as it can be spun up directly into VMWare esxi.
The ova is avialable from Cisco, but in order to download it you need Cisco partner cco login privileges.
The gotcha with the CSR router is that it requires 4Gb RAM, but so far these have been very stable in my virtualised lab.

Natively ansible connects to a host on ssh and then utilises the python interpreter to compile and run the plays. 
However this is not possible on a Cisco router so on the one hand it is less elegent and has a more limited set of modules, 
on the other I think it makes for more streamlined and easier playbooks.
But it does mean a more hands-on approach to the intial router setup...before we can get to work using Ansible with the routers.


<-- I am writing about network automation. but there is definitely a need
to use end hosts. So given that this entire lab is virtualised it is
easy to spin up end nodes as test targets. So this lab will prepare the groundwork by creating the linux machine hosting the ansible applicaiton, and the site linux endpoints...please refer to last weeks blog for details of the <a href="https://craigeowen.github.io/blog/2018/04/06/Lab-environment">lab environment</a><br>
The <a href="https://craigeowen.github.io/ansible/">Labs</a> page <b>Resource</b> section has links to the resource files used throughout the various lab, or alternatively go directly to <a href="https://github.com/craigeowen/my-network-ansible_playbooks">my-network-ansible_playbook</a> GitHub repository.
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
   <br>
<!-- screenshot to add above -->
<!-- <img src="img_girl.jpg" alt="Girl in a jacket" style="width:500px;height:600px;"> -->
<img src="/assets/lab1-hostsfile.PNG" alt="My hosts.yml Inventory File" style="width:400px;height:150px;">
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
<font size="5" color="blue">Below is an example of the 5 step "Procedure to confirm ansible is working correctly".</font>
<blockquote>
<b>1. Ping host</b><br>
<img src="/assets/lab1-ping.PNG" alt="Ping ubuntu endpoint" style="width:400px;height:150px;">
<br>
<b>2. SSH to host</b><br>
<img src="/assets/lab1-ssh.PNG" alt="SSH to ubuntu endpoint" style="width:400px;height:150px;">
<br>
<b>3. Return to Ansible host</b><br>
<img src="/assets/lab1-exit.PNG" alt="Return back to Ansible host" style="width:400px;height:150px;">
<br>
<b>4. Now change directory to /etc/ansible</b><br>
<img src="/assets/lab1-cd_to_ansible.PNG" alt="CD to /etc/ansible" style="width:400px;height:150px;">
<br>
<b>5. Run ansible ping</b><br>
<img src="/assets/lab1-ansible_ping.PNG" alt="Ansible Pingt" style="width:400px;height:150px;">
</blockquote>
<b><font size="10" color="blue">Success!!!</font></b>
<br> -->

