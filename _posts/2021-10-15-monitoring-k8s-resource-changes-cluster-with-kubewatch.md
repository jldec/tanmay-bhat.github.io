---
layout: post
title: Monitoring K8S resource changes with kubewatch
date: 2021-10-15 21:01:04.000000000 +00:00
type: post
background: /img/k8s-bg.png
---
<p>But what's kubewatch ?</p>
  
  
<p><a href="https://github.com/bitnami-labs/kubewatch">kubewatch</a>&nbsp;is a Kubernetes watcher that currently publishes notification to Slack. Deploy it in your k8s cluster, and you will get event notifications in a slack channel.</p>
  
  
<p>Lets see how we can deploy it to our cluster.</p>
  
  
<p><strong>Pre-requisites :</strong></p>
  
<p><!-- wp:list --></p>
<ul>
<li>Kubernetes 1.12+</li>
<li>Helm 3.1.0</li>
<li>A slack bot and a slack channel to integrate the bot</li>
</ul>
 
  
<ol>
<li>Add Bitnami repo to your helm :</li>
</ol>
 
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
```
  
<p> 2. Verify that kubewatch chart is available in the repo :</p>
  
```console
demo> helm search repo kubewatch
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/kubewatch       3.2.16          0.1.0           Kubewatch </code></pre>
```
  
<p>3. Customize the values like slack integration and enabling RBAC. If you directly do <em>helm install chart-name </em>you wont get any event notification as RBAC is set to <em>false </em>by default. </p>
  
  
<p><code>helm show values bitnami/kubewatch &gt; updated-values.yaml</code></p>
  
  
<p>Now edit the yaml file as per your requirement.  Here's what I've changed :</p>
  
  
<p><strong>Slack Integration :</strong></p>

```sh  
slack:
enabled: true
channel: "kubewatch"           #your slack channel name
## Create using: https://my.slack.com/services/new/bot and invite the bot to your channel using: /join @botname
##
token: "your slack bot token here"
```
  
<p><strong>RBAC </strong>:</p>

```yml
## @section RBAC parameters
## @param rbac.create Whether to create &amp; use RBAC resources or not
##
rbac:
  create: true
```

<p>3. Now lets deploy using the below command :</p>

```sh
 helm install kubewatch bitnami/kubewatch -f .\updated-values.yaml
```
  
<p>4. Verify that kubewatch pod is running :</p>
  
```console
demo> kubectl get pod
  
NAME       READY STATUS RESTARTS AGE
kubewatch-c86656645-8znwk 1/1 Running 0 2m19s
```

<p>5. To test it out, lets create a nginx deployment with command :</p>
  
  
<p><code>kubectl create deploy nginx --image=nginx</code></p>
  
  
<p>6. Check your slack channel for notifications :</p>
  
<p><!-- wp:image {"id":227,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/10/image-2.png?w=911" alt="" class="wp-image-227" /></figure>
 
<p><!-- wp:separator --></p>
<hr class="wp-block-separator" />
<!-- /wp:separator --></p>
  
<p>The indication is as follows :</p>
  
<p><!-- wp:list --></p>
<ul>
<li>Green : for resources created</li>
<li>Yellow : for resources updated</li>
<li>Red : for resources deleted</li>
</ul>
 
  
<p>You can customize the notification a lot, for example, which namespace to monitor to ( default value is all namespace) , which resource to monitor to like deployment, pod, PV, service etc.</p>
  
  
<p>You can just edit the configmap : <strong>kubewatch-config</strong> and change the resources to monitor.</p>
  
  
<p>Happy monitoring !!!</p>
  
  
  
  
  
