---
layout: post
title: Dynamic PV in Kubernetes feat. EKS (EBS)
date: 2021-11-13 20:13:38.000000000 +00:00
type: post
permalink: "/2021/11/13/dynamic-pv-in-kubernetes-feat-eks-ebs/"
background: /img/posts/aws-skeleton.png
---
<p>This article doesn't explain what are PV and PVC's in Kubernetes. For that you can refer the official Docs.</p>
  
  
<p>Initial question would be, what does volume resizing mean ( for PV in Kubernetes)  ? </p>
  
  
<p>Its the ability to increase the PV size ( EBS volume behind the scene ).</p>
  
  
<p><strong>Up until v1.16 EKS, you can just increase any ( PV ) EBS volume size just by running command like</strong> : <code>kubectl edit pv your_PV</code> <strong>and just change the size, it used to work since you have storage class  of</strong> <code>kubernetes.io/aws-ebs</code>.</p>
  
  
<p>You can't resize your PV just by changing the size in the manifest file ( if &gt; v1.17 and doesn't have volume controller.) </p>
  
  
<p>Now, what should you do as a Kubernetes admin if you wanna resize your PV with above mentioned version ?</p>
  
  
<p>Simple, Kubernetes team has a new tool called <strong>ebs-csi controller</strong>. What does it do? </p>
  
  
<p><em>The Amazon Elastic Block Store (Amazon EBS) Container Storage Interface (CSI) driver provides a CSI interface that allows Amazon Elastic Kubernetes Service (Amazon EKS) clusters to manage the lifecycle of Amazon EBS volumes for persistent volumes.</em></p>
  
  
<p>You can install the ebs-csi driver by referring to AWS <a href="https://docs.aws.amazon.com/eks/latest/userguide/ebs-csi.html">document</a>.</p>
  
  
<p>Once you install it, you should see the pods like below : </p>
  
<p><!-- wp:image {"id":243,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-7.38.44-pm-1.png" alt="" class="wp-image-243" /></figure>
 
  
<p>And the ebs-csi-controller pod logs should look like : </p>
  
<p><!-- wp:image {"id":244,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-7.43.07-pm.png" alt="" class="wp-image-244" /></figure>
 
  
<p>Looks good, now, for a test, lets edit a PV and increase its size. In my example, I'll just increase the alert-manager PV, Its initial size was 2GB. I'll increase it to 3GB.</p>
  
<p><!-- wp:image {"id":246,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-7.46.10-pm.png" alt="" class="wp-image-246" /></figure>
 
  
<p>If you are thinking how did I beautify Kubernetes editing ? all thanks to <a href="https://k8slens.dev">Lens</a> IDE.</p>
  
  
<p>lets verify the PV size now :</p>
  
<p><!-- wp:image {"id":248,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-7.49.53-pm.png" alt="" class="wp-image-248" /></figure>
 
  
<p>Here comes the real test to see if the actual EBS volume is resized or not. For that let's copy the volume id and search that volume size in AWS console or via AWS cli to verify the disk size.</p>
  
  
<p>To get the volume id of a PV, run the below command :</p>
  
  
<p><code>kubectl describe pv PV_NAME  | grep Volume</code></p>
  
  
<p>Now if you prefer cli, lets verify from AWS CLI: </p>
  
<p><!-- wp:image {"id":251,"sizeSlug":"large","linkDestination":"none"} --></p>
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/2021/11/screenshot-2021-11-13-at-7.56.00-pm.png" alt="" class="wp-image-251" /></figure>
 
  
<p>Tada !!! it worked !!!</p>
  
