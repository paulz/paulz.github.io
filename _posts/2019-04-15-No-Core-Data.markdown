---
layout: post
title:  "Do not use CoreData, use Web Cache"
date:   2019-04-15 10:00:00
categories: swift xcode ios coredata web api
---

# Do not use CoreData

Most apps do not need CoreData. Out of over 80 apps I worked last 10 years, only 2 can justify CoreData with a stretch.

### Common misuse case

We have a business app with user data on server. Server has a database with quite elaborate data schema. Mobile developers often came from server side past naturally think that mobile app would have a slice of that database on user device.

"Nobody gets fired for buying from IBM" mentality often translates in iOS projects to "Nobody gets fired for choosing CoreData". But they should! CoreData is not a safe bet especially for intermidiate level developers.

### When to use CoreData?

When you determined that you absolutely need a database inside the mobile app, you might consider CoreData. First see [Do not use databases](#do-not-use-databases)


## Do not use databases

Most apps do not need databases for local storage, SQLLite or Realm or whatever.

### Do not even create models

### Create view models

## Use Web Cache for offline data

### Cache URL requests

### Cache JSON

### Cache Entities

