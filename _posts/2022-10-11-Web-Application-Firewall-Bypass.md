---
layout: post
title:  "Web Application Firewall Bypass"
tags: [web, hacking, bypass, yeswehack]
categories: [research]
---

A web application firewall (WAF) is a program specially designed to filter, monitor and block malicious web requests related to its configurations.

The WAF filter and detection department is dependent on two primary configurations. The black/white list and a regex. These configurations make it possible for a WAF to determine if a request contains malicious content.

In this article, we will discuss different ways a WAF can be bypassed when a vulnerability has been discovered.
The topic will focus on how to take advantage of the configurations and normalisation that could affect the way a payload is being handled in the transport. We will show different scenarios and examples of manipulations that lead to a successful bypass.

> You can find the full article on [YesWeHack's blog page](https://www.yeswehack.com/learn-bug-bounty/web-application-firewall-bypass){:target="_blank"}
{: .prompt-info }