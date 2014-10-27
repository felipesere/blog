# My kind of Clojure

As part of my apprenticeship I have to learn a functional language.
The functional language of choice here is Clojure.
Its a LISP dialect that runs on the JVM.
As my background is very heavy on the Java side, some things in Clojure just wouldn't sit very well.
Here are things did not enjoy in my first tour through Clojure.
<!-- more -->

## I am not a fan of isomorphism
Homoiconic means that data and code are representated using the same structures.
This allows you to extend the language itself and do all kinds of tricks.
Why do I not like it?
I can't tell the difference between data and behaviour.
Function calls are just lists and you can pass around lists to make function calls.
Vim tries really hard to get some kind of syntax highlighting, but its just too hard.
The conciseness of the langauge means that no elements carry special meaning which could be distinguished visually.
Coming from Java, where the distinction is crystal clear, my brain is missing visual clues when editiing Clojure.

## Short and terse
The Clojure code I had seen so far gave me the impression that writing short, terse code was prefered over expressive and long names for individual functions
that were composed of other methods.
In the code I seen so far there were functions, but these had quite a few anonymous function _mapping_ and _reducing_ datastructures and whatnot.
To me that is unreadable.
In my domain, _reduce_, _apply_ and _map_ have no meaning.
The actual conversions and selections that do happen are interesting, and they should be named.
I propose to extract all of these little functions and promote them to real functions.

## Where to place things
In OO languges there is a natural place to put things.
If your class has too many attributes and you can see that some of those are always used together, break them out into their own class and move the behaviour.
From Clojure I did not get that _feeling_.
I did create a couple of namespaces, but they more closely resembled classes in a OO language than packages.
Where do you draw the lines between namespaces?
Functions _map_ data strucutres all over the place.
Collecting functions by what data structures they work on might be a good approach.

## Bottoms-up!
In Clojure functions can only call functions which have already been declared.
This means that your highlevel functions will end up being at the bottomg of the file since they will in turn call the other functions.
If you have two mutually recursive calls, you need to use _forward declaration_ to get around the order propblem.
From past experiences reading WSDL files, I feel like this is punishing the next reader.
Sure, when writing you know what is going on, but opening a file and then starting to scroll down to find the functions that actually matter is a big no-no.

## 3 Rules For Readable Clojure

1. As few language constructors per functions as possible
   _Apply_, _map_ and _reduce_ have no meaning in your domain.
   You should definitly use them, but give them a purpose-revealing name.
   Not having more than one of these per function forces you to name things for what they are

2. Forward-declare everything
   Like in other programming langauges, important things go to the top.
   The important functions, that give meaning to a namespace should wander all the way to the top of your file.
   If you have any degenerate cases of datastructures you use, them place them close to where they are used.
   They give the reader importnat clues on what things look like.

3. Careful anonymity
   Clojure allows you to write clousres (functions that return new functions having a set data bound to them) and inline-functions.
   Though this is quick and easy to write (and closures have a whole set of benefits) you should use anonymity with care.
   It doesnt help in debugging and it makes it a little harder to read.
   The seond you start nesting these constructus, you have a complex function that definitly deserves a proper name!
