---
layout: post
title: Journey to the Kubernetes world with Digital Ocean
date: 2021-12-11 15:38:19.000000000 +00:00
type: post
background: /img/k8s-bg.png
---
<p>Hey all! It's been a long time since I haven't written a blog about Kubernetes. So I was wandering in r/devops in Reddit and saw a post where the digital ocean is hosting a <b>Kubernetes challenge</b> and guess what they're giving away free credits of <i>$120</i> to try it out free!!!</p>
  
<p>This blog is written in multiple sections from steps to apply to steps to deploy your app in Digital Ocean Kubernetes via CI/CD. Let's get started!</p>
  
<p><!-- wp:heading {"level":3} --></p>
<h3 id="challenge-and-how-to-apply"><strong>Challenge and how to apply</strong> :</h3>
  
<ol>
<li>Link of the challenge page :&nbsp;<a href="https://www.digitalocean.com/community/pages/kubernetes-challenge"><b>Here</b></a>&nbsp;</li>
<li>Pick one challenge from the list mention in above link based on your knowledge.</li>
<li>Create a GitHub or GitLab repo for your project</li>
<li>Fill out the&nbsp;<a href="https://docs.google.com/forms/d/e/1FAIpQLSdil-lIxbh7W08zourmlt2pMWP8Sn8y3u6hhAILR9eiqhy-Ww/viewform"><b>code challenge form</b></a>&nbsp;to get DigitalOcean credits for your project</li>
<li>Join the #kubernetes-challenge channel in the&nbsp;<a href="https://discord.com/invite/7AHdNue">DigitalOcean Deploy Discord</a></li>
<li>Complete your challenge</li>
<li>Write about what you’ve built and share it on a blog or in your project readme</li>
<li>Make a pull request against the&nbsp;<a href="https://github.com/do-community/kubernetes-challenge"><b>Kubernetes Challenge Github Repo</b></a>&nbsp;with information about your project&nbsp;</li>
<li>Let them know you’ve completed your challenge by&nbsp;<a href="https://docs.google.com/forms/d/e/1FAIpQLSe-CT6ynhORAL04GqsvrvYn8d_6bUJuHUsMNFRG8L9mVxE1IA/viewform"><b>filling out this form</b></a>.</li>
</ol>
 
  
<p>Now that we've applied let's take a look at one of the challenges I chose :</p>
  
  
<p><strong>Deploy a GitOps CI/CD implementation</strong><br />GitOps is today the way you automate deployment pipelines within Kubernetes itself, and&nbsp;<a href="https://argo-cd.readthedocs.io/en/stable/">ArgoCD</a>&nbsp;is currently one of the leading implementations. Install it to create a CI/CD solution, using&nbsp;<a href="https://github.com/features/actions">Github Actions </a>for actual image building.</p>
  
  
  
<p><!-- wp:heading {"level":3} --></p>
<h3 id="1-cluster-creation-setup">1. Cluster Creation &amp; setup :</h3>
   
<p><!-- wp:list --></p>
<ul>
<li>Sign in to your DO console at: <a href="https://cloud.digitalocean.com/">https://cloud.digitalocean.com/</a></li>
<li>Click on NEW button and create a Kubernetes cluster with default values.</li>
<li>You can customize the location of cluster nearest to your location to avoid altency issues to API server.</li>
<li>Once you submit, it'll take around 10-15 min for the worker nodes and API server to become ready.</li>
</ul>
 
<p><!-- wp:image {"id":338,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image.png" alt="" class="wp-image-338" /></figure>
 
<p><!-- wp:list --></p>
<ul>
<li>Click on the Actions button and download the kubeconfig file.</li>
</ul>
 
<p><!-- wp:image {"id":340,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-1.png" alt="" class="wp-image-340" /></figure>
 
<p><!-- wp:list --></p>
<ul>
<li>Once you download, install kubectl binary by following steps in the <em>Getting started</em> section of <em>overview </em>tab.</li>
<li>Once, kubectl in installed in your local, you can save / move the config file downloaded to your <code>~/.kube/config</code> location.</li>
<li>Now, you can connect to your API server, test it by running :  <code>kubectl get node -o wide</code></li>
</ul>
 
<p><!-- wp:image {"id":342,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-2.png" alt="" class="wp-image-342" /></figure>
 
<p><!-- wp:heading {"level":3} --></p>
<h3 id="2-project-setup">2. Project setup</h3>
   
<p><!-- wp:list --></p>
<ul>
<li>Clone this repository using below command :</li>
<li>g<code>it clone https://github.com/tanmay-bhat/DigitalOcean-Kubernetes-Challenge-argoCD</code></li>
<li>This project contains below:
<ul>
<li>go app</li>
<li>Dockerfile</li>
<li>github actions file ( CI)</li>
<li>Kubernetes manifest files</li>
</ul>
</li>
</ul>
 
  
<p>Let's Look mainly <a href="https://github.com/tanmay-bhat/DigitalOcean-Kubernetes-Challenge-argoCD/tree/main/kustomize/base">kustomize/base</a> :</p>
  
  
<p>Here, Deployment.yaml file contains the deployment kind YAML file.</p>
  
<p><!-- wp:image {"id":347,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-3.png" alt="" class="wp-image-347" /></figure>
 
  
<p>Notice the image registry I'm using is the <em>Digital Ocean registry</em> itself and not the mostly used Docker Hub.</p>
  
  
<p>The ImagePullSecrets has a name: Tanmay Bhat. This is the Kubernetes secret which has the DIgital OCean registry credentials which we will use to pull the image.</p>
  
  
<p>Now let's look at our Github actions config file located at <a href="https://github.com/tanmay-bhat/DigitalOcean-Kubernetes-Challenge-argoCD/tree/main/.github/workflows">github/workflows</a> :</p>

<script src="https://gist.github.com/tanmay-bhat/9d0ff8da79c334a82b0e53038ab79d05.js"></script>

  
<p>I'm gonna explain the section here since the main goal of this article is to do CI/CD :</p>
  
  
<ol>
<li>The <em>on </em>section says trigger this piepline if the chnages has been pushed to <strong>Main </strong>branch with a tag in format : vx.x.x ( i.e v1.0.0 etc)</li>
<li>On each tag push, pipeline will run 2 jobs. <strong>Build </strong>and <strong>Deploy</strong>.</li>
<li>In <em><strong>Build </strong></em>section, the steps will run on ubuntu image.</li>
<li><em>Check out code</em> step uses pre-build action <code>actions/checkout@v2</code> to clone current repository into the piepline container i.e ubuntu.</li>
<li><em>Extract Git Tag</em> is used to get the latest tag pusued to main branch and store it in th environmental variable GIT_TAG.</li>
<li>In <em>Login to Digitalocean</em>, since we need to push our build docker images to a private registry like Digital  Ocean, I'm using docker login action to auttenticate to the DO registry.</li>
<li>In <em>push image to digitalocean</em>, I'm buidling the docker image and tagging it to latest pushed tag version and pushing to my registry.</li>
<li>Next comes, the <strong><em>Deploy </em></strong>section. here again I'm using ubuntu as base image and again getting the repository from main branch and extracting tag version from the repositiry.</li>
<li>Once that is done, I'll use a tool called <strong>Kustomize </strong>to update my manifest file's docker image tag to the latest tag version.</li>
<li>If you're using helm charts only but not kustomize with Helm, you need to use Sed command and update the image tag in manifest file ( deployment.yaml).</li>
<li>Later, I'm doing the commit of latest tag edit and pushing the changes back to my repo.</li>
</ol>
 
  
<p>So, to sum up, what the exact pipeline does whenever a new tag is pushed to main branch : </p>
  
  
<ol>
<li>clone the repository, build the docker image, tag it and push it to registry.</li>
<li>update the tag in manifest file and push it back to gthe repository.</li>
</ol>
 
  
<p>You might have this question, Tanmay, this is just CI, where's CD ? well, that's the magic ArgoCD solves for us. </p>
  
<p><!-- wp:heading {"level":3} --></p>
<h3 id="3-argocd">3. ArgoCD</h3>
   
  
<p>1. Setup ArgoCD by running the below commands :</p>
  
  
<p><code>kubectl create namespace argocd </code></p>
  
  
<p><code>kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml</code></p>
  
  
<p>2. Once done, you can verify its running status by running the command </p>
  
<p><!-- wp:image {"id":354,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-4.png" alt="" class="wp-image-354" /></figure>
 
  
<p>3. Next step is to retrieve the password of argocd. For that, run : </p>
  
  
<p><code>kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d</code></p>
  
  
<p>4. By default argocd service type will be ClusterIP. That means you cant access argocd outside of your cluster. So, Let's change that to LoadBalancer by running : </p>
  
  
<p><code>kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'</code></p>
  
  
<p>5. Now, wait for couple more minutes for LoadBalancer to start in Digital Ocean and get the endpoint of it by running :</p>
  
<p><!-- wp:image {"id":356,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-5.png" alt="" class="wp-image-356" /></figure>
 
  
<p>6. Open the external IP in your browser and voila, you should see argocd UI login page. Login with username: admin &amp; password got from step 3.</p>
  
  
<h4 id="configure-argocd-for-cd">Configure ArgoCD for CD :</h4>
   
  
<p>1. Once, logged in to argoCD UI, click on <em>new app</em> and set the below values: </p>
  
    
<pre class="wp-block-syntaxhighlighter-code">application name : demo-argocd
Project : default
Sync Policy : Automatic
Repository URL : &lt;GITHUB REPO URL OF YOUR PROJECT&gt;
Rivision : HEAD
Path : kustomize/base
Destination Cluster : https://kubernetes.default.svc
Namespace : default</pre>
    
  
<p>Click create and see your app glowing in your cluster.</p>
  
<p><!-- wp:image {"id":361,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-6.png" alt="" class="wp-image-361" /></figure>
 
  
<p>the ArgoCD magic here is it watches for any new changes to your repo every 3 minutes ( default ) and new changes will be auto-applied in your cluster.</p>
  
  
<p>See the comment that says: update the tag to v1.0.12 ? that was my last commit. If a new tag is committed, here's how it's gonna update and look </p>
  
<p><!-- wp:image {"id":362,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/12/image-7.png" alt="" class="wp-image-362" /></figure>
 
  
<p>Conclusion: I found DOKS to be extreme easy to set up and straightforward. From click to integrate Registry to a Kubernetes cluster, easy cluster creation and scaling up.</p>
  
