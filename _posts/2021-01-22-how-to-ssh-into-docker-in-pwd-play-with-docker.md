---
layout: post
title: How to SSH into docker in PWD (Play With Docker)
date: 2021-01-22 10:28:28.000000000 +00:00
type: post
background: /img/posts/docker.png
permalink: "/2021/01/22/how-to-ssh-into-docker-in-pwd-play-with-docker/"
---
 
  
<p>Hey all ! For those of you who don't  know what PWD is below is short explanation :</p>
  
  
<p>So, PWD stands for Play With Docker. You can deploy &amp; learn docker at free with time limit of each instance up-to 10 Hrs!</p>
  
  
<p>For more info, go to : <a href="https://labs.play-with-docker.com/">Docker Labs</a></p>
  
  
<p>There are two ways you can access the docker instance.</p>
  
  
<ol>
<li>Use the web based console.</li>
<li>SSH into that instance.</li>
</ol>
 
  
<p>I always love to do ssh as it gives me more freedom.</p>
  
  
<p>If you go straight away and do ssh from your terminal, you will get :</p>
  
```console
lab-suse:~/.ssh # ssh ip-x-x-x-@direct.labs.play-with-docker.com
ip-x-x-x-@direct.labs.play-with-docker.com: Permission denied (publickey).
```
  
<p>Why are we getting this ? because there is no fresh key generated in your host.</p>
  
  
<p>Lets create a fresh key, run the below command :</p>
  
```sh
lab-suse:~/.ssh# <code>ssh-keygen
```
  
<p>After you complete the above command, try ssh again, it should work: </p>
  
```console
lab-suse:~/.ssh# ssh ip-x-x-x-@direct.labs.play-with-docker.com
 The authenticity of host 'direct.labs.play-with-docker.com (40.76.55.146)' can't be established.
 RSA key fingerprint is SHA256:UyqFRi42lglohSOPKn6Hh9M83Y5Ic9IQn1PTHYqOjEA.
 Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
 Warning: Permanently added 'direct.labs.play-with-docker.com,40.76.55.146' (RSA) to the list of known hosts.
 Connecting to Ip-x-x-x:8022

###########################
# WARNING!!!!                          
# This is a sandbox environment. Using personal credentials 
# is HIGHLY! discouraged. Any consequences of doing so are completely
# the user's responsibilites.
# The PWD team                                                                                                     

node1 root@192.168.0.28
```

<p>Note : if you are using any ssh applications, save that key you generated to a file and load that file in authentication section. </p>
  