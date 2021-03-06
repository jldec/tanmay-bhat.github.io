---
layout: post
title: A closer look at Cluster Autoscaler for EKS
date: 2021-11-13 21:40:26.000000000 +00:00
type: post
background: /img/posts/eks-2.png
permalink: "/2021/11/13/a-closer-look-at-cluster-autoscaler-for-eks/"
---  
<p>If you're wondering why do I write about AWS that much, that's because AWS is the cloud on which I spend most of my work hours in <a href="https://skit.ai">Skit.ai</a> as a DevOps Engineer.</p>
  
  
<p>Ok, let's take a look at what cluster autoscaler is and how does it work?</p>
  
  
<p><strong>Cluster Autoscaler</strong> is a tool that automatically adjusts the size of the Kubernetes cluster when one of the following conditions is true:</p>
  
<p><!-- wp:list --></p>
<ul>
<li>there are pods that failed to run in the cluster due to insufficient resources.</li>
<li>there are nodes in the cluster that have been underutilized for an extended period of time and their pods can be placed on other existing nodes.</li>
</ul>
 
  
<p>For anyone who's going to implement autoscaler in their EKS cluster, please read this <a href="https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md">FAQ</a>.</p>
  
  
<p>The setting up of autoscaler in EKS is perfectly written by AWS document <a href="https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html">Here</a>.</p>
  
  
<p>once, things are set up, the logs should look like below :</p>
  
<p><!-- wp:image {"id":264,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-9.00.41-pm.png" alt="" class="wp-image-264" /></figure>
 
  
<p>Now if you're getting this, then it means the setup is clean. If we take a closer look at logs, it says node minimum size reached and cant scale down anymore.</p>
  
  
<p>Let's understand scaling up and scale down criteria and it's working. </p>
  
  
<p><strong>Scale down criteria &amp; working : </strong></p>
  
  
<ol>
<li>Every 10 seconds (configurable by&nbsp;<code>--scan-interval</code>&nbsp;flag), if no scale-up is needed, Cluster Autoscaler checks which nodes are unneeded. A node is considered for removal when&nbsp;<strong>all</strong>&nbsp;below conditions hold:</li>
<li>The sum of cpu and memory requests of all pods running on this node is smaller than 50% of the node's allocatable. </li>
<li>All pods running on the node (except these that run on all nodes by default, like manifest-run pods or pods created by daemonsets) can be moved to other nodes. </li>
<li>It doesn't have scale-down disabled annotation (see&nbsp;<a href="https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#how-can-i-prevent-cluster-autoscaler-from-scaling-down-a-particular-node">How can I prevent Cluster Autoscaler from scaling down a particular node?</a>)</li>
</ol>
 
  
<p>If a node is unneeded for more than 10 minutes, it will be terminated.  Cluster Autoscaler terminates one non-empty node at a time to reduce the risk of creating new unschedulable pods. The next node may possibly be terminated just after the first one, if it was also unneeded for more than 10 min and didn't rely on the same nodes in simulation (see below example scenario), but not together. Empty nodes, on the other hand, can be terminated in bulk, up to 10 nodes at a time.</p>
  
  
<p>What happens when a non-empty node is terminated? As mentioned above, all pods should be migrated elsewhere. Cluster Autoscaler does this by evicting them and tainting the node, so they aren't scheduled there again.</p>
  
  
<p>Also, you should consider the below point : </p>
  
<p><!-- wp:list --></p>
<ul>
<li>If there's a node which is under-utilized but that node counts towards minimum node group size, then CA wont terminate that node and the logs will be similar to above screenshot.</li>
</ul>
 
  
<p><strong>Scale-up criteria &amp; working</strong> :</p>
  
  
<ol>
<li>Scale-up creates a watch on the API server looking for all pods. It checks for any unschedulable pods every 10 seconds (configurable by??<code>--scan-interval</code>??flag). A pod is unschedulable when the Kubernetes scheduler is unable to find a node that can accommodate the pod. For example, a pod can request more CPU that is available on any of the cluster nodes. </li>
<li>Unschedulable pods are recognized by their PodCondition. Whenever a Kubernetes scheduler fails to find a place to run a pod, it sets "schedulable" PodCondition to false and reason to "unschedulable". If there are any items in the unschedulable pods list, Cluster Autoscaler tries to find a new place to run them.</li>
</ol>
 
  
<p><strong>Testing the CA : </strong></p>
  
  
<ol>
<li>Let's assume you got 2 <em>t3.medium</em> node and the <em>min</em> value of nodegroup is 2 with <em>max</em> value set to 5.</li>
<li>Run a nginx deployment with 500 replicas to see if cluster autoscaler scales up the nodes.. The command would be :
<ol>
<li><code>kubectl create deployment cluster-killer  --image=nginx --replicas=500</code></li>
</ol>
</li>
<li>2 nodes of that size can't handle 500 pods of nginx, so they should be in pending state and CA scans for pending state pods every 10 seconds which should start couple of nodes within minutes. You can verify from command : <code>kubectl get node</code></li>
<li>Once all pods are scheduled, to test scale down, you can either delete the deployment using : <code>kubectl delete deployment cluster-killer</code> or scale down the replicas to zero with command : <code>kubectl scale deployment cluster-killer --replicas=0</code></li>
<li>If you refer the logs of cluster autoscaler now, it will mention that X node is uneeded for X min etc.</li>
<li>The cool down period by default is 10 min so, after that time, it'll apply taint on that node with name DeletionCandidateOfClusterAutoscaler and ToBeDeletedByClusterAutoscaler and removes the nodes. It looks like below: </li>
</ol>
 
<p><!-- wp:image {"id":272,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/00d9d158-0d67-4179-88f2-c99111894dff.png" alt="" class="wp-image-272" /></figure>
 
  
  
