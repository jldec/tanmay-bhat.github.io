---
layout: post
title: How to install mhVTL in openSUSE
date: 2021-01-14 23:42:03.000000000 +00:00
type: post
background: '/img/posts/opensuse.jpg'
---
<p>Lets see how to install mhVTL (a FOSS VTL software) written by a super hero called : Mark Harvey.</p>
<p>There are 2 ways you can install mhvtl :</p>
<p><!-- wp:list {"ordered":true} --></p>
<ol>
<li>Install the rpm package</li>
<li>Directly compile the source code yourself.</li>
</ol>
<p><!-- /wp:list --></p>
<p>I have tried using the first method as its easy and fast :D.</p>
<p><strong>Note: I have tested this install in openSUSE 15.2</strong></p>
<p>lets get started :</p>
<p>1.First update the packages to latest version available by typing :</p>

<code> sudo zypper up </code>

<p>2. Once the packages are up to date, install the below supporting packages :</p>
<code>sudo zypper install gcc gcc-c++ kernel-devel zlib-devel mt-st mtx lzo-devel perl</code>
<p>3. Now add the repository and install the package :</p>
```sh
zypper addrepo https://download.opensuse.org/repositories/openSUSE:Leap:15.2:Update/standard/openSUSE:Leap:15.2:Update.repo
zypper refresh
zypper install mhVTL
```
<p>4. start the mhvtl service ;</p>
<p><code>service mhvtl start </code></p>
<p>5. check if mhvtl service is running :</p>
<p><code>service mhvtl status</code></p>
```console
test-machine:/home/azureuser # service mhvtl status
● mhvtl.target - mhvtl service allowing to start/stop all vtltape@.service and vtllibrary@.service instances at once
   Loaded: loaded (/usr/lib/systemd/system/mhvtl.target; disabled; vendor preset: disabled)
   Active: active since Thu 2021-01-14 17:13:36 UTC; 50min ago
```   

<p>6. verify if you are able to see the tape library and the drives configured( by default) :</p>
```console
test-machine:/home/azureuser # lsscsi -g
[1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0   /dev/sg3 
[2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda   /dev/sg0 
[3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb   /dev/sg1 
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc   /dev/sg2 
[6:0:0:0]    mediumx STK      L700             0162  /dev/sch0  /dev/sg12
[6:0:1:0]    tape    IBM      ULT3580-TD5      0162  /dev/st0   /dev/sg4 
[6:0:2:0]    tape    IBM      ULT3580-TD5      0162  /dev/st7   /dev/sg11
[6:0:3:0]    tape    IBM      ULT3580-TD4      0162  /dev/st3   /dev/sg7 
[6:0:4:0]    tape    IBM      ULT3580-TD4      0162  /dev/st4   /dev/sg8 
[6:0:8:0]    mediumx STK      L80              0162  /dev/sch1  /dev/sg13
[6:0:9:0]    tape    STK      T10000B          0162  /dev/st2   /dev/sg6 
[6:0:10:0]   tape    STK      T10000B          0162  /dev/st5   /dev/sg9 
[6:0:11:0]   tape    STK      T10000B          0162  /dev/st1   /dev/sg5 
[6:0:12:0]   tape    STK      T10000B          0162  /dev/st6   /dev/sg10
```
<p>For more details about mhvtl, please refer the below link :</p>
<p><a href="https://sites.google.com/site/linuxvtl2/">https://sites.google.com/site/linuxvtl2/</a></p>
