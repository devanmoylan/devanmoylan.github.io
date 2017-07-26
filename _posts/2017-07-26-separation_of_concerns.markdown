---
layout: post
title:  "Separation of Concerns"
date:   2017-07-26 16:34:26 +0000
---


### Structuring your app so that each component prefers a single task. 


A good analogy for real life practicality of this concept can be seen at a restaurant. You could set up your restaurant to have a server that seats a customer, takes an order, cooks the food, delivers it to the table and cleans the table when finished. This may work with a small restaurant with only a few customers, but what happens when the restaurant is larger and popular? At this point, it's more effecient for the restaurant to break down the responsibilities. You have a server (view), a cook (model) and a food runner (controller) to help the process run more smoothly.  

The model handles all the business or domain logic for an object, which are basically the rules that govern the app. The view handles all the presentation logic which does basically what it sounds like, uses logic to present the data to the client. The controller acts a communicator between the two. Speaking about separation of concerns, sometimes your views will have duplicate code across multiple files. This is where helpers come in. More on that later. 

