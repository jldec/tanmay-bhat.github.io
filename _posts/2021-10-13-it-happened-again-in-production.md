---
layout: post
title: It happened again in production !!
date: 2021-10-13 21:07:07.000000000 +00:00
type: post
background: /img/posts/egg-worry.jpg
---
  
<p>Previously I have written <a href="https://wp.me/pcknFJ-2F">article</a> about how AWS pushed broken image to Docker hub and we got screwed as we were using <em>latest</em> as image tag.</p>
  
  
<p>Welp, this happened again in our CI/CD pipeline as we were using <a href="https://github.com/chartmuseum/helm-push">push</a> plugin from helm and using that to push charts to <a href="https://chartmuseum.com/">chartmuseum</a>.</p>
  
  
<h4>How it happened?</h4>
   
  
<p>So we were using the below line to pull the helm push plugin :</p>
  
<code>
helm plugin install https://github.com/chartmuseum/helm-push.git
</code>
  
<p>And were pushing to Chartmuseum via command : </p>

<code>helm push app-name repo-name</code>
  
<p>It turns out that command is not valid and as per their latest (v0.10.0) changes to the plugin, its been renamed to <strong>cm-push</strong> and we gotta use like <code>helm cm-push app-name repo-name</code>. Else we can use the same command with old version of plugin. </p>
  
  
<p>Hence our pipeline got screwed and I've fixed by pulling specific version from their repo by using -version argument. It goes like this : </p>
  
<code>helm plugin install https://github.com/chartmuseum/helm-push.git --version v0.9.0</code>

  
<p>The better solution to this is to replace the hard-coded version above to GitLab CI variable and update the version from there later.</p>
  
