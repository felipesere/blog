- Isomorphism
  - Great, so data and functions are represented using the same strucures. 
  - Now I can't tell the difference anymore!
  - There is no telling if this is a dirty hack or not because the only thing the langauge tries to stop you from doing is reassignment

- Short-and-terse trumps readable
  - Proponents of certain languages (Java, Ruby etc...) clearly state that "being able to read code" is better than "the most functionalty you can squeeze into two lines"
  - It may just be the Clojure I have seen so far (SO, clojuredoc, collegeues)
  - I used to be proud that actual language constructs show up way down in some tiny little method with good name.
  - In clojure reduce with an annonymous partial function which iteratoes over something you generated with with 6 nested function calls is actually "cool".
     - All I have learned about small methods and small classes... Does not want to apply to Clojure?

- No place for things
  - There is no instinctive place to put things
  - Yes you have namespaces, so do many other languages, but they also have 'classes' where I can add behaviour and data
  - Maybe I should get my namespaces smaller?
  - There is no "aftertaste" of doing everything in a gynormous file and there is no "pull" or clear seam where you'd move things into a separate class

- I see parens, parens everywhere
  - I really do enjoy syntax. It gives distinctive things a distinctive look.
  - Just having parens, functions and three-and-a-half datatypes is too little for me.
  - paredit fixes this... but you get a chain of 12 parens at the end... and in JS people are ok to bitch-and-whine about it

- Infix notation
  - I dont really feel THAAAT bad about this one.
  - It just leads to weird-to-read code
  - (> (funny-func-1 dataaa?) (apply max (some-other-data)))

- Concurrency is really not that far up on my 'to solve list'
  - I have not been hit by mutable-state too often. Heck, I avoid it. I like immutabilty.
  - I have not written heavily threaded or micro-optimized code where I had to manage threads myself.
  - I HAVE had to deal my colleagues writing writing simple strings and other primitives all over the palce and me having to re-solve their problems!

- Parameter explosion
  - I feel like where in other languages I would have gathered params into an object, in Clojure I passed 14 params... dont worry, NOBODY cares about their types so you'll just spot those errors at runtime. Urgh.

- Reading bottom up... really...


So Clojure is all about abstracting away state (simply not having it) so that your code will be easy to test, easy to reason about and scale massivly.
I really wonder if legibility needed to take such a hit to achieve that.
Obviously CLojure is built on the Lisp heritage that was written waaay before I started to code.
In my head, Clojure (and Lisp and by extension 'pure functional programming') simply don't map well to how I think and how I feel things should be programmed.
Having been introduced to Java as my second language (PERL being the first by selfstudy) probably brainwashed me to value certain things over others.
Plus, during my apprenticeship and books and Meetups, values such as 'dont expose primitives' or name your domain concepts and promote them to meaningful things 
has become really important to me. I try to carry that over into every new langague so my Clojure isnt really clojury and I probably try to bend it towards things that it doesnt want to do
