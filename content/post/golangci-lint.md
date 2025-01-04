---
title: "[25.3] golangci-lint strange behaviour"
date: 2025-01-03T00:00:00
summary: "golangci-lint consuming 30GB of memory"
draft: false
toc: false
readTime: false
autoNumber: true
showTags: false
tags: ["one-per-day"]
---

Earlier today I was coding on a project that uses `pre-commit`, and for a Go codebase is common that one of the step is to run `golangci-lint`. 
For some reason today it was taking forever (which is not a common behaviour, usually it's pretty fast), ended up the process was consuming 30 GIGABYTES!!!

One friend offered to help, and after some debugging we wasn't able to find anything different that would suggest that high memory usage.
We did what all programmers do when something is not working properly, we tried to update, and it worked!

The thing is, `golangci-lint` is not a [fan](https://github.com/golangci/golangci-lint/issues/4909#issuecomment-2297904093) of Go version updates, the project I am working on had a newer Go version than the one I used to install `golangci-lint`. 

Learning of the day, save yourself the trouble of debugging and looking for the problem, just update the tool and be happy! :D
