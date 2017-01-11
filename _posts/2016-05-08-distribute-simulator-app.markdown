---
layout: post
title:  "Distribute iOS Simulator app within your team"
date:   2016-05-08 10:00:00
categories: xcode source code continuous integration
---
## Overview
This is a way to automate continuous delivery of iOS project on Simulator. This allows
teammates to run latest app code on Simulator.

## Solution

### Launching app on Mac
Using this script file you can launch a iOS Simulator app within the same folder as the script.
{%  gist paulz/7cac11ad9a2229c6920c7c2004e0a2fc %}

Placing this script and app archive on some cloud space we can make it available for anyone 
within the company.

Next, is how to keep app up-to-date. We can use our continuous integration server to keep
 updating the app on the cloud after it passed tests.

### Updating app continuously
Here is our example of using Google Drive to store zip archive of the app. This is part of travis script that 
finds the app, packs into zip and uploads using gdrive. To authorize access we use GDRIVE_REFRESH_TOKEN, and 
to specify zip file on Google Drive to update we use <some unique file id>.

{% highlight bash %}
SIMULATOR_APP=$(xcrun simctl get_app_container booted myapp.bundle.id)
appZip=My.app.zip
ditto -ck --rsrc --sequesterRsrc --keepParent $SIMULATOR_APP $appZip
gdriveFileId=<some unique file id>
gdrive --refresh-token $GDRIVE_REFRESH_TOKEN update $gdriveFileId $appZip
{% endhighlight %}

To install gdrive with Homebrew:
{% highlight bash %}
brew install gdrive
{% endhighlight %}
