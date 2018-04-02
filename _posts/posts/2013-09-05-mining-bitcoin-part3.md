---
layout: post
title: "Mining Bitcoin for Fun and (Basically No) Profit, Part 3: Mobile Development"
date: 2013-09-05 11:07:00
categories: posts
comments: false
en: true
description: "Mining Bitcoin for Fun and (Basically No) Profit, Part 3: Mobile Development"
keywords: "android, api, bitcoin, java, json, mining, programming"
author: mikedank
---

*If you have not done so already, please read parts [1](../mining-bitcoin-part1/) and [2](../mining-bitcoin-part2/) in this series.*

So I have a mining rig that's successfully rewarding me with bitcoins. Normal people would probably stop at this point. One nice thing about mining in Slush's Pool is that it has a handy email notification option that tells you when credit is being transferred to your Bitcoin wallet. This is pretty cool, but what if I want more in-depth information? For example, what if I want to know my hash rate, or if my miner is alive (did the system crash?) or how many bitcoins I have total?

The next step for me was to create a mobile application which could provide all this information - whenever or wherever I wanted it. So, I got to work.

The platform I chose to work with was Android. A logical choice for me as I had prior experience developing Android applications and own an Android phone myself. Programming for Android, as many know, means programming in Java. If you have any prior Java experience, you're already have a head start if you ever wanted to get into Android development.

A fantastic thing about Slush's Pool is that it offers an API (Application Programming Interface) which allows users to pull down information on their miners using the de facto JSON format. So from this I can get at my mining information, but what else do I want? I decided it would be wise to pull down the average value of a bitcoin in USD, at any given moment. This way, I can do some simple calculations to determine a rough estimate of how much I'm generating and getting payed in USD. Lastly, I wanted to get the balance of my Bitcoin wallet, again to be displayed in both Bitcoin and USD.

I already had the API information for Slush's Pool, as it is linked on everyone's profile and accessed via a common base url and unique key for each user. Here is an example of the JSON output for my account:

```
{
    username: "Famicoman",'
    rating: "none",
    confirmed_nmc_reward: "0.00000000",
    send_threshold: "0.01000000",
    nmc_send_threshold: "1.00000000",
    confirmed_reward: "0.00145923",
    workers: {
        Famicoman.worker1: {
            last_share: 1378319704,
            score: "70687.6038",
            hashrate: 1004,
            shares: 1906,
            alive: true
        }
    },
    wallet: "1DVLNHpcoAso6rvisCnVQbCFN8dRir1GVQ",
    unconfirmed_nmc_reward: "0.00000000",
    unconfirmed_reward: "0.00612688",
    estimated_reward: "0.00046390",
    hashrate: "1006.437"
}
```

Next, I needed an API for the average value of a bitcoin in USD. I went on the hunt. Finally, I found that [Mt. Gox](https://www.mtgox.com/), the largest Bitcoin exchange, has a public API for bitcoin rates (located at [this ticker URL](http://data.mtgox.com/api/1/BTCUSD/ticker)). This works perfectly for my needs. Here is some sample JSON output from this API:

```
{
    result: "success",
    return: {
        avg: {
        value: "143.97798",
        value_int: "14397798",
        display: "$143.98",
        display_short: "$143.98",
        currency: "USD"
        }
    }
}
```

So far, so good.

Lastly, I wanted wallet information. I discovered that [Blockchain](http://blockchain.info/) shows records of transactions (as they are all recorded in the block chain), so I did some probing and found they also offered an API for attributes of individual wallets ([Here's a link using my wallet info](http://blockchain.info/address/1DVLNHpcoAso6rvisCnVQbCFN8dRir1GVQ?format=json), it's all public anyway). This includes balance, transactions, etc. The units for bitcoins here is the Satoshi, one millionth of a bitcoin. Some sample JSON output from this service looks like so:

```
{
    hash160: "88fd52ba00f9aa29003cfc88882a3a3b69bfd377",
    address: "1DVLNHpcoAso6rvisCnVQbCFN8dRir1GVQ",
    n_tx: 7,
    total_received: 8434869,
    total_sent: 0,
    final_balance: 8434869,
}
```

So now I had the APIs I was going to use and needed to put them all together in a neat package. The resulting Android application is a simple one. Two screens (home and statistics) with a refresh button that pulls everything down again and recalculates any necessary currency conversions. Android does not allow you to do anything system-intensive on the main (UI) thread anymore, so I had to resort to using an asynchronous task that spawns a new thread. This thread is where I pull down all the JSON (in text form) and get my hands dirty manipulating the data. I utilize a 3rd party library called [GSON](https://code.google.com/p/google-gson/) to parse the data I need from the JSON string. Then, it's just a little bit of math and we have all the necessary data. After all of that is done, the application prints everything on the screen. Pretty basic, and with plenty of room for potential additions.

When running the application, provided there's network connectivity and all the servers are up, you will be rewarded with a screen like this:

{% raw %}<center><a href="/assets/img/2013-09-05-mining-bitcoin-part3-01.png"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2013-09-05-mining-bitcoin-part3-01-sm.png"></a><figquote>The application in action</figquote><br></center>{% endraw %}

Not too shabby. If you wanted to use it yourself, it would be necessary to hard-code your own key from Slush's pool. There doesn't appear to be an API call by username (by design), so it needs to be implemented manually at some point (which happens to be in code as of right now).

The source for this application, which I call *SlushPuppy*, is [freely available on GitHub](https://github.com/Famicoman/SlushPuppy). Feel free to fork it, or just download and mess around with it. If anything, it provides a small example of both Android-specific programming as well as API interaction.

