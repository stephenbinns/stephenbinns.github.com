---
layout: post
title: "A better javascript console"
date: 2013-03-03 15:37
comments: true
categories: javascript
---

window.console is available on most browsers however the entire set of functions aren't always implemented. We have a choice, either we limit ourselves to logging in a single way or try and account for browser shortcomings.

We can make use of some useful javascript techniques to ensure that we can always call the methods we expect.

Its worth noting there are browsers which do not have a console logger, this is no problem to deal with:

```
window.console = window.console || false;
```

We're using a trick here, because window.console will always return a truthy value when the console property exists the value will always be false if it is not.

We can take this a stage further if we feel like and set up a log method instead of returning false

```
window.console = window.console || { log: function() { /* Do nothing */ } };
``` 

Great, so we can always be sure calling window.console.log is safe. Now lets look at wrapping the other methods.

```
var levels = ['debug','info','warn','error','fatal'];

for (var i in levels) {
  var level = levels[i];
  window.console[level] = window.console[level] || window.console.log;
}
```

We're using an alternative method to ensure that a functions exists for each of the levels specified, we use the square bracket syntax to check the presence of the function. We're using the same trick again to ensure that there is always a function attached to all the levels we'd wish to log at. 

You can view the full gist at [here](https://gist.github.com/stephenbinns/5076500)
