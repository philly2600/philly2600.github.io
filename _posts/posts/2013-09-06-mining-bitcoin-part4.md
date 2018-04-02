---
layout: post
title: "Mining Bitcoin for Fun and (Basically No) Profit, Part 4: Aftermath"
date: 2013-09-06 08:38:00
categories: posts
comments: false
en: true
description: "Mining Bitcoin for Fun and (Basically No) Profit, Part 4: Aftermath"
keywords: "asic, bitcoin, exchange, mining"
author: mikedank
---

*If you have not done so already, please read parts [1](../mining-bitcoin-part1/), [2](../mining-bitcoin-part2/) & [3](../mining-bitcoin-part3/) of this series.*

As of writing this, I've spent one week running my setup with one USB Block Eruptor and one week running my setup with three. In my first week, I received about two payouts of 0.01 bitcoin each while in the second week I received that payout almost daily.

The current average Bitcoin rate in USD (as of this writing) is *$144.99322*. This means my payout, one hundredth of that value, is *$1.4499322*. Now, this doesn't sound like too bad of a payout. However, there is a lot to consider when figuring out whether or not I will actually make any money off of this in the long run.

First, we have to consider that the price of a bitcoin is constantly fluctuating. When I started this project, the exchange rate was *~$119.00* USD. This amount could change at any time as the value inflates or deflates. Next, we have to consider the change in mining complexity - as more people start mining, the harder it will be. This is not only a problem of competition, the difficulty of generating a block increases systematically every 2016 blocks (roughly two weeks) Thus, as time goes on, you'll make less money.

Aside from these variable rates, we have some constants to think about. The initial investment wasn't enough to break the bank, but it wasn't anything to ignore.

Recall our initial build list, this time with some prices:

* 1 x Raspberry Pi ($35 + $4.98 shipping = $39.98)
* 1 x ~4GB SD Card ($5.01 + $0 shipping = $5.01)
* 1 x Micro USB Cable ($2.60 + $0 shipping = $2.60)
* 1 x Network Cable ($5.49 + $0 shipping = $5.49)
* 1 x Powered USB HUB ($19.95 + $0 shipping = $19.95)
* n x USB Block Eruptor (($42.99 + $3.99 shipping) * 3 = $140.94)

**Total = $213.97 USD**

Pretty big when you put it all together, but this is worst case scenario - when you don't start with anything. I already had most of this around the house. Besides the USB Block Eruptors, I did need to purchase a USB hub, but I wouldn't consider this part of my investment as I needed one anyway (the project more or less gave me an excuse to get it). I'm more concerned with making back my money from the Block Eruptors, which total *$140.94* USD.

Next, we should consider power requirements. Again, this doesn't matter to me much, I'm just focused on earning back money for the USB Block Eruptors, but let's hook the whole rig up to my Kill A Watt electricity usage monitor and see what it says.

{% raw %}<center><a href="/assets/img/2013-09-06-mining-bitcoin-part4-01.jpg"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2013-09-06-mining-bitcoin-part4-01-sm.jpg"></a><figquote>Kill A Watt reading for kWh over 44 hours</figquote><br></center>{% endraw %}

The Kill A Watt states that the consumption is *0.55 kWh*, this was taken over a period of 44 hours. Now let's say our monthly electricity rate was 15 cents per kWh. We can plug all of those numbers into this handy formula: *0.55 kWh / 44 hours * 732 hours [hours in a month] * $0.15 [price per kWh] = $1.37* per month. So overall the power cost isn't too bad, especially compared to old GPU rigs.

Okay, now we know the power consumption, have our initial costs, are mindful of the changing rates, etc. How do we put it all together?

The [Genesis Block](http://thegenesisblock.com/) has created the [Mining Dashboard](http://mining.thegenesisblock.com/) just for this sort of thing. We can plug in all of our information here and see what's what. They do have some fields for power, but that doesn't take into account the Raspberry Pi and the hub. Plug in what matters to you. You cannot retroactively compute values, so I'll have to base my start in September. However, this doesn't take into account that I've already mined *$9.96* (in the current exchange rate), so I'll subtract that from my investment of *$140.94* to get *$130.98*. It's a dirty workaround, but this is an estimate after all. After putting in all the values, hit 'Calculate.' Here are my results:

{% raw %}<center><a href="/assets/img/2013-09-06-mining-bitcoin-part4-02.png"><img style="width: 80%; max-width: 300px; display: block; margin: 0 auto; border 0" src="/assets/img/2013-09-06-mining-bitcoin-part4-02-sm.png"></a><figquote>My Mining Dashboard projection</figquote><br></center>{% endraw %}

From the projection, I will never break even and will forever be $44 in debt because my setup will be completely obsolete in around 10 months time.

Now as I said, this is a projection but it's likely closer to being accurate than it is to inaccurate. I likely won't make my money back unless the value of a bitcoin continues to rise and/or the mining complexity grows at a slower rate (which is unlikely).

I'm not the only one in this boat. As more and more powerful ASIC rigs are being produced, the window for profit gets smaller and smaller. Some new ASICs sold now won't even be able to turn any profit for owners because the time between ordering and arrival leaves too small a window to mine back the initial investment at the current complexity.

While it is unfortunate to (likely) not turn a profit, this still proved to be a fun and incredibly interesting project. I may not have come out of it with financial wealth, but the ability to look down at my little Raspberry Pi chugging away (actually turning electricity into money, who knew?) was completely worth the time and effort I put into it. I'll likely end up sitting on the bitcoins I mine now for a little while, just like I did back when my wallet got its first deposit. I'm more infatuated with mining and collecting the currency than I am with spending it, at least for right now...

Hopefully you, one way or another, have learned something from my little journey.

I know I did.
