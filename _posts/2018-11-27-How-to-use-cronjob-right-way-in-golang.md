---
layout: post
published: true
title: How to use cronjob golang right way.
---

I used cronjob service in many projects for different purposes like update status, sync up data … The simple thing to do that was starting cronjob service with constant time period. The problem is what if your service take longer than? 

Take a look in explanation how [Cronjob](https://github.com/robfig/cron) work in Golang first.

Whenever you create a new job `c.AddFunc("0 30 * * * *", func() { fmt.Println("Every hour on the half hour") })`, cronjob will be added into a channel and scheduled to run with goroutine.

### Problem :

How about the job will take longer as we estimated ? 

What bothering us is cronjob will create another goroutine beside the old one. This situation leads to complicated bugs cause duplication tasks.

### Solution :

No revolution here, all you need is checking the status of jobs that you opened. 

Here we go, make it works.

Create a bool channel and update status of job to that.

Here’s what I tried with the sample code 

```
doneSyncing := make(chan bool) 
c.AddFunc(“@hourly”, func(){
	SyncTransactions(doneSyncing)
})

func SyncTransactions(){
	select {
	default:
		fmt.Println(“Syncing hasn’t done yet”)
		return
	case <-done:
		defer func() { done <- true}()
		// logic here
	}
}
```

Renew your coffee and be ready to dive deep into code.

As I said, create a new bool channel and put it into our cronjob. If you’re into the SyncTransaction function you can see I use select case for checking status of job.

Once the job has done, update channel is true.

Let’s create a simple codes and run.

The heck… Why application doesn’t invoke the cronjob? Make debug for doneSyncing variable and problem is found. You need to trigger doneSyncing with true for for first starting app.


Add this under c.AddFunc 

```

go func() {
	doneSyncing <- true
}()

```
Yay! It works now. Problem solved and you can exhale and drink your coffee now.

Leave your idea/solution/bug in comment to improve this problem.
