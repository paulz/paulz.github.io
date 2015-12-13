---
layout: post
title:  "Remove Xcode default header comment"
date:   2015-12-12 10:00:00
categories: xcode source code
---
## Xcode file template
Every new source file created from Xcode template has top comment similar to this:
{% highlight c %}
//
//  <file name>
//  <Name of project>
//
//  Created by <My name> on <Date>.
//  Copyright <Year and company>. All rights reserved.
//
{% endhighlight %}
It has little useful information, and often incorrect. As file name, project and company changes how can we keep in sync file comments? In Agile environment when we pair on shared computers the author name often set to generic 'Developer'. We also use version control system that can tell us the date when file was created along with all history of modifications. Let's just get rid of those boiler plate comments and have extra 7 lines on screen to see source code.

## Remove top comments from all sources
To clean up your Xcode project from boiler plate comments use Find and Replace. Within Xcode Find the following regular expression and replace it with empty string:

```//\n//.+\n//.+\n//\n//.+\n//  Copyright.+$\n//\n\n```

This will also get rid empty line after the header and allow the source to be the first line in the files.

## Change Xcode templates to have no header comments
Here is a bash script I found thanks to [Manav Rathi](https://github.com/mx4492). You can save it in your home directory, follow the instructions in comments and delete after use.
{% gist paulz/d17b5bec7e09d622d6dd %}
We have to do this on all workstations for every Xcode installation.

## Git pre-commit hook
To prevent accidental checkin of sources with boilerplate comments we can create git pre-commit hook using [overcommit](https://github.com/brigade/overcommit). See earlier post [Xcode project git hooks](/2015/12/08/xcode-project-git-hooks.html) on how to use overcoomit with Xcode.

{% gist paulz/c1aee2b520a6ffaafcc0 %}
