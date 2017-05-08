---
layout: single
title: Rate-of-Return Report - Lending Club
date:   2017-05-07 17:00:00
categories: posts
tags: [finance]
comments: true
---

I like to experiment in different kinds of investments to see how they compare
to traditional stock investing.
Around 2012 there was some buzz about peer-to-peer lending platforms, with many
favorable comments about Lending Club.
I decided to take the plunge by investing $6,550 and using the Lending Club
tools to automatically select my first "portfolio" of 69 loans to help fund.

Initially, money started pouring in and I was quite excited by how easy it was...
until a few months later when some loans stopped paying.
As more months passed, I saw that several loans were being charged off
and a couple would start performing again and even pay a late fee.
Now that all the loans have completed, let's take a look at the numbers involved.

| Invested | $6,550 |
| Weighted Average Rate | 15.52% |
| Payments Received | $6,650.06 |
| Actual Return | 1.5% |

{% include image.html
  name="portfolio1.png"
  caption="LC summary of my first portfolio"
%}

That is pretty bleak!
I had no idea it would turn out so badly at the time, but high number of
defaults really concerned me.

## Machine Learning to the Rescue

One really cool feature about Lending Club is that they make the data about
their loans available for download so that you can do your own analysis.
I downloaded the data fed it into Weka, a machine learning framework, using
"alternating decision trees" so that the rules generated were understandable
in human terms and I could decide what I could actually put into practice
with the capabilities of the platform.
I learned a lot of interesting things about which loans are less likely to
default; for example, during one run my system generated the rule that people
with a poor credit score but wanting to borrow to fix up their home were
more likely to never default.

Over the course of the next year I invested several more times, with a few
months between each round of investment.
Here are the performance results of each individual round:

| Round | Invested | Number of Notes | Weighted Avg Rate | Payments Received | Actual Return |
|-------|----------|-----------------|-------------------|-------------------|---------------|
|   1   | $6,550 |  69 | 15.52% | $6,650.06 | 1.5% |
|   2   | $50    |   2 | 14.82% | $37.98 | -24.04% |
|   3   | $1,175 |  47 | 12.20% | $1,323.14 | 12.61% |
|   4   | $1,350 |  44 | 10.60% | $1,463.51 | 8.41% |
|   5   | $250   |  10 |  7.18% | $257.31   | 2.92% |
|   6   | $7,625 |  73 | 15.51% | $8,620.17 | 13.05% |
|   7   | $6,700 | 122 | 14.27% | $7,413.55 | 10.65% |
|   8   | $1,575 |  56 | 13.92% | $1,743.42 | 10.69% |
|   9   | $650   |  15 | 19.65% | $746.72   | 14.88% |
|  10   | $950   |  38 | 14.46% | $1,074.92 | 13.15% |
|  11   | $1,575 |  50 | 10.58% | $1,741.53 | 10.57% |


The later rounds of funding do much better than my initial round, but you can
see that none of them live up to the "weighted average rate" that helped define
the portfolio when you're selecting loans to fund.
Even more damning, the "actual return" I calculated is simply the payments
received divided by the original amount invested; I am not taking into account
the fact that my money was tied up over several years.
(For reference, the average rate of return on a *yearly* basis for the overall
stock market is about 7%.)

## Overall Numbers

I initially put in $20,000 which I used to fund loans in the first six months
that I was on the Lending Club platform.
For another year, I would take the proceeds and fund even more loans, which I
think helped to boost the numbers a bit.
Over the years, I've withdrawn $23,343.20, giving me a 16.72% overall return
spread over over several years (I think most of the loans were three year
loans).

The reporting on the site makes all this a little hard to see.
Lending Club calculates my average yearly rate of return at 6.53%.
According to their data, I am right in the middle of the pack compared
to other investors.
(Given how badly their automated picking of notes performed for me, I wonder
how other investors decided to pick their notes.)

{% include image.html
  name="performance.png"
  caption="Comparison against similar portfolios"
%}

There is clearly some room to make money on this platform.
Other blogs describe timing your funding with when new opportunities are
added to the platform so that you get the best pick of loan applicants.
Also, charge offs tend to level off after the first six months, so there's
an opportunity to by notes from other lenders.
My feeling in the end was that specialized (and automated) tools are necessary
for an investor to make a rate of return better than the stock market on the
Lending Club platform.
The company has made some of these tools, which I haven't tested, but my
own experiences left me wanting to move on to other opportunities.

