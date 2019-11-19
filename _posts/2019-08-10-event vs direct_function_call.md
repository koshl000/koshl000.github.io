---
layout: post
title:  "Event vs Direct function call"
categories: Design
tags: Design
author: moai
---

Because events can be subscribed to and unsubscribed from by other components or segments of code, to be notified about what that first component is doing without tight coupling. If you were to use a direct method call, that direct method would have to notify other components itself which creates tight coupling. Events are most common on reusable components or components that need to communicate or expose themselves to a system.

Direct method calls don't quite have the flexibility event driven components can provide, but it depends on your system. A good design will have loose coupling in mind from the get-go.