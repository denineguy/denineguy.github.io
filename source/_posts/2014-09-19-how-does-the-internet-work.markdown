---
layout: post
title: "How Does the Internet Work"
date: 2014-09-19 15:42:12 -0400
comments: true
categories: internet
---

This week I was asked a question that kind of stumped me.  How does the Internet work?  I had an idea of how it works, but I didn't know the correct terminology to correctly articulate.  It got me thinking, how does the Internet actually work? In this post I attempt to explain how it works at it's most basic level
<!-- more -->

##What is the Internet
The Internet is essentially a wire that is actually buried underground.  It may be fiber optic wire cables or sometimes beamed to satellites or cell phone networks but it is essentially a wire. The Internet, as I am sure you all know, is useful because two computers connected to this wire can communicate with each other.  A server is a computer connected directly to the Internet and a web page is essentially a file that is stored on that server’s hard drive.  

##How to render a Webpage
So say you are on your computer and you want to go to the google.com webpage.  You would type in www.google.com and voila a webpage would render...but how does that happen? Well your computer is a client because it does not directly connect to the Internet.  It connects to the Internet through and Internet Service Provider, using DSL. To get to google.com webpage, which is and ISP and a server, a lot happens on the backend. Let's first take a look at protocols

##Protocols
Protocols are sets of rules that machines follow to complete tasks. Two of the most important protocols for the web are TCP/IP, which defines how electronic devices (like computers) should be connected over the Internet, and how data should be transmitted between them. 

1. TCP - transmission control protocol
    - TCP breaks down every piece of data into packets, because a webpage is too large to transfer over the Internet all at once.
2. IP is Internet protocol
    - IP address is specific to each device that is connected to the Internet and it is how one machine can find another machine through the network

The TCP/IP protocols are usually used together and they are basically the rules for how information is passed through the Internet. When you want to send a message or retrieve information from another computer TCP/IP is what makes this possible. One of the most common types protocols for the web is Hypertext Transfer Protocol (HTTP), which takes care of the communication between the web server and the web browser.  It is used for sending information from the client (browser) to the web server and then returning the web content back from the web server to the client.   

##Sending and Retrieving Information
As stated previously, when I type in google.com a request is sent to the Internet Service Provider, which routes the request to the server. It first hits the domain name server (DNS), which looks at the domain name typed in (such as www.google.com) and then searches for the correct IP address associated with that Domain Name.  The DNS will then point that request in the correct direction.   Once it hits the target server, in our case it would be the server for Google, the server will respond by sending the files back to your computer in a series of packets.  The packets are about 1000 – 1500 bytes and have header and footer telling what information is in the packet and how it fits in with the entire file, along with the IP address.  Note that the packets don't all follow the same path back because they are looking for the quickest route back to the client.  Routers direct the packets around the Internet helping them reach your computer.   Each router examines the destination addresses and contents of the packets it receives and then passes the packets on to another router as they make their way to their final destination (your computer). Once they get to the client TCP comes into play again and puts the packets back together based on the information stored in that packet, whether that be an image, an email message or even a tweet. HTTP helps to return the web content to the browser.  The Hypertext Markup Language (HTML), in the files, tells the Web browser how to display the page and its element and thus google.com webpage is rendered.

So this is very basic, but it is in essence how the Internet works.  Learning this was very helpful to me and I hope that is will be of use to you.




