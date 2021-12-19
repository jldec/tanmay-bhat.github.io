---
layout: post
title: Pulling private image from Docker hub in GitLab CI
date: 2021-08-23 20:20:33.000000000 +00:00
type: post
background: /img/posts/gitlab.png
---  
<p>Hey people ! I'm back this time with a how-to on GitLab CI to make your life easy being DevOps Engineer. I thought of  writing this since I spent hours searching and fixing this :/ </p>
  
  
<p>Lets look at the problem or the requirement. It goes like this :</p>
  
  
> I have a GitLab CI file integrated into my project which builds a Dockerfile and pushes that image into ECR. But the dockerfile has a base image which is from a private Docker hub repository. how do I pull from that repo ?
  
  
<p>Lets consider the below <code>gitlab-ci.yml</code> file :</p>
```yml  
image: "python:3.6"     
                    
stages:                                   
  - publish_image                         

build and push docker image:        
  stage: publish_image
  only:                                   
    variables:
        - $CI_COMMIT_TAG =~ /^v&#091;0-9]+\.&#091;0-9]+\.&#091;0-9]+-&#091;0-9]+\.&#091;0-9]+\.&#091;0-9]+$/ 
        - $CI_COMMIT_TAG =~ /^v&#091;0-9]+\.&#091;0-9]+\.&#091;0-9]+$/     
  variables:
    DOCKER_HOST: tcp://docker:2375
  image: 
    name: amazon/aws-cli
    entrypoint: ""
  services:
    - docker:dind 
  before_script:
    - echo "$CI_COMMIT_TAG"
    - amazon-linux-extras install docker
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
  script:
    - docker build -t $DOCKER_REGISTRY/$APP_NAME:$DOCKER_TAG .
    - aws ecr get-login-password | docker login --username AWS --password-stdin $DOCKER_REGISTRY 
    - docker push $DOCKER_REGISTRY/$APP_NAME:$CI_COMMIT_TAG
    - docker push $DOCKER_REGISTRY/$APP_NAME:$DOCKER_TAG
```
  
<p>Link for the above file : <a href="https://gist.github.com/tanmay-bhat/6fa65b9cd9d5f7f5e780dbe3efcb1fb7">Here</a></p>
  
  
<p>What the above CI file does :</p>
  
  
<ol>
<li>Uses base image <code>python</code> on which the stages will run.</li>
<li>has a single stage which will build and push images to ECR</li>
<li><em>only</em> section tells gitlab to run the stage only if the git tag is done and it matched the regex mentioned. </li>
<li>in <em>before_script</em> section, we're displaying the commit tag and installing docker in aws-cli image since that image doesn't come preinstalled with docker.</li>
<li>finally we're doing docker login to with our dockerhub account before building Dockerfile.</li>
<li>Later we build the Dockerfile and then push it to ECR</li>
</ol>
 
  
<p>Now, lets see <strong>How to login to Docker hub in GitLab CI to pull your private repository images</strong> </p>
  
  
<ol>
<li>To configure the Dockerhub credentials, go to your GitLab <em>project -&gt; settings -&gt; CI/CD</em></li>
<li>In Variables section, add the below Key and their value :</li>
</ol>
 
```sh
Key : CI_REGISTRY ||  Value : docker.io
Key : CI_REGISTRY_USER ||  Value : your_dockerhub_username
Key : CI_REGISTRY_PASSWORD || Value : your_dockerhub_password
```

<p><!-- wp:image {"id":144,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/08/image.png" alt="" class="wp-image-144" /></figure>

  
<p>Now, to setup AWS credentials, configure the below values :</p>
  
```sh
Key : AWS_ACCESS_KEY_ID || Value : your_aws_accesskey
Key : AWS_SECRET_ACCESS_KEY || Value : your_aws_secretkey
```
<p><!-- wp:image {"id":147,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/08/image-1.png" alt="" class="wp-image-147" /></figure>
 
  
<p>That's it.  Voila !! the GitLab runner should pull your docker credentials from variables and pull the image.</p>
  
