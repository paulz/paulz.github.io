---
layout: post
title:  "Desktop or Laptop for iOS Developer"
date:   2017-01-10 10:00:00
categories: ios xcode objc swift
---
What should you choose iMac or Mac Book Pro for your iOS Development?

For me it's a simple choice of top performance at a price.

Best iMac cost less then MacBookPro+Monitor.

Here is the performance comparison:

~~~
** BUILD SUCCEEDED **


real    1m56.945s
user    8m1.084s
sys	1m37.461s
~~~

an on iMac:

~~~
** BUILD SUCCEEDED **


real	1m20.605s
user	5m46.970s
sys	1m6.669s
~~~

while running `time xcodebuild ... clean build` on average size project.

##This data shows iMac is 40% faster for xcode build


 * iMac: 4GHz Intel i7, 32Gb, 250GB Flash
 * Macbook Pro: 2.2GHz Intel i7, 16Gb, 250GB Flash

Given the same Flash storage iMac can afford 80% faster CPU and 2x memory at roughly the same price (If you want to get best laptop $2,999 vs best iMac $3,149 as of January 2017). 

And as many laptop users would need a display to plugin at their desk, iMac saves on a cost of 5K monitor another $1000.
