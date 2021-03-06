---
layout: post
title: How to migrate a Node-Group from Multi AZ to single AZ in AWS EKS
date: 2021-10-11 12:46:58.000000000 +00:00
type: post
background: /img/posts/eks-2.png
permalink: "/2021/10/11/how-to-migrate-all-your-worker-nodes-from-multiple-az-to-single-az-in-aws-eks/"
---
  
<p>After reading the above title you maybe thinking why though? moving the complete worker node fleet into single Availability Zone (AZ) is not a good solution when it comes to high availability of your Kubernetes cluster workload.</p>
  
  
<p>There's a reason at least why I had this requirement, <strong>Cost optimization in AWS</strong>.</p>
  
  
<p>When you create a EKS cluster, it'll have 3 subnets each correcting to a single AZ i.e 3 AZ in a region. Now for staging / testing clusters the Inter Availability Zone data transfer fees we were getting was a hefty one, which was unnecessary as HA is not needed for the testing environment.</p>
  
  
<p>I couldn't find this anywhere else, so with an outage at staging cluster :D ( shhhhh!) </p>  
<p>I found out that the solution is to create a new node group with AZ hard-coded while creating it and any node you spawn in that node group using ASG (Auto Scaling Group) will be in that single AZ only keeping your inter AZ data transfer cost to 0. </p>
  
<p><!-- wp:code --></p>
<pre class="wp-block-code"><code><code>eksctl create nodegroup --cluster=staging_cluster  \
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp;--region=ap-south-1 \
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;--node-zones=ap-south-1a \
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;--name=M5.2xlarge_NG \
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;--node-type=m5.2xlarge</code></code></pre>
<p><!-- /wp:code --></p>
  
<p>In the above snippet, I'm creating NG in Mumbai Region in AZ ap-south-1a with m5.2xlarge instance.</p>
  
  
<p>If you want to go with GUI way, then :</p>
  
  
<p>1. Go to your cluster in EKS and then click on <strong>Add Node Group</strong> :</p>
  
<p><!-- wp:image {"id":191,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/10/image.png" alt="" class="wp-image-191" /></figure>
 
  
<p>2. Go with usual flow of giving it a name, taint if required, and IAM role.</p>
  
  
<p>3. Select AMI, disk size, instance family etc.</p>
  
  
<p>4. In Networking section, by default 3 subnets will be selected, untick 2 of them and keep 1 ( any desired AZ 'a/b/c'). If you're unsure about the name and AZ, you can verify that in VPC &gt; subnets.</p>
  
<p><!-- wp:image {"id":193,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/10/image-1.png" alt="" class="wp-image-193" /></figure>
 
  
<p>That's it, create the node group and all the instances will be spawned in that AZ only.</p>
  
  
<p>??You??can??also??do??this??in??a??hackish??way??by??editing??the??ASG corresponding??to??the??node??group??and??removing??2??subnets??from??there.
</p>

<p>It??works??but??your??node??group??will??become??Unhealthy and??AWS??wont??do??anything??to??the??node??groups??which??is having??health??issue.
So??better??to??create??a??new??node??group.</p>
  
  
<p>There's one more way which I found out i.e to use ekctl command line and create from config file.</p>
  
  
<p>You can read more about <em>ekctl </em>and configure it by referring <a href="https://eksctl.io/introduction/">Here</a>.</p>
  
  
<p>If you're going with config file, it should look like below : <code>ap-south-la-NG.yaml</code> :</p>

<pre class="wp-block-code"><code>
  apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig
metadata:
  name: Your_Cluster_Name
  region: ap-south-1

managedNodeGroups:
  - name: demo-nodegroup
    labels: { role: worker-nodes }
    instanceType: m5.xlarge
    desiredCapacity: 1
    volumeSize: 50
    availabilityZones: &#091; ap-south-1a ]
    minSize: 1
    maxSize: 2
    volumeType: gp3
    privateNetworking: true

</code></pre>
<p><!-- /wp:code --></p>
  
<p>Then you can apply using the below command : </p>
  
    
<pre class="wp-block-syntaxhighlighter-code">eksctl create nodegroup --config-file ap-south-la-NG.yaml</pre>
    
  
<p><strong>Finally</strong>, once the new Node groups is created, you can scale down your existing node group to <strong>0</strong> so that AWS will drain the nodes gracefully and all your workloads will be moved to newly created node groups.</p>
  
