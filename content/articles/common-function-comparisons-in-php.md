---
title: Common Function Comparisons in PHP
description: 
draft: true
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

If you were to investigate how many core functions PHP comes with on v8.1.2 you would find:

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

$imploded = '';

 foreach ($arr as $str) {
    $imploded .= $str;
}

return $imploded;
```

### count() vs if (!count()) {}

Performance enhancing when only existence of 1 or more is fine.

### substr() 

### explode() vs str_split() vs str_tok()

### trim() vs ltrim() vs rtrim() vs preg_replace()

### strtr() vs str_replace()

### array_merge() vs +=

## Summary

Now that we have looked at some of the most common similarities and differences, I hope you will consider yourself well armed and well informed on the great choice of incredibly powerful weapons at your disposal!

_Now go slay some data._

![choose your weapon](/images/choose-your-weapon.png)