---
title: "[25.2] Unix commands for the win!"
date: 2025-02-01T00:00:00
summary: "leverage unix command"
draft: true
toc: false
readTime: false
autoNumber: true
showTags: false
tags: ["one-per-day", "unix"]
---

Some weeks ago, I was doing some automation and the script output format was separeted by `,`:

```py
Alice,30,New York
Bob,25,Los Angeles
Charlie,35,Chicago
```

The problem was that it was a job running on TC and for some regions the output size was really big, a size that I couldn't just scroll down and count.
To solve this problem I used the linux command `wc -l` to count the lines then proceed to use `sed` to run the it batches using `sed -n '30p'`, this means that I will get from the first line until the 30th, after I could do `sed -n '31,60p'` to execute the other next 30.

Sometimes all we need are some combination of unix commands and nothing more.

