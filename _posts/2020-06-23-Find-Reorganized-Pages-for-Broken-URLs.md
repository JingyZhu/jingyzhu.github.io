---
category: Info
layout: post
section-type: post
tags:
- Project Description
title: "Find Reorganized Pages for Broken URLs"
---

<img src="https://www.eecs.umich.edu/eecs/images/faculty-CSE-hi.png" alt="CSE-Logo" title="CSE-Logo" style="border: 0"/>
<!-- ![](https://www.eecs.umich.edu/eecs/images/faculty-CSE-hi.png) -->
<br><br><br>

###  Before the description
Hello, this is a page describing our research project. If you are **a site administrator who is redirected here by our crawls (from the link in the user-agent)**, we hope this post can provide some information for why we crawl your pages.
<br><br><br>

### What does this project do?
It is well known that web is decaying, namely pages created in the past are gradually broken as time goes on. However, it remains unclear that what's the status of their breakage and why they are broken. In order to figure out the answers to those questions, our project is trying to crawl URLs created at different times to see what's the fraction of broken pages and more importantly, why are they broken. 

In fact, we found that many broken pages are not "really" broken. Although the page (or content) are no longer accessible from the URL, it could be the reason that the web hosts have reorganized the site structures and the page is moved to a new place.  

Currenly, we are trying to develop a system that can help web hosts automatically find broken pages and the potential reorganized URLs that the old page could be moved to, so that redirections can be automatically set up without human effort.
<br><br><br>

### Project Members
- Jingyuan Zhu: PhD Student @ CSE
- Jiangchen Zhu: Undergrad Student @ CSE
- Vaspol Ruamviboonsuk: PhD Student @ CSE
- Harsha V. Madhyastha: Prof @ CSE
<br><br><br>

### For the web hosts:
During this study, we have to crawl a large scale of URLs in order to collect persuasive data and examples, as well as to test our system. We try our best to spread our the traffic and respect the policy of your site. Here is what we have done to achieve the goals:
- Whenever possible, we randomly shuffle all the URLs in our queue, so that at each period, there will be minimum requests to a single domain.
- We first crawl a domain's robots.txt, and then follow rule specified on it.

We just want to clarify that all the crawls (including content) are only used as the determination of a page's breakage and same page matching. There is no other usage. If you still feel uncomfortable with our crawls, please contact us at jingyz AT umich DOT edu to let us know.
