---
layout: post
title:  "Xcode project git hooks"
date:   2015-12-08 10:00:00
categories: xcode git testing tools
---
## What are git hooks?
Git has ability to run special script along with standard git commands. There are many uses for git hooks from enforcing code style before commit to deploying on push, etc. See more on [http://githooks.com](http://githooks.com). 

### Git pre-commit hook
Git pre-commit hook can help developer accidentally committing temporary changes to the source code made for debugging or testing.

## Example of git pre-commit hook
Xcode projects that use [Specta](https://cocoapods.org/pods/Specta) or [Quick](https://cocoapods.org/pods/Quick) can take advantage of focus tests. Focus tests allows to focus on a subset of a test suite. See [how temporarily run a subset of focused examples in Quick](
https://github.com/Quick/Quick/blob/master/Documentation/QuickExamplesAndGroups.md#temporarily-running-a-subset-of-focused-examples).
Focus tests speed up Test Driven Development as we can focus on one failing spec at a time. The only problem with focus tests, they require source code modification that we might commit and limit our ability to run full suite of tests. Here is a blog post on [Git Pre-Commit Hooks and Specta’s Focused Examples](http://spin.atomicobject.com/2014/12/21/git-pre-commit-hook-specta/). Unfortunately git hooks are not part of the repository. If we want to share git hooks on the project between multiple developers or even between multiple computers we have to install hooks on each clone.

## Git hook manager
One way to share git hooks across multiple clones is to use tool like [overcommit](https://github.com/brigade/overcommit) . Overcommit is "A fully configurable and extendable Git hook manager". It adds `.overcommit.yml` to configure hooks and allows adding custom hooks to `.git-hooks/` folder in the project root folder.

## Overcommit and Xcode
To make overcommit available for Xcode inside IDE we have to install overcommit gem at a system level:

{% highlight text %}
rvm system
gem install overcommit
{% endhighlight %}

If you are not using [RVM](https://rvm.io) - Ruby Version Manager you can skip first line and run this in shell:

{% highlight text %}
gem install overcommit
{% endhighlight %}

## Xcode pre-commit hook for Focused Examples
Here is the gist of a pre-commit hook that checks tests for temporary focused examples:

{% gist paulz/e8ff07bdb390ea40c4dd %}

To install:

  1. update `.overcommit.yml` with the above lines
  2. save above script to `.git-hooks/pre_commit/no_focused_examples.rb`

Now when we try to commit a focused test we get an error message similar to:
{% highlight text %}
Running pre-commit hooks
Checking for temporary focused examples...........[NoFocusedExamples] FAILED
UnitTests/SomeSpec.swift:108 has focused it block `fit`

✗ One or more pre-commit hooks failed
{% endhighlight %}

Same message will be shown by Xcode when we try to commit from IDE:
![Xcode error screenshot]({{ site.url }}/assets/xcode-pre-commit-hook-error-message.png)

What hooks would you add for your Xcode project?