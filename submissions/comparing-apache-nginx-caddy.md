---
author:
  name: Linode Community
  email: docs@linode.com
description: 'A comparision of Apache HTTP Server, Nginx and Caddy Web Server, identifying the key features and there best usage cases'
keywords: 'apache, ngnix, caddy, web server'
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
published: 'Sunday, November 19th, 2017'
modified: Sunday, November 19th, 2017
modified_by:
  name: James White
title: 'Comparing Apache HTTP Server, Nginx and Caddy Web Server'
contributor:
  name: James White
  link: http://github.com/jamesmacwhite
  external_resources:
- '[Link Title 1](http://www.example.com)'
- '[Link Title 2](http://www.example.net)'
---

*This is a Linode Community guide. If you're an expert on something for which we need a guide, you too can [get paid to write for us](/docs/contribute).*

----

## Introduction

When you visit any webpage on the internet, a web server is responsible for processing that request and the returning the appropriate result back to the client (web browser). There are many web server platforms available that run on different operating systems and are built with different technologies, features and objectives in mind. This guide will compare three popular web servers, Apache HTTP Server, Nginx and Caddy Web Server, all of which can run on the Linode hosting platform and are most commonly found on a Linux based setup.

<img src="https://4.bp.blogspot.com/-KoPll1nPZ7w/Vh8udJAtgzI/AAAAAAAAFCU/HxFxr9C3aAI/w800-h800/apache.png" alt="Apache HTTP Server" />

## Apache HTTP Server
- https://httpd.apache.org/

Apache HTTP server (better known as just Apache) is the oldest web server software of the three platforms covered in this guide with its first public version released in 1995. Apache is free and open source software maintained by the Apache Software Foundation under the Apache License 2.0 (with earlier versions under the 1.1 license). It is estimated that as of the June 2017, over 90% of Apache web server installations are running on some form of Linux distribution. Due to its age, Apache is considered to be one of the most mature and stable web servers available today, which is a testament to its widespread usage, particularly on a Linux based stack. There are several major version milestones of the Apache HTTP Server that have been released throughout its lifecycle.

* Apache 1.0 (with subsequent releases in the 1.x branch)
* Apache 2.2 (minor versions 2.2.x)
* Apache 2.4 (minor versions 2.4.x) [current]
* Apache 2.5 (next upcoming major release)

### Key features:

* Basic access authentication (basic auth)
* Digest access authentication
* SSL/TLS
 * TLS 1.0 - 1.2
 * SNI (Server Name Indication)
* Virtual Hosting (run multiple domains from one web server instance)
* Supports FastCGI/SGCI/WSGI/ISAPI
* Server Side Includes
* IPv4/IPv6
* HTTP/2

Apache is widely regarded to be one of the most configurable web server platforms with lots of configuration options available. However due to the vast amount of configuration available, Apache is sometimes seen as quite difficult to setup, because its configuration syntax can be quite overwhelming to new comers that are not familiar with command line or configuration files, some of which can be quite arcane at times if don't really want to spend time with the various .conf files. However, thanks to its [comprehensive documentation](https://httpd.apache.org/docs/) all of its configuration options are fairly well documented, meaning its very likely you'll be able to do pretty much anything with Apache. Also thanks to its wide spread usage, there is a lot of support for Apache, from both user communities and also commercial support.

As well as configuration files, Apache also makes it possible to change certain system wide configuration options through a special file, known as the .htaccess file. This file accepts a variety of options with a specific syntax allowing you to perform certain tasks like URL rewriting, overriding certain PHP configuration options and more. For security, some system wide settings or environment variables will not able to be modified, but generally you can control a fair bit of the environment with .htaccess to your requirements. It is a very popular configuration for shared hosting platforms.

### Why use Apache?

If your looking for a solid web server that offers the most configuration possibilities and want to run a typical LAMP stack, Apache would be a great choice. Its well documented, a lot of configuration examples from both the official documentation and third party sources will allow you to setup Apache to your liking. It is very rare to find something Apache cannot do these days! Likewise with its ability to run code like PHP directly, you can get yourself a typical LAMP stack setup in no time.

<img src="https://nginx.org/nginx.png" alt="nginx" />

## Nginx
- https://nginx.org/en/

Nginx (pronounced Engine X) saw its first public release in 2004 and like Apache is free, open source and runs on different platforms. One of the primary goals when developing Nginx was to actually out perform Apache in a variety of areas. In one way, being a younger project, Ngnix had the average of seeing how Apache performs in certain areas and essentially improving upon Apache's shortcomings that have been identified over the years. The main areas Nginx focuses on are:

* Serving requested content to the client (web browser) quicker.
* Be able to handle more requests per second.
* Reducing the overall memory footprint of nginx compared to Apache
* Be really fast!

### Key features:

* Basic access authentication (basic auth)
* Digest access authentication
* SSL/TLS
 * TLS 1.0 - 1.2
 * SNI (Server Name Indication)
* Virtual Hosting (run multiple domains from one web server instance)
* Supports FastCGI/SGCI/WSGI
* Server Side Includes
* IPv4/IPv6
* HTTP/2

Nginx was essentially developed because its original creator, Igor Sysoev, needed to solve the "C10k problem" (Concurrent 10k connections). This is a term to describe a scenario in which a website is receiving tens of thousands requests at regular intervals and needs to handle the large number of clients effectively, while maintaining a fast and responsive experience. In more recent years, the term "C10M problem" has also been used to describe a website or service that receives a concurrent 10 million connections, with websites like Facebook being a classic example of such high traffic loads.

Its no surprise then that Nginx is very performance focused in everything that it does, but how does it achieve this? It actually starts at its foundation. Unlike Apache and its process based model, Nginx is designed with an event-driven, asynchronous, non-blocking and single threaded principle in mind. Essentially Nginx runs as one master process with worker processes. This design methodology helps Ngnix out perform web servers like Apache in areas like scalability, system performance and one of the most important areas load/request time. It does have one potential disadvantage that compared to Apache, Nginx cannot directly interpret languages like PHP and instead has to pass these to other processes, whereas Apache can directly interpret such languages if configured to do so. This may or may not be a disadvantage from Nginx's point of view, but will require further configuration for Nginx to be able to parse code like PHP

In addition to its different design principles another way this performance gain is achieved is by not providing the ability to change system wide access settings on a per-file basis. Apache allows this through its .htaccess implementation, Nginx on the other hand does not provide such functionality as standard. In addition to this, Nginx also requires third party modules to be statically linked at compile time, rather than being able to dynamically load modules in on a more ad-hoc basis. While Nginx does allow for dynamic module loading in version 1.9.11 and above, such modules still need to be compiled with Nginx during compile time. This benefits overall performance at the cost of easy extendability.

In recent years many companies have adopted Nginx as the web server of choice for serving static content likes CSS, images and other file types due to its better performance with serving such content. In fact most Content Distribution Networks (CDN) are likely to be using Nginx in some form to serve such content. One example is CloudFlare, a popular CDN service, they use Nginx for this very purpose. In fact they operate hundreds of Nginx deployments in a load balanced setup, again thanks to Nginx having better scalability and overall performance benefits.

### Why use Nginx?

If your looking to run a website that is all about performance with needs for scalability, caching, load balancing and more, Nginx is the way to go. In certain configuration, Nginx will out perform Apache in a number of benchmarks. Unlike Apache, Nginx cannot run code like PHP through itself, instead you'll need to run something like php-fpm (FastCGI) which will require additional configuration and might be a bit daunting to configure for someone unfamiliar with server configuration.

<img src="https://caddyserver.com/resources/images/brand/caddy-black.png" alt="Caddy Server" width="300" />

## Caddy Web Server
- https://caddyserver.com/

Caddy is a relatively new web server with its first public release in April 2015 and is written entirely in the Go programming language. Like Apache and Nginx it is also free and open source. One of Caddy's most unique characteristics is its emphasis on security. Caddy, follows a default HTTPS on principle making it the first major web server to achieve an out of the box SSL configuration without any additional configuration required by the user. To achieve this Caddy uses the ACME protocol to configure a valid LetsEncrypt SSL certificate against a configured website running on Caddy. It handles the HTTP to HTTPS redirection for you and automatically renews the configured the SSL certificate. This is something that Apache and Nginx cannot do automatically. The upcoming release of Apache 2.5 is to include the mod_md module which provides similar functionality to assign a SSL certificate obtained via LetsEncrypt ACME protocol at the VirtualHost level, but doesn't have the same automatic configuration that Caddy does.

Given that there is a major push for getting websites across the internet using HTTPS under the LetsEncrypt initiative, this is a very welcome feature to help improve security on the web. Statistics show about 2% of overall LetsEncrypt SSL certificates issued worldwide are from this feature in Caddy.

### Key features:

* Basic access authentication (basic auth)
* SSL/TLS
 * TLS 1.0 - 1.2
 * SNI (Server Name Indication)
 * Automatic HTTPS configuration through LetsEncrypt ACME protocol
* Virtual Hosting (run multiple domains from one web server instance)
* Supports CGI via WebSockets
* FastCGI proxy support (e.g. php-fpm)
* Server Side Includes
* Native Markdown rendering built in
* IPv4/IPv6
* HTTP/2
* Experimental QUIC UDP support

In addition to Caddy's default HTTPS feature, Caddy was also one of the first to implement HTTP/2 (default for HTTPS) as standard and is typically deemed to be a more security focused web server, having very strong SSL/TLS configuration by default. Because of this, Caddy is not vulnerable to several major exploits found with SSL/TLS including, Heartbleed, DROWN, POODLE and BEAST which a vast majority of Apache and Nginx installation were affected by in some way. In addition, Caddy also makes use of TLS_FALLBACK_SCSV, to prevent downgrade attacks occurring where a client is deliberately forced to negotiate to a lower SSL/TLS version where an attacker can potentially decrypt SSL traffic by leveraging the vulnerabilities mentioned above.

Caddy also prides itself of being easier to configure compared to Apache and Ngnix, something which might be less daunting for newcomers who wish run their own web server and websites themselves but don't have much experience with editing configuration files or using command line.

By default Caddy comes with a extensive amount of features to allow you to get up and running. If you want more features, you'll want to look at the various Caddy plugins (formerly called add-ons) available that allow you to extend the functionality of Caddy. A large portion of these plugins have been written by the Caddy web server community. These can be loaded into Caddy quite easily and is a little more friendly than dynamic loading/static compiled modules in Apache and Nginx respectively.

### Why use Caddy Web Server?

If your looking for a web server that's easy to configure, priorities security principles and want to take advantage of the built in HTTP/2 support without additional configuration, give Caddy a try. It isn't perhaps as scalable as Nginx, or provide as much configuration ability as Apache but will certainly be suitable for smaller sites or personal web projects that you might not want to host publicly and its pretty fast too!

### Summary

The three web servers covered in this guide generally have feature parity when compared to each other, Generally, the best way to tackle the all important web server question, would be to think about what type of website you are going to run. Think about size, scale and what your website might need to do in the future.

One important area to note with web servers is that they are software components. As all of the web servers covered in this guide all run on a variety of different platforms, you might start off using something like Caddy and maybe progress onto using Apache and then Nginx as you get more comfortable with managing a web server. In the case of Apache or Ngnix there are control panel solutions like DirectAdmin or cPanel, which take some of the burden of having to setup and configure these web servers from scratch away from you and instead let you focus more on building/hosting websites as required.

Like with any piece of free software, you have the choice. Overall, one recommendation would be to experiment with each different web server for your website to get an understanding of how its configured, how it works and ultimately what it offers. It wouldn't be wise to continuously keep switching the web server platform for a website regularly, but often is the case with any software, testing and actually experimenting with it is often the best way to make a decision. Given all of the web servers covered in this guide are free. Its quite easy to setup a test instance of any of them to play around to find out what works for you.


