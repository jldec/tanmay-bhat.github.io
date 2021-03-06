---
layout: post
title: Scheduling pods in both Spot and On-demand nodes in EKS
date: 2021-11-19 01:46:58.000000000 +00:00
type: post
background: /img/visual-stories-micheile-ZVprbBmT8QA-unsplash.jpg
---
<p>You may have faced this scenario where you wanna keep scaling up apps &amp; nodes but also under-keeping costs at a limit.  Spot Instance is the way for that task. Now,  how do we do that? let's see.</p>
  
<p><!-- wp:heading {"level":3} --></p>
<h3 id="background">Background</h3>
   
  
<p>As you know there are mainly 2 types of instances in AWS, called <strong>On-demand</strong> and <strong>Spot</strong>. As the name suggests On-demand is priced highest because it's literally on demand from your side to AWS about node requirement. </p>
  
  
<p>Spot instances are a bit different. Spot instance is the unused capacity in AWS cloud sitting idle and AWS gives that to you at an extremely low price for like <strong>80-90%</strong> cheaper than on-demand. The difference being whenever AWS needs the capacity to handle on-demand, it <em>takes back the spot instances with ~2 min notice</em> via notification to you.</p>
  
<p><!-- wp:heading {"level":3} --></p>
<h3 id="node-groups">Node groups</h3>
   
  
<p>Now that we've learned about what spot offers, it makes total sense to include that in your workload to save quite a lot of money. So let's learn about the node groups for on-demand and spot.</p>
  
<p><!-- wp:heading {"level":5} --></p>
<h5 id="designing-node-groups">Designing node groups :</h5>
   
<p><!-- wp:list --></p>
<ul>
<li>Since spot can go down anytime, you should always run your critical workloads in on-demand instances.</li>
<li>All stateful- sets should run in on-demand instances.</li>
<li>Have multiple nodegroups for spot so that you can maximize chance of getting spot instances.</li>
<li>Use CA for scaling up / down.</li>
</ul>
 
<p><!-- wp:heading {"level":3} --></p>
<h3 id="real-world-scenario">Real-world scenario </h3>
   
  
<p>Now, let's say you have critical apps running in on-demand NG and other cronjobs or monitoring stacks are in spot NG.  If you wanted to schedule pods with the below architecture I  got the answer.</p>
  
  
<p><strong>Required architecture</strong>: If your app has 8 replicas, 4/5 of them should run in on-demand NG and 3/4 of them in spot NG such that even if the spot goes down, ondemand can handle the load until the new spot comes in and takes the load. In this way, you'll have 'Zero Downtime'.</p>
  
  
<p>Now for the above problem, there isn't a straightforward or clear-cut solution out of the box. But I'll explain the way I've implemented it.</p>
  
<p><!-- wp:heading {"level":5} --></p>
<h5 id="solution">Solution:</h5>
   
  
<p>Once both the NG are created, let's take a look at the label of a spot node.</p>
  
```sh
kubectl describe no ip-100-45-51-226.ap-south-1.compute.internal | grep SPOT
eks.amazonaws.com/capacityType=SPOT
```
  
<p>Any node created by spot will have the above label and any node with ondemand will have label : <code>eks.amazonaws.com/capacityType=ON_DEMAND</code></p>
  
  
<p>Once, they are done, you can create a sample Nginx deployment with the below configs: </p>
  
<script src="https://gist.github.com/tanmay-bhat/8f7777b4846bdb0b173ceb1c6a295c03.js"></script>

<p>All the other configs are pretty easy to understand and come under basic Kubernetes concepts. Since nodes created by spot and on-demand NG will have the above-mentioned labels, we can utilize that and request scheduler to try its best effort to schedule 40% of pods in this deployment to SPOT and 60% to ON_DEMAND.</p>
  
  
<p>You can change the above weight as per your needs. Once the above YAML is deployed, let's take a look at the way pods are scheduled. </p>
  
<p><!-- wp:image {"id":302,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-19-at-1.36.57-am.png" alt="" class="wp-image-302" /></figure>
 
  
<p>In my case, nodes with names <strong>25</strong> and <strong>226</strong> are Spot instances. If calculated correctly,  6 pods are running in on-demand and 4 pods are running in spot NG which is exactly as we expected.</p>
  
  
<p>NOTE: This may not be always exactly the ratio you need since the scheduler gives pods to nodes on a best effort basis.  But it'll be almost a similar result.</p>
  
  
<p>Happy Kuberneting !!!!</p>
  
