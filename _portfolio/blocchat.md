---
layout: post
title: BlocChat
thumbnail-path: "img/blocchat.png"
short-description: BlocChat is a chat room application that allows users to submit electronic messages.
date: 2015-08-31

---

{:.center}
![]({{ site.baseurl }}/img/blocchat.png)

## Summary

As time elapses, society shifts toward electronic and digital means to communicate. The days of writing letters are simply over. Today, emails and text messages are prevalent and won't drift away anytime soon. BlocChat is a product of today's society. BlocChat is an application that permits users to chat with others in real time digitally. Postage is unnecessary when sending messages to family and friends who live near and far away. A simple typed message, along with a username and at least one chat room, is all that is need to operate BlocChat.

## Explanation

A lot must be considered when designing and engineering BlocChat. In recognition of the users' desires, BlocChat aims to achieve the following goals in its application: 

- Determine how to log into BlocChat
- Determine how to display and create the chat rooms
- Determine how to associate messages with chat rooms 
- Determine how to associate usernames with messages
- Determine how to submit messages and display them 

## Solution 

![BlocChat]({{ site.baseurl }}/img/blocchatusernameentry.png)

Messages cannot be transmitted to chat rooms from users without an identity a.k.a. username. Users are denied access to BlocChat is they do not enter a username. 

A pop-up box appears immediately upon entrance into the application. If a user enters a blank username, an alert box will appear and deny access into BlocChat. Once a valid username is submitted, access into BlocChat is granted. That username is stored as a cookie.

![BlocChat]({{ site.baseurl }}/img/blocchathome.png)

The BlocChat homepage will typically not have any chat rooms listed upon first entry, but for sake of demonstration, an example snapshot is provided above.

What will appear everytime a user enters BlocChat is a button that can create chat rooms. 

![BlocChat]({{ site.baseurl }}/img/blocchatroomentry.png)

When a user clicks on the chat room creation button, a pop-up will appear on the screen, similar to the username login modal. Just like that one, a user must enter a valid chat room name. If the entry is blank, an alert box will display a message denying access.

When a chat room name is submitted, a chat room is officially created and a user can begin sending messages or establish more rooms.

![BlocChat]({{ site.baseurl }}/img/blocchat.png)

The message input is displayed at the bottom because the messages are listed in the middle of the application. The list of messages descends after ever submission. The messages are also programed so that their submitted to their respective chat rooms. A message's submission time is also stored.

## Conclusion

BlocChat offers convenience and simplicity to users who want to connect with friends, family members and peers digitally.
