---
title: 'Higher-Order Promises'
tags:
    - promises
author: brian d foy
images:
  banner:
    src: '/static/1280px-Fractal-Cauliflower.jpg'
    alt: 'fractal cauliflower'
    data:
      attribution: |-
        <a rel="nofollow" class="external text" href="https://www.flickr.com/photos/joeshlabotnik/3059554662/">Image</a> by <a href="https://www.flickr.com/photos/joeshlabotnik/">Joe Shlabotnik</a> <a href="https://creativecommons.org/licenses/by-sa/2.0" title="Creative Commons Attribution-Share Alike 2.0">CC BY-SA 2.0</a>
data:
  bio: briandfoy
  description: 'Create new, complex Promises by composing Promises'
---

## Create new, complex Promises by composing Promises

Mojolicious 7.49 added an its own implementation of the [Promises/A+ specification](https://promisesaplus.com). mohawk wrote about these in [Day 14: You Promised To Call!](https://mojolicious.io/blog/2017/12/14/day-14-you-promised-to-call/) of the 2017 Mojolicious Advent Calender where he showed you how to fetch many webpages concurrently. This Advent entry extends that with  more Promise tricks.

---

A Promise is a structure designed to eliminate nested callbacks (also known as ["callback hell"](http://callbackhell.com)). A properly written chain of Promises has a flat structure that easy to follow linearly.

A higher-order Promise is one that comprises other Promises and bases its status on them.

## All

An "all" promise resolves only when all of its Promises also resolve. If one of them is rejected, the "all" Promise is rejected. This means that the overall Promise knows what to do if one is rejected and it doesn't need to know the status of any of the others.

## First come, first served

A "race" resolves when the first Promise is no longer pending and after that doesn't need the other Promises to keep working.

## Any

An "any" Promise resolves immediately when the first of its Promises resolves.

## Some

A "some" Promise resolves when a certain number of its Promises resolve.

## None

A "none" Promise resolves when all of the its Promises are rejected.

