---
layout: post
title:  "Limited SSRF with blind file read, resulting in full file read"
tags: [web, hacking, realstory]
categories: [bugbounty]
---

In this article, I will cover one of my favorite vulnerabilities of all time and the one that I spent over three months trying to exploit at one of the largest companies in the world (_which I unfortunately cannot name_).

It all started with a reconnaissance, I discovered a web application that provided a 404 not found, but for some reason there was something about this 404 page that caught my interest. I've never seen a 404 page that had this design style before, which made me come to the theory that there must be either custom or a more rare service behind this application.

I started with a general wordlist just to see if I could find any type of directory or file in the application. My approach is always that if you can find something like a simple directory or file. It's a sign that there are more hidden assets to discover. I did discover one thing, when doing a path traversal within the path, the page was redirected to an admin login page. From the admin panel, I was able to find clues as to what the application's purpose was and it didn't take me long to discover that the application was being used as an API server. The application was running a rare API service that I had never seen before. I was able to crawl and extract more catalogs in the application and eventually found a request and upload function, but it was not the usual upload function. I was unable to understand the upload function from my blackbox testing so I decided to dig deeper into the application and perform more reconnaissance and asset discovery. Eventually, I was able to discover a php file that was leaking source code. It took me a while to analyze the source code before I realized that the code contained a function that seemed to be used in the upload function that I discovered earlier. From the leaked source code, I could see patterns in the variable names but also what the possible outcome would be for the HTTP response when this function was used.