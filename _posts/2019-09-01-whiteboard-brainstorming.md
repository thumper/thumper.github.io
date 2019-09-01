---
layout: single
title: Whiteboarding as Brainstorming
date:   2019-09-01 17:00:00
categories: posts
tags: [engineering]
comments: true
---

Part of my "brainstorming process" for working on a problem is to find a whiteboard
where I can write down all the ideas for solutions and then start filling
in pros and cons for each idea.
This helps me think of more ideas, as well as to begin considering what
are the values that are important in choosing a solution.


## A Whiteboarding Example

A recent problem my group had to look at was how to configure multiple ethernet
devices on a single host:

| Idea  | Pros  | Cons |
|-------|-------|------|
| Static Configuration  | requires work to fetch IP addresses | does not break during infrastructure outage |
| DHCP | trivial to configure | might have reliability issue if DHCP breaks |

Looking at our table told us we had two concerns: reliability, and time to develop.
The project was more nuanced than that, however: we were trying to get up-and-running
quickly to unblock another team, so `reliability` was important to us only in the
long term, and `time` was the most important concern *right now*.

Unfortunately, one of our partner teams didn't value the priorities the
same way.
Were there other approaches we had missed?
In fact, there were.
The obvious one is to implement both solutions - DHCP first to unblock the other teams,
and then later switch to static configuration.
Another idea we thought of was to keep `eth0` with static configuration (which
maintains the reliability of health monitoring), and use DHCP for the extra
ethernet devices.


| Idea  | Pros  | Cons |
|-------|-------|------|
| Static Configuration  | requires work to fetch IP addresses | does not break during infrastructure outage |
|                       | estimate a month to complete | easier to debug |
| DHCP | trivial to configure | might have reliability issue if DHCP breaks |
|                             | estimate a day to complete | harder to debug |
| DHCP first, Static Later | best of both worlds  | extra work |
| eth0 static, others DHCP | reliability would only affect applications, not monitoring | could be confusing to debug problems |
|                          | estimate a day to complete | |

Thinking about the new idea of mixing static and dynamic configuration together
identified a new concern: ease of problem diagnosis for the customer.

The point here is that this *process* of pedantically using the whiteboard
to layout choices can be a tool that helps identify alternative solutions.
I'm using the whiteboard to offload details from my brain so that I can step
back and see different aspects.

I hope this example gives you some ideas for how this technique might be
useful in your own "brainstorming process."
