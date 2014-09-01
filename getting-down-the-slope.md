I really got into snowboarding last march when I went to Davos with some friends from Uni.
Though I had snowboarded before,  this time around I actually decided to take a beginners course to improve my technique and gain confidence.
On the first day on the beginner slope, the teacher said something that stuck in my mind I just remembered last week:

> You are going to head down steep slopes with snow and ice on a little board.
You will have speeds of around 30 km/h and you will actually use just a fraction of the board on the snow.
If you go straight, you speed up and hurt yourself.
If you don't do proper turns, you fall and hurt yourself.
If you don't pay attention, your board will catch an edge and you will fall.
So, whatever you do, do it consciously and purposefully!

What does that have to do with software? Well, getting safely down a slope is like completing a new feature.
It can be a lot of fun, but it can also be frustrating and painful at times.
Lets look at what we can do to get safely and quickly to the bottom of our feature/slope.
We might just have some fun on the way!

<!--more-->

#3 At the top of the course..
When snowboarding, unless you really know the ski area, you'll stop at the top of the course to look around.
Where are the ice patches? How are other people (_possibly more experiences_) riding the course? 
You will start with a rough idea which route to take to get to the bottom.

The same applies to software.  
Once you want to begin, take a look around.
Where will your new feature be used? Does it compare to something similar in the system? Are there tests in place? Can you recognize a style you may want to follow?
When modifying existing behavior, look for current corner cases.
Will you have to extend those? Are tests in place?
Part of the planning should also be what you want to learn personally?  
If this is a entirely new code-base, just snooping around might an achievable goal.
Maybe you want to improve your pairing? Or your TDD? All these things will influence what "route" you take through your feature.

#3 On your way down
Once you decide on a route and "leap" over the edge, things can get pretty hectic pretty fast on a snowboard.
As you turn left and right, avoid ice plates and other obstacles, you may forget your route or miss that one place where the course split and you wanted to go left rather than right.
At best you'll be able to backtrack, at worst you are now in an unfamiliar slope which might be steeper than what you think you can manage.
The way to avoid this is to define 3-4 clearly visible points on the route and actually stop and regroup there.

When coding this means you might waste time pursuing a pattern, feature, cleanup or test that does not get you closer to your goal.
Obviously cleaning up is great and your entire team is thankful if you untie those six objects that nobody else understood.
But you set out to do something entirely different.
A very low-tech to combat these "positive distractions" to keep a todo list.
Good old fashioned pen and paper will do just fine.
Take down everything you think you should address at the end or some time later.
The great thing about software is that, like in snowboarding, you can have more than one go at feature/slope.
Rather than trying to get everything perfect on the first go.
Take notes where you think you might need more testing or cover a specific edge case.
Then come back once you are done and drill down on that pesky edge case.
The good thing: you already have the feature in the bag, so there is less pressure.
Plus, you know the code better than before.
In snowboarding this would equate to head down a known course to specifically practice your turns, or breaking or some other technique.

#3 Wearing the right equipment
On the slopes, most people wear cloths of bright and/or contrasting colors.
Although this has become a fashion statement, the underlying purpose is to see and be seen by others, for safety reasons.
Then there are helmets.
I am pretty sure helmets are not mandatory, but every beginner snowboard/ski course clearly emphasizes the dangers of not wearing a helmet.
And lastly there are sun glasses.
Not a fashion statement but a tool.
Orange for increased contrast, black/brown for bright sunlight, blue for dim weather.
Having the right equipment does not make it easy, but definitely easier.

In software there is large set of tools you can choose from.
Some are closely tied to your IDE and language.
Others are more generic and may require some tweaking.
One I have come to like is ``git grep``.
One I have come to like is ``git grep``.
All it does it look for strings or regexes in files.
You can tweak it to show you line numbers, give you a bit of context with ``-A <n>`` and ``-B <n>`` and do nice highlighting.
The point of having a strong search tool is to give you some orientation. 
If you are refactoring a method, you can find all occurrences to spot places that need further fixing. 
If you want re-use a class in a new context, you can use a good search to find similar places to understand how the class is to be used.

#3 Never go alone
When snowboarding, especially when you are a beginner or on a new course, you will not head out on a course on your own.
Buddying-up addresses some obvious security concerns, but there is a more valuable lesson in there as well.
Snowboarding is tedious and tiring if not done well.
Having someone around that has experience or knows the course give you an enormous boost in confidence.
That person acts like a mentor, helping you refine your technique and taking care that both of you stay on the desired course without you having to worry about it.

Pairing in software projects is the exact equivalent.
There are so many reasons to do it.
Most of them apply to the code that is being written.
It improves design quality, reduces defects and spreads knowledge.
Those are all great, but in this example ``pairing`` is all about you.
It gives you confidence.
A pair helps you when you get stuck on a bit of tricky code.
A pair gives you a pat on the back when you reach a goal.
These ``small details`` are the ones that really matter.
A good pair will encourage you and teach you. 


#3 Conclusion
Next time you start a new features.
Stop and think for a couple of minutes.
Try to to prepare yourself and workplace for the task at hand.
Get a pair, talk the task through with him.
Grab a piece of paper and just write "Todo" at the top. 
In a single sentence, write down the feature you want to add. 
And then get going! Use the tools at your disposal.
Stop and think when you think you have reached a nice milestone.
Add todos as you move along.
Decide do fix a something small and cross if of your list. 
Before you know it, you will be crossing that initial feature of your Todo list too!
