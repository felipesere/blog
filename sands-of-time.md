> Most people thnk that time is like a river that flwos swift and sure in one direction.
But I have seen the face of time.
And I can tell you, they are wrong.
Time is an ocean in a storm.
You may wonder who I am or why I say this...

Time is a programmers worst nightmare.
Sort of.
Right there after concurrency and (n+1) queries.
We have many many many standards surrounding time.
We have UTC, unix timestamps, we have libraries in all flavours and shades of quality (pre-Java-8s Data, Time and Calendar are probably amongst the worst).
Yet, time is such an elusive "thing".


For example, you may have service that is replicated 3 times behind a load balancer.
Your client sends a chain of request and at some point an error pops up.
He sends you his log and a kind plea for help.
Since you log everything on your three services, lets see what really happened with those requests.
You pull in all three logs, squash them together sorted by time and you inspect the right time window.
Ah, you spot the issue! They are sending the requests in a bogus order.
No, they can't possibly do that, do they?
Pitfall number one: time on server A is not time on server B is not time on server C.
If two things don't use the exact same clock, you can assume that ``clock drift`` happens.
That ``clock drift`` might be as large as whole second, which in times of hundreds if not thousands of requests per second, is a lot!
Oh but time has more issues!
Take timezones! Timezones are a great invention to cope with the problem that the earth is rather big and the sun is not at its highest in London when it is at its highest in Chicago.

That creates all kinds of challenges! Assume you want to store events in a database.
For simplicity, an ``event`` in this case means some point in time when it starts and some point in time when it ends.
Sounds simple enough! Lets give users a bit of freedom.
Let's say they only need to enter a start time.
We will then have the event run for the rest of the day.
When would that end? Our database stores time in UTC (Coordinated Universal Time).
The naive thing is to simply set it to 00:00 UTC of the day after.
Great! Oh wait.
00:00 UTC is 23:00 BST.
Meaning it should actually go until 01:00 UTC.
So it depends on where the user is entering the data from.
So we can't use a comon "tombstone" value like 00:00 UTC to show that an event "goes until the end of the day".
It also makes our queries harder!
If everything ended 00:00 UTC of the day after, we could have done some clean, generic queries...

Then there all those issues of leap-seconds, daylight-savings and leap-years.
Plus, it's not like programmers can keep up with the changes in time a sovereign nation can make.

If I were to make one recomendation in this blog, it would be to stay away from time.
Try to figure out some other way, some other property to use in your systems.
And if you do have to use time.
Don't be overly clever.
Don't try to use some property of time that can not be explained on a napkin.
Even if you don't get it wrong, next guy might no be such a time-geek.


