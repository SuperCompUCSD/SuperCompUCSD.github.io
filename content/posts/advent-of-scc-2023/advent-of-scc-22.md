---
title: "Advent of Supercomputing: Day 22"
date: 2023-12-22
publishDate: 2023-12-22T08:00:00Z
author: ["matei"]
draft: false
---

# "Hey Siri, what's the weather like?"

I scream that a lot, and Siri may or may not respond adequately[^1]. Whenever
it so happens that she doesn't even respond, and I still want to know what the weather
will be like, I have two options. Either:

- Stop being lazy, get my phone out and check the weather
- Open a browser tab and check the weather.

The first option is obviously off the table (literally, because my phone is
usually not at the table). But the second option is _so_ uncool! I mean,
_anyone_ can just open a browser tab and check the weather. But hey, we're in
the Supercomputing Club! There has to be a nerdier way to check your weather.

Of course there is. So let's try something cool out.

# What's the weather in San Diego like?

Or rather...what's the `wttr.in/San+Diego` like? 😉

Yes, there is a website that you can check to get the weather quite easily named `wttr.in`.
On its own, it's just option two. But what if I told you they care about terminals?

Try it out:
```sh
storm_firefox1 on Storm Prism   on ☁️   
>>> curl "https://wttr.in/San+Diego"
Weather report: San+Diego

     \  /       Partly cloudy
   _ /"".-.     18 °C          
     \_(   ).   ↖ 13 km/h      
     /(___(__)  16 km          
                0.0 mm         
                                                       ┌─────────────┐                                                       
┌──────────────────────────────┬───────────────────────┤  Thu 21 Dec ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│     \   /     Sunny          │  _`/"".-.     Patchy rain po…│  _`/"".-.     Patchy rain po…│  _`/"".-.     Patchy rain po…│
│      .-.      17 °C          │   ,\_(   ).   19 °C          │   ,\_(   ).   18 °C          │   ,\_(   ).   18 °C          │
│   ― (   ) ―   ↖ 8-12 km/h    │    /(___(__)  ↑ 15-19 km/h   │    /(___(__)  ↖ 17-24 km/h   │    /(___(__)  ↖ 8-13 km/h    │
│      `-’      10 km          │      ‘ ‘ ‘ ‘  9 km           │      ‘ ‘ ‘ ‘  10 km          │      ‘ ‘ ‘ ‘  10 km          │
│     /   \     0.0 mm | 0%    │     ‘ ‘ ‘ ‘   0.2 mm | 100%  │     ‘ ‘ ‘ ‘   0.1 mm | 100%  │     ‘ ‘ ‘ ‘   0.0 mm | 78%   │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
                                                       ┌─────────────┐                                                       
┌──────────────────────────────┬───────────────────────┤  Fri 22 Dec ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│  _`/"".-.     Light rain sho…│  _`/"".-.     Patchy rain po…│  _`/"".-.     Light rain sho…│  _`/"".-.     Patchy rain po…│
│   ,\_(   ).   16 °C          │   ,\_(   ).   17 °C          │   ,\_(   ).   16 °C          │   ,\_(   ).   16 °C          │
│    /(___(__)  ↖ 16-28 km/h   │    /(___(__)  ↖ 7-9 km/h     │    /(___(__)  → 7-9 km/h     │    /(___(__)  ↘ 12-17 km/h   │
│      ‘ ‘ ‘ ‘  10 km          │      ‘ ‘ ‘ ‘  9 km           │      ‘ ‘ ‘ ‘  10 km          │      ‘ ‘ ‘ ‘  10 km          │
│     ‘ ‘ ‘ ‘   1.5 mm | 100%  │     ‘ ‘ ‘ ‘   0.5 mm | 100%  │     ‘ ‘ ‘ ‘   0.3 mm | 100%  │     ‘ ‘ ‘ ‘   0.1 mm | 100%  │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
                                                       ┌─────────────┐                                                       
┌──────────────────────────────┬───────────────────────┤  Sat 23 Dec ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│     \   /     Sunny          │     \   /     Sunny          │  _`/"".-.     Patchy rain po…│    \  /       Partly cloudy  │
│      .-.      15 °C          │      .-.      17 °C          │   ,\_(   ).   16 °C          │  _ /"".-.     16 °C          │
│   ― (   ) ―   ↖ 8-11 km/h    │   ― (   ) ―   ↑ 13-15 km/h   │    /(___(__)  ↑ 12-17 km/h   │    \_(   ).   ↑ 8-11 km/h    │
│      `-’      10 km          │      `-’      10 km          │      ‘ ‘ ‘ ‘  10 km          │    /(___(__)  10 km          │
│     /   \     0.0 mm | 0%    │     /   \     0.0 mm | 0%    │     ‘ ‘ ‘ ‘   0.0 mm | 83%   │               0.0 mm | 0%    │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
Location: San Diego, San Diego County, California, United States of America [32.7174209,-117.1627714]

Follow @igor_chubin for wttr.in updates
```

This is me literally just now running the command. `wttr.in` is an amazing
website that works with any location. You can get:

- Your current location's weather with just `curl https://wttr.in`
- Any city's weather with just `curl https://wttr.in/City+Name` (use `+` instead
  of spaces, because URL's hate spaces for some reason)
- A random attraction's weather with just `curl https://wttr.in/~Attraction+Name` (such as the `https://wttr.in/~San+Diego+Zoo`)
- An airport's weather with just their IATA code: `curl https://wttr.in/san` or `curl https://wttr.in/lax`

All in all, amazing little service that you can just check in the terminal
whenever you feel like. Not to mention the service boasts a ton of little
additional features. For instance, there's JSON support for your scripting needs:

```sh
>>> curl https://wttr.in/San+Diego?format=j1
{
    "current_condition": [
        {
            "FeelsLikeC": "16",
            "FeelsLikeF": "61",
            "cloudcover": "75",
            "humidity": "78",
            "localObsDateTime": "2023-12-21 07:18 AM",
            "observation_time": "03:18 PM",
            "precipInches": "0.0",
            "precipMM": "0.0",
            "pressure": "1015",
            "pressureInches": "30",
            "temp_C": "16",
            "temp_F": "61",
            "uvIndex": "5",
            "visibility": "16",
            "visibilityMiles": "9",
            "weatherCode": "116",
            "weatherDesc": [
# --->8--- snipped --->8---
```

There are honestly INFINITY options available for `wttr.in`, and there's a lot
of nifty small little use-cases you wouldn't think of, like outputting weather
via one-line support to add to your window manager bar or just your shell
prompt. There's map rendering, custom formats you've heard little of before,
like for instance even Prometheus metrics support[^2].

Anyway, if you want to find out every possible feature, check out
[the `wttr.in` GitHub page](https://github.com/chubin/wttr.in) for all of the deets.
Hope you enjoy finding a geeky way to check the weather!.

[^1]: More often than not it's evidently inadequate.

[^2]: Don't worry if you don't know what that is, just know that it's very
    esoteric and funny. If you wanna know, it could be a story for another time.