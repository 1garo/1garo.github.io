---
title: "A quick conversation about go pointers."
summary: "I was wrong. You shouldn't always use receive pointers in your Go methods."
date: 2026-03-29T00:00:00
draft: false
showTags: true
tags: ["go"]
---

I had this habit for years, and even suggested to other teammates to do the same, but I was wrong.


I always make sure I am reading [A Semana Go](https://www.asemanago.dev/). The author of the newsletter created a video on Youtube named **The Go Way**, which he basically goes over a couple of patterns that he believe is valuable, and one of them was:

> If you are not gonna change the values, default the receivers to value and not pointers.


I was always someone that would suggest using pointers at all places possible on Go, my team knew that I would always be the one pointing out that a copy could be a pointer in our PR review process. At some point, they started asking, why? 

It was funny because I didn’t had a really strong and science-based answer. At that moment, I went with the expected response: you are not gonna want to have copies all over the place, passing copies to the functions and methods, etc. The answer was okay at the time, but I started to question myself, with a little help of not being able to give a proper response to my team, and the Youtube video was the last thing I needed to change this perspective.

The argument that Elton Minetto brings to the table is that:

> You will have better performance sticking to copies, pointers are to be used just when necessary. By sticking with values, you are not gonna need heap allocations, also, by not having pointers the garbage collector will have no job to keep track of that information to free when no one else refer to it.

Of course, there are cases where pointer receivers make perfect sense — if your method needs to modify the struct, or the struct is large enough that copying it is wasteful. The same goes for types with internal locks or other non-copyable fields (like sync.Mutex), or when you want consistent method sets across interfaces. The key is to use pointers when there’s a reason, not just by habit.

