---
layout: post
title: How to access AWS EKS cluster when you mess up the aws-auth configmap
date: 2021-09-01 11:34:29.000000000 +00:00
type: post
permalink: "/2021/09/01/how-to-access-aws-eks-cluster-when-you-mess-up-the-aws-auth-configmap/"
background: /img/posts/aws-mess.jpg
---
  
<p>Hey people, this is not a complete solution article, but rather a cut story and a probable solution for the below problem statement when it comes to locked out issue in EKS cluster:</p>
  
> I wanted to add a user to my EKS, hence while adding the user to <code>aws-auth configmap</code> of my EKS cluster, I made some syntax mistakes and now neither I nor anyone can login to EKS cluster" whole cluster is gone, help me please !!!
  
<p><span style="text-decoration:underline;">Straight forward solution which I found out :</span></p>
  
  
<p><strong>Find out who created the EKS cluster and ask them to edit the aws-auth configmap and correct your mistakes.</strong></p>
  
  
<p>The user who created the cluster is the root user for entity. Hence regardless of aws-auth configmap mess, he/she can login via kubectl anytime.</p>
  
  
<p>Read more <a href="https://aws.amazon.com/premiumsupport/knowledge-center/eks-api-server-unauthorized-error/">here </a>on solution by AWS.</p>
  
  
<p>I wrote this because I made this mistake in my company and spent hours searching for answer before finding this info.</p>
  
  
<p>Once I found out the creator, she corrected it in 1 min. :D </p>
  
  
<p>Good work buddy, job secured !!!!</p>
  
<p><!-- wp:separator --></p>
<hr class="wp-block-separator" />
<!-- /wp:separator -->
  
<p><strong>Long term solution :</strong></p>
  
  
<p>You might be saying ' Thats one solution to save my job, how do I make sure I dont do this mistake again ?'</p>
  
  
<p>Alright, so here's what you can follow from next time :</p>
  
  
<ol>
<li>First get the configmap yaml file by typing :</li>
</ol>
 
<p><!-- wp:paragraph {"align":"left"} --></p>
<p class="has-text-align-left"><code>kubectl get configmap aws-auth -n kube-system -o yaml &gt; aws-auth-configmap.yml</code></p>
  
  
<p>2. Once you get the yaml file, edit the file using your favorite text editor and update your changes. </p>
  
  
<p>3. Now, update the configmap with your new updated file by typing :</p>
  
  
<p><code>kubectl apply -n kube-system -f <code>aws-auth-configmap.yml</code> </code></p>
  
<p><strong>Remember</strong>, live editing is never a good option !!!</p>
  
  
  
