---
author: Jeff
title: Amazon FPS vs Google Checkout
excerpt:
layout: post
categories:
  - Web Development
tags:
  - Amazon
  - Amazon FPS
  - AWS
  - Google
  - Google Checkout
  - Web Development
post_format: [ ]
comments: true
date: 2008-02-21T18:49:42+0000
---
So working with these two relatively new systems I’ve come to the surprising conclusion that I like Amazon’s better. I find it odd because I’m normally a bit of a Google fanboy. I should explain more this conclusion is not for all cases there are many thing you can do with one you can’t with the other, but for the basic case I was trying to set up Amazon FPS worked much better. I wanted to create a simple checkout that allowed me to generate accounts after the payment was processed. Sounds pretty simple I did something like this with 1Shoppingcart.com and if they made it easy Google and Amazon should make it a snap.

So reading up on Google Checkout I found one surprising thing, I have to have some sort of certificate, well I think the documentation was a bit confusing. That is for the level 2 API integration and that seems to be what I wanted so I could process the information after they paid. Well I don’t want to get the secure stuff setup don’t want the bother or the cost of it, I would if this was going to be a real big deal site but its just a small project that at this point needs low costs and quick implementation more then keeping 1 or 2 people from creating a bogus login.

So I started looking at Amazon FPS and to start the documentation was much more approachable not sure if it was the content or the general layout but I found what I wanted in about 1/10th of the time. They have a nice section explaining how to make a dynamic button which is perfect and then describe how to sign the button when code samples, and the best part is they have many languages Java, PHP, and Perl to name the once I quick saw. The best part is that the callback URL can just be any URL no need for https so this makes it fit right into my current system.

Overall I found Amazon FPS quick and easy to implement a system with that interacted back with my site while with Google you can make quick Buy It Now buttons the integration back into the my system was not as easy and Google Checkout is very geared towards physical items that you ship while Amazon FPS doesn’t seem to really tie me into that. Maybe someone trying to sell physical items on Amazon FPS will have issues I’m not sure but I would love to hear any experiences.
