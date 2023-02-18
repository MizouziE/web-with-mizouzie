---
title: Enkle Word Game
dateMonthYear: April 2022
description:  A game built using JavaScript based on the same concepts as Wordle. Designed to be customizable for my young son to enjoy.
type: page
topic: project
# link: "https://enkle-word-game.github.io/enkle/"
image: "https://enkle-word-game.github.io/enkle/public/images/winner.png"
tags:
- JavaScript
- learning
- GitHub
hideMeta: true
---

# [Enkle Word Game](https://enkle-word-game.github.io/enkle/)

A copy of the popular word puzzle game Wordle made with JavaScript and hosted on a static site.

<ul class="post-tags"><li><a target="_blank" href="https://enkle-word-game.github.io/enkle/">Click here to play!</a></li></ul>

## The idea

Seeing that my young son was able to stare at his tablet screen for hours if not interupted, I wanted to make something to at least give him some sort of lesson while he played. All his games are simple, noisy and colourful. I found a nice tutorial on Laracasts that showed how to make a Wordle clone on the popular (and my favourite) framework Laravel, but after follwoing along I realised that the whole framework was unnecessary.

## Stripped down

I extracted all the JavaScript working logic from what I had built on the Laravel framework, and applied it to a standalone static site that could be hosted on GitHub pages for free.

## Built up

After getting it working in it's primitive form on a static site, and it was stable, I spent a little time developing it further and adding a few features so that my son wouldn't get bored ofit too quickly.

### Additions

Beyond the standard 3x5 grid that the laracast tutorial shows you, I added a few buttons and features behind the scenes;

- Add or reduce number of guesses, like a difficulty setting
- Choose the length of the word to guess from 3, 4 or 5 letters

Each addition required a little extra on the JavaScript side of things as well as increasing the size of the library of words that the app uses. It was an interesting challenge.

