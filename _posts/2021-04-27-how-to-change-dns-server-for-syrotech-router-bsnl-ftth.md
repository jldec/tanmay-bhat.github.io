---
layout: post
title: How to change DNS server for Syrotech Router [BSNL FTTH]
date: 2021-04-27 18:26:11.000000000 +00:00
type: post
background: /img/posts/fiber.jpg
permalink: "/2021/04/27/how-to-change-dns-server-for-syrotech-router-bsnl-ftth/"
---
  
<p>Ok, to be honest, I searched a lot on the internet to change ISP DNS servers to 3rd party servers (which you should !) for my router and couldn't find a direct article / steps to do that. Hence, this article.</p>
  
  
<p><strong>Steps to change the ISP DNS to 3rd party DNS :</strong></p>
  
  
<ol>
<li>Open the router login page, which is mostly : <strong><a href="http://192.168.1.1">192.168.1.1</a> </strong>in your case.</li>
<li>After logging in, navigate to Network page,  LAN IP Address tab.</li>
<li>Change the <em>Lan Dns Mode</em> to : <strong>static</strong></li>
<li>Set the primary and secondary DNS address and click on Save/Apply.</li>
<li>Perform a reboot of router to apply the changes.</li>
</ol>
 
<p><!-- wp:image {"id":104,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/04/screenshot-2021-04-27-180654.png" alt="" class="wp-image-104" /><br />
<figcaption>Config details</figcaption>
</figure>
 
  
<p>There are a lot of DNS providers out there most of them for free. However, please be wise while choosing them.</p>
  
  
<p>I have chosen <span style="text-decoration:underline;">1.1.1.1 </span>DNS as my primary server which is provided by Cloudflare.</p>
  
  
<p>I have set the secondary  server to <span style="text-decoration:underline;">8.8.8.8 </span> which is provided by Google so that if one of the service is down, it will fallback to another.</p>
  
  
<p>List of some of the best DNS providers list :
<a href="https://www.reddit.com/r/sysadmin/comments/976aj2/updated_list_of_public_dns_resolvers_curated_by">r/sysadmin</a>
</p>
  
  
  
  
<p>Psst...... <strong>Feeling Geeky ?</strong></p>
  
  
<p>Perform DNS benchmark tests : <a href="https://www.grc.com/dns/benchmark.htm">https://www.grc.com/dns/benchmark.htm</a></p>
  
  
<p>Cloudflare DNS validation test : <a href="https://1.1.1.1/help">https://1.1.1.1/help</a></p>
  
  
<p> Need more? Read the detailed guide for BSNL FTTH : <a href="https://www.reddit.com/r/bsnl/comments/ht37q4/guide_for_bsnl_ftth/">Fiber optimization</a></p>
  
  
<p>Worth reading : <a href="https://www.howtogeek.com/664608/why-you-shouldnt-be-using-your-isps-default-dns-server/">3rd party DNS resolvers.</a></p>
  
