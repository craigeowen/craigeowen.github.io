---
layout: post
title: "Lab Environment"
date: 2018-04-06
---

The great thing about building labs today is that it is pretty easy to virtualise most components. 
Therefore I have set out to build this lab entirely in a VMWare ESXi host. 
At first I started out building things in Virtual Box on my laptop, but a Cisco CSR1000v requires 4Gb RAM, so this soon outgrew my laptops resources.
Luckily I have a pretty good VMWare host so it makes so much more sense to use it as I decided that this was going to be a much longer term project than I had first envisaged.<br>
The lab will be built from the ground up during the course of the various labs, this is the link to the final <a href="https://github.com/craigeowen/my-network-ansible_playbooks/blob/master/2b.%20Laptop_Lab_JPEG.jpg">lab topology drawing</a>

My VMWare hosts is spec'd as follows:<br>
Dell Precision T3600<br>
	CPU: Intel Xeon E5-1620 @ 3.6GHz (On HCL)<br>
	RAM: 32Gb<br>
	Storage: 450GB Sata<br>
VMWare: ESXi 6.5.0<br>
<br>
This easily allows me to spin up the required VM's I need for this series of labs (so far 7 CSR1000v routers and at least 5 linux endpoints). 
The Cisco routers specify 4Gb RAM, so that is what I have provisioned. When I was running this in Virtual Box I did try to provision less RAM per router, but I suffered significant stability issues. 
The VMWare CSR1000v virtual machines run at between 2.4 and 3.4 Gb RAM on average, but then I am not really taxing them in performance terms in this lab.

One of the biggest disappointments I came across during early lab trials, was the stability of ubuntu. Now don't get me wrong I like ubuntu and have used it as my linux distro of choice for many years. However for some reason it just kept freezing when  I used it as my ansible host...However it is fine for the normal lab endpoint machines. Therefore I opted to use Red Hat Enterprise 7 as Red Hat is the custodian for Ansible and also it is possible to use RHEL under a develpoer licensing model 

I will create all the VM's up front, but will introduce them into the lab when required. The VM's will be created as follows:
(Based upon this <a href="https://github.com/craigeowen/my-network-ansible_playbooks/blob/master/2b.%20Laptop_Lab_JPEG.jpg">topology</a>)<br>

<b><u>Virtual Machines</u></b> 

<b>ansible host</b><br>
RedHat1 (RHEL7)<br>
VLAN 10<br>
IP Address - 172.16.10.10/24<br>
Gateway - 172.16.10.254<br>

<b>Site1</b> - (VLAN 10)<br>
test1@test1 (Ubuntu 17.10)<br>
IP Address - 172.16.10.4/24<br>
Gateway - 172.16.10.254<br>
<br>
test10@test10 (Ubuntu 16.04 LTS)<br>
IP Address- 172.16.10.5/24<br>
Gateway - 172.16.10.254<br>
    
CSR10 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 172.16.10.252/24<br>
HSRP IP Address - 172.16.10.254<br>
GE2 IP Address - 10.10.10.10/24<br>
Lo1 - 1.0.10.1/32<br>
Lo10 - 1.0.10.10/32<br>
Lo100 - 1.0.10.100/32<br>

CSR11 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 172.16.10.253/24<br>
HSRP IP Address - 172.16.10.254<br>
GE2 IP Address - 10.10.10.11/24<br>
Lo1 - 1.0.11.1/32<br>
Lo10 - 1.0.11.10/32<br>
Lo100 - 1.0.11.100/32<br>
    
<b>Site2</b> - (VLAN 20)<br>
test2@test2 (Ubuntu 16.04 LTS)<br>
IP Address- 172.16.20.2/24<br>
Gateway - 172.16.20.254<br>
    
CSR20 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 172.16.20.254/24<br>
GE2 IP Address - 20.20.20.20/24<br>
Lo1 - 1.0.20.1/32<br>
Lo10 - 1.0.20.10/32<br>
Lo100 - 1.0.20.100/32<br>

<b>Site3</b> - (VLAN 30)<br>
test3@test3 (Ubuntu 16.04 LTS)<br>
IP Address- 172.16.30.3/24<br>
Gateway - 172.16.30.254<br>
    
CSR30 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 172.16.30.254/24<br>
GE2 IP Address - 30.30.30.30/24<br>
Lo1 - 1.0.30.1/32<br>
Lo10 - 1.0.30.10/32<br>
Lo100 - 1.0.30.100/32<br>
    
<b>WAN Routers</b><br>
WAN100 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 10.10.10.254/24<br>
GE2 IP Address - 100.20.0.10/24<br>
GE3 IP Address - 100.30.0.10/24<br>
GE4 IP Address - ###External connection 192.168.150.6/24###<br>
Lo1 - 1.10.0.1/32<br>
Lo10 - 1.10.0.10/32<br>
Lo100 - 1.10.0.100/32<br>  

WAN200 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 20.20.20.254/24<br>
GE2 IP Address - 100.20.0.20/24<br>
GE3 IP Address - 200.30.0.20/24<br>
Lo1 - 1.20.0.1/32<br>
Lo10 - 1.20.0.10/32<br>
Lo100 - 1.20.0.100/32<br>

WAN300 (Cisco CSR1000v - IoS 16.03.06)<br>
GE1 IP Address - 30.30.30.254/24<br>
GE2 IP Address - 100.30.0.30/24<br>
GE3 IP Address - 200.30.0.30/24<br>
Lo1 - 1.30.0.1/32<br>
Lo10 - 1.30.0.10/32<br>
Lo100 - 1.30.0.100/32<br>
