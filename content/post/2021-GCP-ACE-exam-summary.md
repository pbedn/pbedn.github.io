+++
title = "Google Associate Cloud Engineer exam summary"
date = "2021-01-19"
draft = false
tags = ['google cloud', 'gcp']
+++

A week ago I passed Google ACE exam, here I post a personal summary and some tips on how to pass it.

<!--more-->

#### About ACE certification
An Associate Cloud Engineer according to [official site][ace-official], deploys applications, monitors operations, and manages enterprise solutions.

The exam takes two hours, consists of 50 multiple choice and multiple select questions, and can be taken remotely or at a test centre.

#### Personal summary
I am working with Google Cloud, though with only a few services like AppEngine Standard, SQL and Bucket, for almost two years now. I signed to program through my company, that offered free access to learning course and materials, and in the end, a free voucher for an exam (125$). It took me about two months of ahead preparation. And still, I felt uncertain about the exam itself. I finished the exam 30 minutes before time and felt very confident about around 50% of the questions. For the rest I had to review them once again, choosing the best option that felt right.

#### Learning materials
I went through required by my company [Qwiklabs][qwik] courses:

* Google Cloud Platform Fundamentals: Core Infrastructure
* Essential Google Cloud Infrastructure: Foundation
* Essential Google Cloud Infrastructure: Core Services
* Elastic Google Cloud Infrastructure: Scaling and Automation
* Preparing for the Google Cloud Associate Cloud Engineer Exam

And next time I would take more time for this, doing the labs slower, and watching the videos with more focus. Especially with the labs, you can play with the real GCP environment for free. The lab agreement says you can do only things from the description, however, small deviations from my observations are fine as well, as long you do not run multicluster CPU expensive machines with many GPUs ;)

Humble Bundle had a nice offering, and I bought an "Official Google Cloud Certified Associate Cloud Engineer Study Guide" book by Dan Sulivan, that I read once. In addition to the book, you get ACE Study Guide Test Bank from [Willey Efficient Learning][test-bank] that contains almost 500 sample questions, divided into book chapters, pre-assessment and two sample exams. I went through all of them and highly recommend. Instructions on how to use it are in the book.

My third source of learning was a concise [Kubernetes Deep Dive][cg-k8s] course by Nigel Poulton, as k8s were a new beast to me. Nigel has a specific funny way of teaching which I like.

And finally, the fourth source was documentation and especially about Google best practices:

* [Best practices for enterprise organizations][bpeo]
* [Best practices for operating containers][bpoc]

#### Tips on exam
I had only one multiple-choice question, though I read that other people had more. Most important topics for the exam are IAM (roles, permissions, hierarchy, inheritance), networks (VPC, subnets, firewall, VPN, on-premise), Kubernetes (basics, deployment). I had some questions about Compute, AppEngine, differences between databases and when to use them, a few about billing. Only one or two questions about choosing the correct gcloud SDK command.

The hardest questions were about possible situations covering the migration of an application from client to GCP, how to set up a secure connection between client and GCP or deal with unexpected failures during an operation (logging, debugging). I think I would benefit from reading [SRE books][sre-books] and then answer these questions more easily, but I did not have time for that. 

---

Resources:

* [ACE certification official site][ace-official]
* [Qwiklabs Hands-on training][qwik]
* [Willey Efficient Learning site][test-bank]
* [Kubernetes Deep Dive course ][cg-k8s
* [Best practices for enterprise organizations][bpeo]
* [Best practices for operating containers][bpoc]
* [SRE books][sre-books]

[ace-official]: https://cloud.google.com/certification/cloud-engineer
[qwik]: https://www.qwiklabs.com/
[test-bank]: https://www.efficientlearning.com
[cg-k8s]: https://acloudguru.com/course/kubernetes-deep-dive
[bpeo]: https://cloud.google.com/docs/enterprise/best-practices-for-enterprise-organizations
[bpoc]: https://cloud.google.com/solutions/best-practices-for-operating-containers
[sre-books]: https://sre.google/books/
