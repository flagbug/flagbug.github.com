---
layout: post
title: Better open source collaboration with Gitter
---

With [Mahapps.Metro](https://github.com/MahApps/MahApps.Metro) we've always had the problem that discussing new plans with the core team and the contributors, or questions from the users didn't really work over Github issues. The core team communicated over Skype and we also had an open IRC channel, which was really cumbersome since IRC doesn't preserve the chat history without everyone installing a bot that logs the history on their own server.

Today I discovered [Gitter](http://gitter.im), a fancy new service that allows people to create chatrooms for projects on Github.

They're currently invite only (meaning only people with invites can create chatrooms but people without invites can join them), but after asking I almost immediately got an invite and created the [Mahapps.Metro chatroom](https://gitter.im/MahApps/MahApps.Metro)

<img src="images/gitter.png" width="700" />

Gitter has an awesome Github integration, there's a configurable activity log on the right side of the chat where recent issues, pull requests, commits, etc are displayed. The chat also supports auto-linking of issues, code snippets and Markdown support.

After only a few hours, I've got most of our current contributors to switch from IRC to Gitter and what should I say, it's been an awesome experience.

The team of Gitter responds insanely fast to questions on Twitter and you can also drop in on the [GitterHQ chatroom](https://gitter.im/gitterHQ/gitter) and ask them questions directly (they even changed that you don't need to be the owner of an organization, but only require push access in order to create a chatroom for a organization repository after I asked them if it was possible)

## The bad parts

I don't have much to write here. 

Gitter requires write access to your Github account, this is a limitation of the Github API, they have it described [here](https://gitter.zendesk.com/hc/en-us/articles/200178961-Why-do-you-ask-for-write-access-to-my-profile-) and [here](https://gitter.zendesk.com/hc/en-us/articles/200178971-You-want-write-access-on-my-private-repos-Are-you-insane-)

This isn't too much of a problem for me.

## Conclusion

Gitter is an awesome piece of work and I believe it will simplify the collaboration on the projects I'm working on.

The only thing that's currently missing for me is an Android app, but from their website it seems that this is in work.
