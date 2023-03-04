---
title: Common Function Comparisons in PHP
description: 
draft: false
date: "2023-03-04"
categories:
- web development
tags:
- web dev
- PHP
- learning
ogimage:
- /images/php-function-comparison.png
- 605
- 485
---

![Making up your mind which PHP function to use](/images/php-function-comparison.png)

## PHP Functions

PHP is an arsenal of tools that can process data in a multitude of ways. This is one of the reasons that it powers ~75% of website server-side code. It is fast, has huge support and scales very well.

If you were to investigate how many core functions PHP has on v8.1.2 you would find:

```sh
>>> $funcs = get_defined_functions();
=> [
     "internal" => [
       "zend_version",
       "func_num_args",
       "func_get_arg",
       "func_get_args",
       "strlen",
       "strcmp",
       "strncmp",
       "strcasecmp",
       "strncasecmp",
       "error_reporting",
       "define",
       "defined",
       "get_class",
       "get_called_class",
       "get_parent_class",
       "is_subclass_of",
       "is_a",
       "get_class_vars",
       "get_object_vars",
       "get_mangled_object_vars",
       "get_class_methods",
:

>>> echo count($funcs['internal']);
=> 1735
```

That is not a small amount by any means, so it is impossible to know them all. Even the most experienced PHP developers will regularly refer to [php.net](https://www.php.net/) to look up which they can use for what.

With 1735 functions being available, surely some must overlap or be similar. In this discussion we will look at the ones that seem like they could be the same thing and see if we can decide best use cases for each.

### Most commonly used

Looking through [Exakat](https://www.exakat.io/en/the-100-php-functions-in-2022/)'s _top 100 php functions 2022_, we can see a few likely candidates to choose from. Lets take:

- [str_replace()](https://www.php.net/manual/en/function.str-replace)
- [implode()](https://www.php.net/manual/en/function.implode)
- [count()](https://www.php.net/manual/en/function.count)
- [substr()](https://www.php.net/manual/en/function.substr)
- [explode()](https://www.php.net/manual/en/function.explode)
- [trim()](https://www.php.net/manual/en/function.trim)
- [strtr()](https://www.php.net/manual/en/function.strtr)
- [array_merge()](https://www.php.net/manual/en/function.array-merge)

> <sup>click on any to see the official definitions<sup>

## Compared to their closest cousins

### str_replace() vs preg_replace()

The same, but regular expressions can make it more precise (and complicated!).

### implode() vs join()

Join is actually just an alias, so they're exactly the same. Implode works like running:

```php
function implode(string $separator, array $arr): string
{
    $imploded = '';

    foreach ($arr as $index => $str) {

        switch ($index) {
            case !0:
                $imploded .= $separator;
            default:
                $imploded .= $str;
                break;
        }
    }

    return $imploded;
}
```

Going through the steps above, it does something along the lines of:

1. Initiate an empty string variable as `$imploded`
1. Loop through the input array
1. If the element of the array is at index 0, **do not** concatenate the separator to `$imploded`
1. Otherwise concatenate given separator string
1. Concatenate element to `$imploded`
1. Repeat until all elements of array have been cycled
1. Return fully constructed string `$imploded`

### count() vs if (!count()) {}

This function has such high popularity due to it being used commonly for frontend "decision making". When choosing whether or not to display a particular component depending on there being any instances of something in the backend database, developers will call count() on a parameter passed to the view being displayed. The idea is that if it returns a "truthy" value, as in **not** 0, then it confirms the existence of at least one instance that parameter in the database.

This can be fine for a single call and if there is not an enormous number of instances of that parameter. Often though, there will be a large number of conditional components which means multiple calls to count() which ends up counting an enormous amount of your database. This is not ideal for UX as it can make the page load excruciatingly slowly. There is a simple optimization demonstrated below:


```php
// Rather than calling count() on everything like so...
<?php if ($param->count()) { ?>
  <div>Show this example HTML component!</div>
<?php } else { ?>
  <div>Show alternate HTML or Nothing 🤷</div>
<?php } ?>

// ...we can check NOT == 0 like this instead
<?php if (!$param->count()) { ?>
  <div>Show alternate HTML or Nothing 🤷</div>
<?php } else { ?>
  <div>Show this example HTML component!</div>
<?php } ?>

```

The optimized second option flips the original and checks only for a **non-falsey** value which is satisfied by the very first counter beyond 0. After that first 1 is counted, the rest of the code can move on and display the non-existent case or the existent case accordingly. We are effectively only "counting" a maximum of the number of calls to count as opposed to counting the number of calls to count **multiplied** by the number of instances of the counted records in the database. That time can add up! Count one or zero and move on.

### substr() 

### explode() vs str_split() vs str_tok()

These functions are all about creating an iterable out of a given string

### trim() vs ltrim() vs rtrim() vs preg_replace()

### strtr() vs str_replace()

### array_merge() vs +=

## Summary

Now that we have looked at some of the most common similarities and differences, I hope you will consider yourself well armed and well informed on the great choice of incredibly powerful weapons at your disposal!

_Now go slay some data._

![choose your weapon](/images/choose-your-weapon.png)