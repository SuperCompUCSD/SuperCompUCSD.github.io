---
title: "Advent of Supercomputing: Day 24"
date: 2023-12-24
publishDate: 2023-12-24T08:00:00Z
author: ["matei"]
draft: false
---

# Web API's aren't so nice anymore

You know, back in the olden days of yore, when you pinged some random API route
for a website, you had a 90% chance that the returned result was just some
random text. It was awesome, you could just pipe that input to your shell, use
`awk` to filter out spaces and words, etc, and then you'd be able to script a
ton of stuff.

But then people had to get fancy. Now everyone uses "standardized data formats",
and while I absolutely agree that's the way it should be[^1], I just can't help
but reminisce about the "good old days" when the Internet was a Wild West of
random resources to mess around with.

But hey, everyone uses JSON now, thankfully, so at least we can build some kind
of JSON parser whenever we need to work in our shell scripts with web API routes
and call it a day.

# Wait, JSON?

Yes, the **J**ava**S**cript **O**bject **N**otation.

It's a nigh universal response format at this point in the world of the web.
This isn't only because it just so happens that JavaScript is the only web
language you can possible have in a browser[^2], but also because JSON is quite
human-readable for most use-cases:

Here's a quick example:
```json
// This is all untyped, don't expect freebies from JavaScript on type safety.
{
    "name": "Anonymous Cat",
    "age": 23,
    // Array of any element.
    "likes": [
        "fish",
        "hooman",
        42
    ],
    // A map of [key] -> value.
    "information": {
        "superLike": "alone time",
    }
}
```

That being said, this one is potentially manageable to parse and pull data from.
Imagine doing this for a 3,000-line JSON file. This is just an example I've found,
I _assure_ you I've seen worse ones:

```sh
>>> curl "https://jsonplaceholder.typicode.com/comments" | wc -l
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  154k    0  154k    0     0  3225k      0 --:--:-- --:--:-- --:--:-- 3277k
    3501
```

At the end of the day, it's just really hard writing your own JSON parser,
especially for a rather unspecified spec descending from YAML.[^3]

So naturally someone else did this already. This is the world of free
open-source software we're talking about.


# Meet `jq`

A couple of days back, I showed you a cool way to check the weather using `wttr.in`.
Let's do that again, but with the JSON format qualifier, since that gives _all_ the
weather data, not just a brief description/view:

```sh
>>> curl https://wttr.in/San+Diego?format=j1
{
    "current_condition": [
        {
            "FeelsLikeC": "18",
            "FeelsLikeF": "65",
            "cloudcover": "75",
            "humidity": "73",
            "localObsDateTime": "2023-12-23 12:19 PM",
            "observation_time": "08:19 PM",
            "precipInches": "0.0",
            "precipMM": "0.0",
            "pressure": "1018",
            "pressureInches": "30",
            "temp_C": "18",
            "temp_F": "65",
            "uvIndex": "4",
# --->8--- OUTPUT SNIPPED, IT'S A LOT --->8---
```

So, uhhh, it's a lot. Like 100+ lines a lot. I'm lazy. We should just first install `jq`:
```sh
brew install jq
```

There exists a version on Ubuntu, Arch Linux, Windows(?), and any other OS you likely use.

Now, we have our JSON data, but it's _a lot_. We could just look at the current
condition rather easily by asking `jq` to just access the specific key via the `.` operator:

```sh
>>> curl https://wttr.in/San+Diego?format=j1 | jq ".current_condition"
[
  {
    "FeelsLikeC": "18",
    "FeelsLikeF": "65",
    "cloudcover": "75",
    "humidity": "73",
    "localObsDateTime": "2023-12-23 12:19 PM",
    "observation_time": "08:19 PM",
    "precipInches": "0.0",
    "precipMM": "0.0",
    "pressure": "1018",
    "pressureInches": "30",
# --->8--- OUTPUT SNIPPED --->8---
    "windspeedKmph": "13",
    "windspeedMiles": "8"
  }
]
```

I mean, that's helpful? Ish, I guess. I need to get rid of that array around the object
because it looks rather unclean. Plus it's hard to work with. Guess I'll just access
the 0'th element of this array. We could pipe the new result to another `jq` call, or
adjust the one we have:

```sh
>>> curl https://wttr.in/San+Diego?format=j1 | jq ".current_condition[0]"
{
  "FeelsLikeC": "18",
  "FeelsLikeF": "65",
  "cloudcover": "75",
  "humidity": "73",
  "localObsDateTime": "2023-12-23 12:19 PM",
  "observation_time": "08:19 PM",
  "precipInches": "0.0",
  "precipMM": "0.0",
  "pressure": "1018",
  "pressureInches": "30",
  "temp_C": "18",
  "temp_F": "65",
  "uvIndex": "4",
  "visibility": "16",
  "visibilityMiles": "9",
  "weatherCode": "116",
  "weatherDesc": [
    {
      "value": "Partly cloudy"
    }
  ],
  "weatherIconUrl": [
    {
      "value": ""
    }
  ],
  "winddir16Point": "S",
  "winddirDegree": "170",
  "windspeedKmph": "13",
  "windspeedMiles": "8"
}
```

Epic! Now what if I just want the temperature in Celsius?
```sh
>>> curl https://wttr.in/San+Diego?format=j1 | jq ".current_condition[0].temp_C"
"18"
```

Look at that, reduced a giant pile of JSON to a very specific piece of
information. Amazing!

As you can imagine, `jq` is rather powerful. Here's a couple of other things that
it does easily:
- Filter, map and reduce elements from a JSON array or others!
- Merge multiple JSON objects into a singular one, clobbering keys in whichever
  order you want.
- Regex filtering?!

I don't know, there's a lot you can do. You might find a use for `jq` in pretty
much any side-project involving using a modern website's API. Nifty stuff, I must say.

There are also a few extra cool packages that I found [from this GitHub repo](https://github.com/fiatjaf/awesome-jq),
but here are the highlights:
- [JQ-Play](https://jqplay.org/), an online "playground" for `jq` on any arbitrary JSON data
- [gojq](https://github.com/itchyny/gojq), [xq](https://github.com/MiSawa/xq)
  and [jq.js](https://github.com/mwh/jqjs), all libraries implementing the `jq`
  query language into your favorite programming language. I only listed the
  packages for Go, Rust and JavaScript. Yes, I get the irony of that one.
- [An extension for VSCode creating a `jq` playground](https://github.com/davidnussio/vscode-jq-playground), in case
  you're squeamish about handing sensitive data to other websites.

As a final note, `jq` was highly influential in the web community for its
simplified query syntax whilst parsing JSON, so you'll find it around in other
libraries. In general, if you want to query stuff from JSON data fast, `jq` is
_probably_ your best bet, whatever language or environment you're working in.

[^1]: Even though it took literal decades until _almost_ everyone universally
    agreed to use JSON. There were many steps in between: XML, RPC, I don't even
    know the rest.
[^2]: Barring using WebAssembly, but we don't talk about that yet.
[^3]: Yes, I am not lying; this is legit and something I struggle with on occasion.