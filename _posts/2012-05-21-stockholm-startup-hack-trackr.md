---
title: Stockholm Startup Hack — Trackr!
author: Richard
layout: post
categories:
  - Uncategorized
---
![Trackr](/images/wp/stockholm-startup-hack-trackr.png "Trackr")

Last weekend [me][1], [Jonas][2] and [Martin][3] spent 12 hours hacking at the [Startup Hack][4] in Stockholm. It was a great hackathon, and we actually managed to get some kind of a product out!

At breakfast we discussed our initial idea to create some kind of viral, mobile friendly, Facebook enabled, lame game. But the idea for the game was too bad to present to the sophisticated crowd (and also, we can’t really program games), so we had to come up with something new, really fast.

## Trackr!

The new plan was to create something useful we could use in the office — realtime website stats! We currently use the great service [Chartbeat][5] to track number of visitors currently online, top referrers and other useful stats. But once you want to track more than 1,000 concurrent users the service becomes quite expensive.

We created something similar; using a tracker script inserted into the customer web page, we poll active users every 5 second by generating an image element pointing to our app. This will require a fast, asynchronous backend to handle the traffic from multiple sites!

## The Setup

Hosting on [Heroku][6] can get you quite far for free, perfect for our lab! The services we choose to get up and running was:

*   Tracker app with Thin webserver running [async_sinatra][7] to track visitors asynchronous. Exposes a JSON API.
*   Redis, perfect for storing and generating quick reports of “online visitors”. [Redis To Go][8] is free for 5 MB memory!
*   Simple frontend/reports app running Rails to “signup customers” and view your report.
*   [Unicorn][9] web server for the reports to run multiple processes in the single, free Heroku dyno. (The realtime graph needs to update every second or so.)
*   Great chart lib [Highcharts][10] to visualize our live stats!

## The result

Our simple frontend app should actually work if you want to try it out! Its currently hosted at <http://trackrapp.herokuapp.com/>.

We tried this on Mynewsdesk, and the results are quite good! The low capacity of a free Heroku app, and 5 MB limit in Redis makes the reports not perfect (actually, only about half the traffic is being reported…). But in the current state I think the app could accurately track a website with fewer online visitors than Mynewsdesk (1000+ in peak hours).

Feel free to [try it out][11]!

 [1]: https://twitter.com/richardjohansso/
 [2]: https://twitter.com/himynameisjonas
 [3]: https://twitter.com/martinsvalin
 [4]: http://startuplocation.com/hack
 [5]: http://chartbeat.com/
 [6]: http://www.heroku.com/
 [7]: https://github.com/raggi/async_sinatra
 [8]: http://redistogo.com/
 [9]: http://unicorn.bogomips.org/
 [10]: http://www.highcharts.com/
 [11]: http://trackrapp.herokuapp.com/