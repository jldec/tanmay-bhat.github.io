---
layout: post
title: Hosting the Chartmuseum in Digital Ocean Spaces
date: 2021-12-12 21:26:48.000000000 +00:00
type: post
background: /img/do-clean.jpg
---
![image info](/img/posts/vlz-1.png)

<p>Most DevOps engineers who use Chartmuseum to store/host their helm charts use S3 as their storage medium. Well, I wanted to try Digital Ocean spaces as its S3 compatible storage option.</p>
  
  
<p>Well, there's an obvious reason why to use S3 in the first place. Beautiful integration with AWS other services, cheap, easy to access, versioning, MFA delete protection etc.</p>
  
  
<p>However, if you're an early developer / DevOps engineer or in a small startup who doesn't wanna go through 1000 configurations in AWS just to create one single storage bucket in the cloud and again go through 1000 more security hurdles in case you want this bucket to be public, you should use DO Spaces. I'll list down why :</p>
  
  
<ol>
<li>Super easy to set up.</li>
<li>Damn cheap. $5 for 250GB storage.</li>
<li>One-click public/private button.</li>
<li>Easy to integrate CDN if you have any.</li>
</ol>
 
  
<p>That being said, Let's look at how we can utilize Spaces as an AWS S3 replacement to host our helm charts.</p>
  
<h2 id="create-spaces">Create Spaces :</h2>
   
<ul>
<li>Log in to your console and click on Spaces and select <em>Create spaces for $5</em>.</li>
<li>The name has to be unique and make the <em>permission</em> (<strong>File Listing)</strong> private.</li>
</ul>
 
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/img/posts/d1.png?w=1024" alt="" class="wp-image-401" /></figure>
 
 
<ul>
<li>Once done, you should have the endpoint of the spaces created like :</li>
</ul>
 
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/img/posts/d2.png?w=1024" alt="" class="wp-image-402" /></figure>
 
 
<ul>
<li>Now, go to API settings and create Access keys for Spaces.</li>
<li>Please note that the <em>secret key</em> will be displayed only once and hence keep it safe copied somewhere.</li>
<li>With that said, you're ready to move to the next step.</li>
</ul>
 
  <hr class="wp-block-separator" />

  
<h4 id="install-chartmuseum">Install Chartmuseum</h4>
   
 
<ul>
<li>Add helm repo :</li>
</ul>
 
  
<p><code>helm repo add chartmuseum https://chartmuseum.github.io/charts</code></p>
  
 
<ul>
<li>Update the repo :</li>
</ul>
 
  
<p>           <code>helm repo update</code></p>
  
 
<ul>
<li>Here, we can just run helm install since we need to tell chartmuseum to use Space as a holy place to store charts instead of local PVC ( default).</li>
<li>Download the chart to your local system by running :</li>
</ul>
 
  
<p><code>helm pull chartmuseum/chartmuseum —untar</code></p>
<p></p>
Update the below fields :
<p></p>
<pre>
STORAGE: "amazon"
STORAGE_AMAZON_BUCKET: < your space name >
STORAGE_AMAZON_REGION: < REGION in which your space is created>
STORAGE_AMAZON_ENDPOINT: "https://REGION_NAME.digitaloceanspaces.com"
#enable API to interact with endpoint 
DISABLE_API: false
#secret section
BASIC_AUTH_USER: admin  
# password for basic http authentication
BASIC_AUTH_PASS: secret_password123
#Spaces access key
AWS_ACCESS_KEY_ID: "YOUR KEY"
#spacess secret key
AWS_SECRET_ACCESS_KEY: "YOUR SECRET KEY"
</pre>
  
<p>That's it, run the below command to install the chart :</p>
<p><code>helm install chartmuseum chartmuseum/ -f chartmuseum/values.yaml</code></p>
<p> Next, add an entry in ingress to point to your Repository URL. For example :</p>
  
```yml   
host: chartmuseum.tanmaybhat.tk
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: chartmuseum
                port:
                  number: 80
```    
  
<p>Please note that you need to set <em>False </em>for Key <em>Disable_API</em>,else you cant send any data to your endpoint.</p>
  
  
<p>That's it, you can now add your repo to your helm by typing :</p>
  
  
<p><code>helm repo add chartmuseum &lt;chartmuseum.example.com&gt; -u USERNAME -p PASSWORD</code></p>
  
  
<p>If you want to push chart to this repo, you can do that by installing the helm push plugin:</p>
  
  
<p><code>helm plugin install https://github.com/chartmuseum/helm-push.git --version v0.9.0</code></p>
  
  
<p>Once installed, push the chart by running :</p>
  
  
<p><code>helm push chart_directory chartmuseum</code></p>
  
  
<p>Happy Helming !</p>
  
---
   
  
<p>References :</p>
  
  
[Artifact Hub](https://artifacthub.io/packages/helm/chartmuseum/chartmuseum)
  
  
[Chart Museum](https://chartmuseum.com)
  
