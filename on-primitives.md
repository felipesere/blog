# On primitives and complex types

We have been having some great debates at work in the last couple of weeks.
One I was particularly interestd in was on the nature of primitve types and whether these were _good_ or _bad_.

# Defining primitive types
What is a primitve type?
Depending on who you ask, what programming language they use and what programming paradigme they adhere to, you might get differnet answers.

From a programming language perspective, anything that does not have methods or _behaviour_ might be called a primitive type.
Take the integer 42 in laugages like Java or C#.
The integer 42 does not have any methods you can call on it.
It only represents the number or _42-ness_ of something.
The integer is used as part of a bigger concept and then it becomes a meaning, like the monetary value of a transaction of the number of tomato soup cans in your kitchen cupboard.
The same applies to floats, doubles, chars etc.

Another possible explanetion of primitive types is that they don't have any inherent _business meaning_ to them. 
They are not specific to your domain or business logic, although they are used to build up such concepts.
This definition includes the above, but expands it Strings (which _do_ have methods) and collection datastrucutres such has lists, maps and trees.
These data structures are used all over your programm, yet they don't provide any business value.
They are not a named-entity in your application.
You can spot these by looking at their declaration in the code:

    List<Transaction> ledger = ...

    Ledger ledger = ... 

The first line shows the primitive type ``List`` used to represent a collection of transactions. 
The domain specific name of that list is hidden in the variable name: it's a ledger.
The second line makes this clear.
A ledger, in a banking application, is what stores a bunch of transactions, and thus we have _named_ that concept more precisely.

# Are primitive types good or bad?

