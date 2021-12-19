---
layout: post
title: How to install netstat tool in openSUSE 15
date: 2021-01-25 01:03:12.000000000 +00:00
type: post
background: /img/posts/terminal.jpg
permalink: "/2021/01/25/how-to-install-netstat-tool-in-opensuse-15/"
---
<p>Even though the <strong>netstat </strong>tool is depreciated, sometimes we can't stop the old habit and we arrive at a situation where its difficult to adapt to new things.</p>
  
  
<p>Actually we should be using <strong>ss </strong>tool installed of netstat !</p>
  
  
<p>All common network related tools are bundled with package net-tools.</p>
  
```
nwlab:/etc # rpm -qa | grep net-tools
net-tools-2.0+git20170221.479bb4a-lp152.5.5.x86_64
```  
  
<p>However in openSUSE 15, the team decided to knock it off from net tools package!</p>
  
  
<p><span style="text-decoration:underline;"><strong>So, the solution ?</strong></span></p>
  
<p><!-- wp:paragraph {"align":"justify"} --></p>
<p class="has-text-align-justify">Install the package net-tools-deprecated by typing:</p>
  
  
<p><code>sudo zypper install net-tools-deprecated</code></p>
  
```console
nwlab:/etc # rpm -qa | grep net-tools
net-tools-deprecated-2.0+git20170221.479bb4a-lp152.5.5.x86_64
```
  
<p>Once installed, netstat should work totally fine now !</p>
  
  
```console
nwlab:/etc # netstat -ano | grep 9000
tcp6 0 0 :::9000 :::* LISTEN off (0.00/0/0)
```
  
