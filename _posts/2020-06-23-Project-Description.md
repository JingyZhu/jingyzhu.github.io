---
category: Info
layout: post
section-type: post
tags:
- Project-Description
title: "Broken links on the web: Detection, Characterization, and Solutions"
permalink: /ReorgPageFinder/
---

<img src="https://www.eecs.umich.edu/eecs/images/faculty-CSE-hi.png" alt="CSE-Logo" title="CSE-Logo" style="border: 0"/>
<!-- ![](https://www.eecs.umich.edu/eecs/images/faculty-CSE-hi.png) -->
<br><br><br>

You might be accessing this page because you received requests which included the link to this page in the user-agent. The requests that you received are benign and are generated at a low rate. They were generated as part of an academic study to study and fix broken links on the web. More detailed information about the project is given below. If you would like us to stop making these probes to your site, please send us email at **EMAIL ADDRESS** and we will exclude your site from our crawls.
<br><br><br>

### What does this project do?
It is well known that web is decaying, namely links to pages created in the past cease to be functional as time goes on. In order to understand this phenomenon, we perform representative crawls of the web to quantify the fraction of broken pages and, more importantly, to characterize why they are broken.
<br><br>
In fact, we have found that many broken links to web pages are not because those pages no longer exist, but instead due to reorganization of websites. Therefore, when a page is no longer accessible from its original URL, we have found that it is often available at a different URL on the same site.
<br><br>
Currenly, we are trying to develop a system that can help web providers automatically find broken links to their pages and potential URLs that the old link could be redirected to.
<br><br><br>

### Project Members
- Jingyuan Zhu: PhD Student @ CSE
- Jiangchen Zhu: Undergrad @ CSE --> PhD Student @ Columbia
- [Vaspol Ruamviboonsuk](https://vaspol.me/): PhD Student @ CSE
- [Harsha V. Madhyastha](https://web.eecs.umich.edu/~harshavm/): Associate Professor @ CSE
<br><br><br>

### Measures taken to minimize impact of web crawls
In this study, we crawl a large number of URLs in order to collect representative data and examples, as well as to test our system. We try our best to spread out our requests to any particular site and respect the policy of each site (by following the rules specified in the site's robots.txt). All of our crawls (including page content that we fetch) are only used to identify broken links and to match a page's content with what was previously fetched when crawling that page's link.
<br><br>
If you still feel uncomfortable with our crawls, please contact us at **EMAIL ADDRESS** and we will exclude your site from our crawls.
