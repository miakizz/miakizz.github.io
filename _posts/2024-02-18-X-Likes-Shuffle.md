---
title: "X Likes Shuffle"
date: 2024-02-18 12:00:00 -0500
categories: [code-snippets]
---
This entire blog refresh was prompted by me wanting to post like, 5 lines of code onto social media and not being able to. So, here is that code.

I use X likes mainly as a way to save things for later. However, it's not a great system for that as they're not really indexed and difficult to access. I wanted a way to go back and look at old stuff I liked before. So, using the `likes.js` file in the GDPR export, this will shuffle your likes and open 20 in your browser. Have fun!
```
import json
import random
import webbrowser

# remove every thing before the first = before using script
# ancient ah JSON specs, twitter was written in the dark ages
file_path = "data/like.js"

# Open the file and load its contents
with open(file_path, "r") as file:
    data = json.load(file)
expanded_urls = [item['like']['expandedUrl'] for item in data]
random.shuffle(expanded_urls)
for i, url in enumerate(expanded_urls):
    webbrowser.open(expanded_urls[i])
    if i % 20 == 19:
        input("Press enter for next 20 urls, or ctrl+c to exit")
```