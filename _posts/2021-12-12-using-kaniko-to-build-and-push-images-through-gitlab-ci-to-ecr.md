---
layout: post
title: Using Kaniko to build and push images to Gitlab-CI to ECR
date: 2021-12-12 12:28:22.000000000 +00:00
type: post
background: /img/posts/gitlab.png
---
 
<div class="wp-block-image">
<figure class="aligncenter size-large"><img src="{{ site.baseurl }}/assets/2021/12/Kaniko-Logo.png" alt="" /></figure>
</div>
 
  
<p>Though this seems like an easy straight forward task by referring to the docs, it's not trust me!</p>
  
  
<p>Until today in my Gitlab CI, I used to use aws-cli image and later install amazon-linux extras install docker and then use DIND service to build docker images through Gitlab-CI. that will change from today. I learned about the tool called Kaniko from Google which is built to simplify the docker build process without using Docker daemon hence not giving root-level privileges to the runner hence security says top-notch during the build process.</p>
  
  
<p>From Kaniko's doc :</p>
  
 
<blockquote class="wp-block-quote"><p>kaniko is a tool to build container images from a Dockerfile, inside a container or Kubernetes cluster.</p>
</blockquote>
  
<p>kaniko doesn't depend on a Docker daemon and executes each command within a Dockerfile completely in userspace. This enables building container images in environments that can't easily or securely run a Docker daemon, such as a standard Kubernetes cluster.</p>
  
  
<p>Let's see how to achieve this in our pipeline. For simplicity, I'll be using Gitlab CI in this example. You can use circle CI or GitHub actions or anything you like (a little bit of modification required ).</p>
  
  
<p>Prerequisites:</p>
  

<ol>
<li>AWS IAM credential ( Access-key and Secret-key) with ECR full access.</li>
<li>Bit of time to implement the below 😀</li>
</ol>
 

<h4 id="setup-AWS-credentials">Setup of AWS credentials :</h4>

<p>Go to CI-CD settings of your project and set the below variables with appropriate values:</p>
 
```yml
AWS_ACCESS_KEY_ID = <your access key>
AWS_SECRET_ACCESS_KEY = <your secret key>
```  
  
<h4 id="gitlab-ci">Gitlab-CI :</h4>
   
  
<p>The GitLab CI file should look like below :</p>

```yml
image: alpine
stages:
  - build_and_push

build and push docker image:
  stage: build_and_push
  only:
    variables:
        - $CI_COMMIT_TAG =~ /^v[0-9]+\.[0-9]+\.[0-9]+$/
  variables: 
    AWS_DEFAULT_REGION: REGION_NAME
    CI_REGISTRY_IMAGE: YOUR_ID.dkr.ecr.REGION_NAME.amazonaws.com/REPO_NAME
  image:
    name: gcr.io/kaniko-project/executor:debug
    entrypoint: [""]
  script:
    -  mkdir -p /kaniko/.docker
    -  echo "{\"credsStore\":\"ecr-login\"}" > /kaniko/.docker/config.json
    -  /kaniko/executor
      --context "${CI_PROJECT_DIR}"
      --dockerfile "${CI_PROJECT_DIR}/Dockerfile"
      --destination "${CI_REGISTRY_IMAGE}:${CI_COMMIT_TAG}"
```
    
  
<p>Let's look at the above pipeline in detail :</p>
  
  
<ol>
<li>We're using the base image as alpine.</li>
<li>We have one stage which is will build the Dockerfile and push to ECR using Kaniko</li>
<li>As we have mentioned <em>only</em> constraint, the pipeline will only trigger when a new tag is pushed to Master branch.</li>
<li>Next, we have 2 variables, in which we're defining the default AWS region and our Registry address of ECR. ( please update with your values)</li>
<li>Next, we're using the Kaniko base image to build run the scripts mentioned and build our image.</li>
<li>Then we're making a docker folder that will have the registry to push credentials.</li>
<li>Note that ECR regular login is a bit different than other container registries like Quay or GCR.</li>
<li>You won't get the regular username and password for this repo from AWS side. You must know that to log in to ECR, you need to run <code>aws ecr get-login</code> command which will give an authentication token that has a TTL of 12 hours, which doesn't work in our case.</li>
<li>Luckily was has created a new ECR login provider extension that will work through IAM permissions.</li>
<li>Kaniko has built-in support for that provider, so you just need to add the variable of AWS creds in GitLab CI and Kaniko will take care of the rest. ( magic !)</li>
<li>Then we provide Kaniko the path to Dockerfile which will be inside our current project, hence the use of <b>CI_PROJECT_DIR</b> which is a pre-defined variable from GitLab CI points to the current project context.</li>
<li>Then I'm tagging the image with the latest tag from the repository and pushing to the ECR.</li>
</ol>
 
  
<p>Happy CI-CDing !!!</p>
  
  
<p>Reference :</p>
  
  
<p><a href="https://github.com/GoogleContainerTools/kaniko">https://github.com/GoogleContainerTools/kaniko</a></p>
  
  
<p><a href="https://gist.github.com/tanmay-bhat/6bc6d6034644ef010d841ea8373a41d6">https://gist.github.com/tanmay-bhat/6bc6d6034644ef010d841ea8373a41d6</a></p>
  
