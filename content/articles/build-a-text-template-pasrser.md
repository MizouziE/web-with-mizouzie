---
title: Build a Text Template Parser
description: A simple tutorial of how to make a text template parsing 
draft: true
date: "2023-03-02"
categories:
- web development
tags:
- web dev
- PHP
- tutorial
ogimage:
- /images/text-template-parser.png
- 1024
- 714
---

![A text template parsing bot](/images/text-template-parser.png)

## What is a text template parser?

After numbers, text has to be the most commonly dealt with data type in web and software development. It is how we humans communicate with one another and so developers find themselves dealing with text all the time.

A text template parser is a tool that can take a set **template** of text which has a number of specially marked locations with in indicating where and which particular variables need to be inserted. It is a perfect tool for sending mass emails, for example, where the sender wants to personalise each individual email with the recipients name or some other personal detail that they have a record of.

Usually, programming languages will have a library available to perform this task to make it easy for developers, so it's not really required to think too much about the ins-and-outs. Today, however, I needed to write one quickly and was amazed at how little was needed to get the desired result.

## Libraries

As mentioned, a lot of languages have a library for this type of thing and I think the one in Golang is a good example to show.

```go
    // Define a template.
    const letter = `
Dear {{.Name}},
{{if .Attended}}
It was a pleasure to see you at the wedding.
{{- else}}
It is a shame you couldn't make it to the wedding.
{{- end}}
{{with .Gift -}}
Thank you for the lovely {{.}}.
{{end}}
Best wishes,
Josie
`
```

The above is one of the first template libraries I used and this example stuck with me as the best illustration of what it was and how it worked. It is an email/letter and it uses `{{ curly braces }}` to denote where different variables go and even some conditionals.

In fact, the site builder [Hugo](https://gohugo.io) that I use for this very site uses this templating library and it's speed of converting a page of these curly braces into the html you are reading now is astonishing.

## Tutorial

This brings me to this tutorial. I was working on a project that was first attempted in Go, using the above mentioned library, that is now being re-made as a Laravel application and therefore uses PHP, my first learnt language. It came to the part that required the use of this library, where before I hadn't thought twice about importing and using the module, and as I was more comfortable thinking in PHP I realised that it was not all that difficult to achieve the desired output with a relatively small function.

So let's go through it!

### use the same layout

Having used the curly brace syntax of the Go library, I decided to stick with it. Let's create a template to work with:

```php

$template = 'Write me a poem about a {{ mood }} {{ animal }} that is {{ action }}'

```

Now we need to specify the search targets and then some values to replace them with:

```php

$search = [
    'mood',
    'animal',
    'action'
]

$values = [
    [
        'happy',
        'duck',
        'swimming'
        ],
    [
        'sad',
        'cow',
        'eating'
        ],
    [
        'tired',
        'cat',
        'waiting'
        ],
]

```

### choose the function

At first I made this with `str_replace()` which served well, but it required some concatenation:

```php

public function parse() 
{
    return str_replace()
}



```

### what are the inputs



### iterate