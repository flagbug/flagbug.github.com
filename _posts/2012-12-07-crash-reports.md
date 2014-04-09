---
layout: post
title: Crash Reports
comments: true
---

## Simplicity is everything

**To every developer out there: Make it simple for users to report crashes of your application.**

I've recently created a simple window that lets users send me the details about crashes that occure in my current project [Espera](http://https://github.com/flagbug/Espera). It only consists of a message that informs the user that the application has crashed, a details box and two buttons: *Send* and *Cancel*.

You wouldn't believe how much bug/crash reports I've received in 2 days after I releases the new reporting system. Bugs that I hadn't the time to cover in my unit tests, or that I never would think of.  Bugs that were invisible to me, till users experienced them.

As a user and developer at the same time [(some people might disagree)](http://www.codinghorror.com/blog/2004/09/the-rise-and-fall-of-homo-logicus.html), I can tell you one thing: Make it easy for users to submit crash- and bug reports. The last thing you want is to fiddle through a dozen of steps so you can submit this one bug to the developer of an application.

## FogBugz makes it easy

Since I'm the only developer of Espera, I'm able to use the free [Student and Startup edition](http://www.fogcreek.com/fogbugz/StudentAndStartup.html)  of FogBugz. [20 lines of code later](https://github.com/flagbug/Espera/blob/92dc8d98b38bd7a96ec317be0893e47da2438fad/Espera/Espera.Services/FogBugzService.cs), I've integrated it within my the application, since FogBugz allows the send bug reports directly through a Http request.

One thing I learned: always send at least the **version number** of your application with the crash report, so you can easily distinguish new crashes from crashes that appear only because the user hasn't updated the application yet.