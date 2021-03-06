---
layout: post
published: false
title: Design Architecture [Part 2].
---

This article is the next step of [part1](https://thinhlvv.github.io) Design Architecture.

### When do you need to make decision to scale your system?

When you are handed over a source code but too complicated and confused to maintain.

When you check your server heal and see a lot of peaks and CPU up and down frequently.

When you see the 5xx requests is more than your daily meal.

![why](https://seanwes.com/wp-content/uploads/2016/02/242-sp-find-your-why.png)

Or you just want to provide bunch of features to your customers and make your system available.

If you find yourself in those, make a proposal to your boss or your team to make the scalability happens.

### Understand your system first 

Understanding your system is most important part before you think to somethings like distributed database, load-balancing …

If your system is kind of read-system, focus on your cache server and cache behaviours. If it’s a realtime system with socket, 
do build Pub/Sub system to handle that huge dataset. 

Investigate the APIs those are most frequently and append them to your consideration.
 
### General view with architecture and trade-offs

Once we finish those, let’s see how we can approach the very basic primer to find out your most affordable to you.

![tradeoffs](https://cdn-images-1.medium.com/max/1600/1*D1euW219UdxafVeOjqvzNA.png)

- Vertical scaling

	Just add more weapon (RAM, CPU) to your application server. 
	
- Horizontal scaling

	Add more server to handle request, of course you need ha-proxy to direct your requests.
	
- Caching

	Need to observe request behaviours to decide caching strategy.
	
- Load balancing

	For high traffic system to direct the requests between servers.
	
- Database replication

	Improve availability for system.
	
	Improves the performance for retrieval of global queries as the result can be obtained locally from any of the local site
	
- Database partitioning 

	Sharding your database is most popular to partition your database to improve your queries.
	
	Need to divide your dataset strategy into partitions.
