---
layout: post
title: Story of keeping CI pipeline from getting screwed when AWS pushes broken docker image to Docker hub
date: 2021-09-19 20:15:04.000000000 +00:00
type: post
background: /img/posts/aws-fire.jpg
permalink: "/2021/09/19/story-of-keeping-ci-pipeline-from-getting-screwed-when-aws-pushes-broken-docker-image-to-docker-hub/"
---  
<p>If you're using aws-cli docker image in your CI pipeline then this story could be useful &amp; amusing for you.</p>
  
  
<p>On Thursday, I started receiving alerts that our CI pipeline is failing.</p>
  
  
<p>I started checking the failed job error and it pointed out to docker is unable to install killing the pipeline.</p>
  
<p><!-- wp:code --></p>
<pre class="wp-block-code"><code>Installing docker
Installation failed. Check that you have permissions to install.
Cleaning up file based variables
ERROR: Job failed: command terminated with exit code 1</code></pre>
<p><!-- /wp:code --></p>
  
<p>After scratching the head for sometime, I found that the latest <code>aws-cli </code>image from amazon Docker hub repository is causing the issue as I haven't changed anything else in the CI file in few weeks.</p>
  
  
<p>So I went to Docker hub and I saw on that day there was a new version pushed which was <b>2.2.39</b> tagged as <em>latest</em>. Since in our CI file, we didn't mention specific image version to pull so it always assumes the tag to pull is <em>latest</em>.</p>
  
  
<p>As a temporary fix, I changed the image version to older one which was 2.2.38 and it worked fine.</p>
  
  
<p>If you ask me for a better a better solution, it would be always good to use a <b>specific version</b> in production since you know it will work for sure instead of using <em>latest</em> tag which could change every single day.</p>
  
  
<p>Else push that image to your private container repositories like ECR and pull from there.</p>
  
  
<p>I'm pretty sure AWS broke few thousand CI pipelines over the world whoever used latest as the image tag :D</p>
  
<p><!-- wp:separator --></p>
<hr class="wp-block-separator" />
<!-- /wp:separator --></p>
  
<p>To give an idea about how to install docker inside <code>aws-cli</code> image, you can just run the below command which should install docker from AWS hosted repo for a faster install :</p>
  
  
<p><code>amazon-linux-extras install docker</code></p>
  
<p><!-- wp:separator --></p>
<hr class="wp-block-separator" />
<!-- /wp:separator --></p>
  
<p> DevOps story ends here. I'll update more stories like this in future :) </p>
  
  
  
  
  
