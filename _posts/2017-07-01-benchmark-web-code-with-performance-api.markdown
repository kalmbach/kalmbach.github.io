---
layout: post
title: "Benchmark Web Code with Performance API"
date: 2017-07-01 17:28:30 -0300
categories: javascript
---

From [developer.mozilla.org][mdn]
> The `Performance` interface represents timing-related performance information for the given page.

An object of this type exists in `window.performance`, as a read-only attribute.

From this object, we will use the `performance.now()` method, which returns a timestamp measured
in milliseconds with microsecond precision. By taking a timestamp before and after the code we want 
to measure, we can report how long it took to execute.

**Syntax**
{% highlight javascript %}
t = performance.now();
{% endhighlight %}

**Example of use**
{% highlight javascript %}
var t0 = performance.now();
doTask();
var t1 = performance.now();
console.log("Task took " + (t1 - t0) + " milliseconds.");
{% endhighlight %}

To avoid boilerplate code, we can write our own measure function with two parameters, the task 
we want to measure and an optional _tag_ if for example, the task is an anonymous function and 
we want a more meaningfull report.

{% highlight javascript %}
function measure(task, tag) {
  if (typeof(task) === "function") {
    var t0 = performance.now();
    task();
    var t1 = performance.now();

    var taskName = tag || task.name;
    console.log(taskName + " took " + (t1 - t0) + " milliseconds.");
  }
}
{% endhighlight %}

Now, we can run that piece of code several thousands of time to get an average of his execution
time. Having a rudimentary benchmark implementation:

{% highlight javascript %}
function measure(task) { } // returns (t1 - t0)

function report(task, tag, time) {
  var name = tag || task.name;
  time = time || meassure(task);

  console.log(name + " took " + time + " milliseconds.");
}

function benchmark(task, tag, rounds) {
  rounds = rounds || 50000;
  var accumulator = 0;

  for (var i = 0; i <= rounds; i++) {
    accumulator += meassure(task);
  }

  var time = accumulator / rounds;
  report(task, tag, time);
}

{% endhighlight %}

I find this really helpfull to quickly meassure how long a portion of code is taking. 
An mvp of the code discussed can be found here [kalmbach/benchmark][github] 

[mdn]: https://developer.mozilla.org/en-US/docs/Web/API/Performance
[github]: https://github.com/kalmbach/benchmark
