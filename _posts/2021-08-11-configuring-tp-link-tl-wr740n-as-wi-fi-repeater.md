---
layout: post
title: Configuring TP -Link TL-WR740N as WI-FI repeater
date: 2021-08-11 00:36:50.000000000 +00:00
type: post
permalink: "/2021/08/11/configuring-tp-link-tl-wr740n-as-wi-fi-repeater/"
background: /img/posts/router.jpg
---  
<p>Hey people, in this article, we'll see how to configure TP -Link <a href="https://www.tp-link.com/in/home-networking/wifi-router/tl-wr740n/">TL-WR740N</a> (preferably old one) as repeater to extend your main WI-FI signal in your house.</p>
  
  
<p>Lets get into basics real quick.</p>
  
  
<p><strong>What's a repeater ?</strong></p>
  
  
<p>Definition : A WiFi repeater or extender is used&nbsp;<strong>to extend the coverage area of your Wi-Fi network</strong>. It works by receiving your existing Wi-Fi signal, amplifying it and then transmitting the boosted signal.</p>
  
  
<p><strong>Prerequisites :</strong></p>
  
  
<ol>
<li>Main / primary router</li>
<li>Secondary / old router </li>
<li>This article to help you out :)</li>
</ol>
 
  
<p><strong>Steps  on secondary router :</strong></p>
  
  
<ol>
<li>Do a factory reset of your secondary router. You can refer <a href="https://youtu.be/4AkkPRE9ZBM">this</a> video for how-to steps.</li>
<li>Once the router is up and running, connect to it wirelessly / through LAN cable.</li>
<li>Go to admin console by typing this IP address in browser URL : <code>192.168.0.1</code> with credentials , username : <code>admin </code>&amp; password : <code>admin </code> ( super secure :D )</li>
<li>Lets first change the IP address of this router to something else rather than the default one as later this IP can cause IP allocation conflict due to DHCP set in primary router.</li>
<li>To to that , lets go to Network -&gt; LAN -&gt; IP address and change it to something like <code>192.168.1.100</code> .and hit <code>Save</code>. ( you can change it to almost any IP you like in this subnet) </li>
</ol>
 
<p><!-- wp:image {"id":127,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/08/ip_change.png" alt="" class="wp-image-127" /></figure>
 
  
<p>6. Do a <em>reboot </em>of the router and connect back to router console using the new IP in browser URL i.e. in my case  <code style="font-size:1em;color:var(--darkreader-text--wp--preset--color--foreground);">192.168.1.100</code><span style="font-size:1em;background-color:var(--darkreader-bg--wp--preset--color--background);color:var(--darkreader-text--wp--preset--color--foreground);font-family:var(--font-base, &quot;PT Sans&quot;, -apple-system, BlinkMacSystemFont, &quot;Segoe UI&quot;, &quot;Roboto&quot;, &quot;Oxygen&quot;, &quot;Ubuntu&quot;, &quot;Cantarell&quot;, &quot;Fira Sans&quot;, &quot;Droid Sans&quot;, &quot;Helvetica Neue&quot;, sans-serif);"> </span> or the IP given by you.</p>
  
  
<p>7. Lets configure the repeater mode. To do that, go to <code>Wireless-&gt; Enable WDS Bridging</code>.</p>
  
  
<p>8. Click on Survey and select the WIFI name which you want to repeat.</p>
  
  
<p>9. Type the password for that in <strong>Password</strong> field and hit <code>Save</code>. Later you may get alert on switching the repeater to be in same Wi-Fi  channel as main router, select ok to that pop-up.</p>
  
<p><!-- wp:image {"id":132,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/08/wds.png" alt="" class="wp-image-132" /></figure>
 
  
<p>10. Next thing would be to setup DHCP of the router.</p>
  
  
<p>I'll explain a bit here regarding the <em>problem </em>I faced.  According to YouTube tutorials and articles out there, we need to disable the DHCP option in secondary router.</p>
  
  
<p>What I faced after that is <em>I cant connect any device to that router later as </em>DHCP is disabled, the router wont be able to assign any IP address to any device asking for connection. So your device will be struck in "Obtaining IP address".</p>
  
  
<p>So I found out the below trick and its working brilliantly for me.</p>
  
<p><!-- wp:image {"id":130,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/08/dhcp.png" alt="" class="wp-image-130" /><br />
<figcaption>DHCP setting</figcaption>
</figure>
 
  
<p>11. Ok, lets through the settings one by one, </p>
  
  
<p><strong>DHCP Server</strong> : Keep it <code>Enable</code></p>
  
  
<p><strong>Start IP Address:</strong> Enter : <code>192.168.1.101</code> OR the +1 IP of the assigned IP to your router. i.e</p>
  
  
<p>If you gave <code>192.168.1.10</code> to your router, mention here <code>192.168.1.11</code></p>
  
  
<p><strong>End IP Address:</strong> : Enter  <code>192.168.1.199</code> or the IP range limit you need. I mentioned here 98 (199 - 101) Address limit assuming my number of devices wont exceed 98 devices :D </p>
  
  
<p>[ Follow the start IP address logic if you mentioned any alternate IP address to router. ]</p>
  
  
<p><strong>Address Lease Time:</strong> Keep the default value.</p>
  
  
<p><strong>Default Gateway:</strong> Here, enter the IP address of your primary router. You can mostly find out by seeing the backside of your primary router.</p>
  
  
<p>Else, you can run the below command via cmd to get the value ( after connecting to primary router)  : </p>
  
  
<p><code>ipconfig /all | findstr Gateway<br />Default Gateway . . . . . . . . . : 192.168.1.1</code></p>
  
  
<p><strong>Default Domain:</strong> Keep the default value i.e. empty.</p>
  
  
<p><strong>Primary DNS:</strong> You can mention the DNS resolver address. This is optional  and same for below one also. if none is mentioned, DNS resolver given by ISP is used. which is not a good solution from privacy perspective. You can use Google Public DNS ( 8.8.8.8) , Quad DNS,(9.9.9.9) Cloud flare DNS (1.1.1.1) here.</p>
  
  
<p><strong>Secondary DNS:</strong> This value corresponds to what resolver to use if the request is not resolved by the first DNS. Its good to mention different service to ensure high reliability.</p>
  
  
<p>That's it. hit <code>Save </code>and do a reboot of the server to get new changes.</p>
  
  
<p><strong>Note </strong>: there's a high change you wont be able to connect to your repeater later if you're in a Wi-Fi crowded place i.e. you are surrounded by lot of WIFI. When there are lot of Wifi nearby the router tries to get to channel which is less crowded.</p>
  
  
<p>But in repeater mode, both repeater and main router needs to be in same Wi-Fi channel. So I would highly advice you to go to your primary router <strong>set the Wi-Fi to a particular channel</strong> and keep the same channel in repeater also. rather than the default setting : <code>Auto</code>.</p>
  
