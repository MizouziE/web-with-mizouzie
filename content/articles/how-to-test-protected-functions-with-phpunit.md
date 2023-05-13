---
title: How to test protected functions with PHPUnit in your Laravel app
description: An essential part of developing an app is being able to test your code. My preferred method of testing my Laravel code is using PHPUnit and this is how I test those 'harder to reach' protected and private methods inside various classes.
draft: false
date: "2023-05-13"
categories:
- web development
tags:
- web dev
- laravel
- learning
- PHP
ogimage:
- /images/reflection.png
- 1280
- 720
---

![reflection classes](/images/reflection.png)

## Typical places this arises

Building an application in PHP with Laravel means that we can call on a great number of classes to do various parts of the overall process we wish to achieve. For the purposes of this example (and where I first came across this issue) we will look at writing tests for a Job Class. A job class is one which will be sent some data and then process that data in the background via a worker so that the application is free to continue processing other commands.

A job class will often contain methods and variables that are;

- public
- private
- protected

As you can imagine, public methods are easy enough to test against as they are "exposed" to the outside of the class instance. It gets a little more difficult to gain access to these private and protected methods for testing, as they are intended to be called only by other methods within the class instance. They'll be things like getters and setters so that a handle() function does not get clogged up with logic for retrieving it's necessary data.

## Error: Call to private Some\ExampleClass::__construct() from scope Tests\Feature\Some\ExampleClassTest

What this error message is telling us is that we are trying to invoke a __construct() method inside the `Some\ExampleClass`, which is not allowed from the scope of our test that may look something like the following:

```php
namespace Tests\Feature\Some;

class ExampleClassTest extends TestCase
{
    /** @test */
    public function my_job_does_something()
    {
        $job = new \Some\ExampleClass($input);

        // Make some assertions
        ...
    }
}
```

Just in that single line, we have already encountered the problem. If we look at the example class itself, we'll see what trips us up:

```php
<?php

declare(strict_types=1);

namespace Some;

final class ExampleClass
{
    private function __construct(
        public readonly int $input,
    ) {
    }
    ...
}
```

Our attention should be drawn to the `private function` part. That constructor called on initiation **CANNOT** be called from outside of the namespace `Some`, we are trying to call it from `Tests\Feature\Some\ExampleClassTest`.

## Error: Call to protected method Some\ExampleClass::exampleMethod() from scope Tests\Feature\Some\ExampleClassTest

This error is very similar to the above, just in this case we may have a `public function __construct()` so our class may be instantiated from outside, but our helper functions cannot be invoked outside of the `handle()` method. If we use the same example as above, we can get one step further before hitting a wall:

```php
namespace Tests\Feature\Some;

class ExampleClassTest extends TestCase
{
    /** @test */
    public function my_job_does_something()
    {
        $job = new \Some\ExampleClass($input);

        $result = $job->exampleMethod();

        // Make some assertions like...
        // $result->assertEquals('expected', $result);
        ...
    }
}
```

So the class is created with no problem, but we want to test methods that are called from _within_ the `handle()` method so we can be certain that the data being handled is the correct data.

## Solution: ReflectionClass

> Available in; PHP 5, PHP 7, PHP 8

### getConstructor()

### getMethod()

### setAccessible()

### invokeArgs()