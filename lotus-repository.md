# Using Lotus - Model 

As part of the apprenticeship I began building a toy-sized Rails application. 
The purpose is to get familiar with concepts and techniques of Rails.
Although Rails already prings a pre-packaged interface into the database, ActiveRecord, I wanted to use a different framework form my data access layer: ![Lotus][http://lotusrb.org]


<!--more-->

# Lotus itself
Lotus was recommended as an alternative to Rails to me by my fellow apprentice ![Uku][www.ukutath.co]
It's a rather young project and still has some ways to go. 
But it brings forward a couple of nice properties which might make it really attractive down the road.
The first thing you'll notice just by going to the projects' website is that Lotus is split across six modules.
Each of the modules stands on its, allowing you to swap out single peaces or use elements of Lotus in conjunction with other frameworks.

## Foundations of Lotus model
Let's look at the Model, Lotus answer to persisting "things" "somewhere".
...add a sentence or two here..

### Entity
A ``Lotus::Entity`` is a regular Ruby object which gets a very few methods by using Lotus.
Here is an example of my ``Book`` entity in my ``Slate`` book application:


    require 'lotus/entity'
    
    module Slate
      class Book
        include Lotus::Entity
    
        self.attributes = :id, :title, :author, :reading_state, :started_reading_on
    
        def days_reading
          if @started_reading_on
            Date.today.mjd - @started_reading_on.mjd
          else
            0
          end
        end
      end
    end

Including ``Lotus::Entity`` gives you access to the ``attributes`` method. 
All it does is provide you accessors for the designated attributes.
Furthermore an initializer is created for you that takes a hash as an argument and assigns those attribues.
The only notable attribute to mention here is that your entity must have an ``id`` attribute.
Other than that there is very little to say about ``Lotus::Entity``. 
It is nice, unobstrusive and gives you barely enough functionality to consider it.
Mind you, you could do all the _magic_ that ``Lotus::Entity`` does yourself. 
The framework neither cares about it nor does it rely on any inheritance chain for it to work.

### Repository
The repository is the gateway between whatever persistance system you use and your entities.
An important concept to grasp here is that _persistance_ does not automatically mean SQL. 
The repository only needs to rely on the ``query`` abstraction and the underlying adapter. 
If the Lotus guys did their job right, you should be able to switch adapters with _zero_ effect on your repositories or entities.
I have tried switching between the ``SqlAdapter`` and the ``Memory`` adapter and it worked like a charm!
Below you can find my tiny ``BookRepository``:


    require 'lotus/repository'
    
    module Slate
      class BookRepository
        include Lotus::Repository
    
        def self.create_book(attrs)
          book = Slate::Book.new(attrs)
          create(book)
        end
    
        def self.find_by_title(title)
          query.where(title: title).first
        end
      end
    end

Including ``Lotus::Repository`` does quite a bit more than ``Lotus::Entity``. 
From a users perspective, it adds basic methods to ``persist`` , ``update`` and ``delete`` entities as well as some framework related methods. 
The most interesting one of those framework methods is ``query``.
It allows you to construct complex queries against your dataset.
A very important feature here is that ``query`` is a *private* method.
This means that it can not be accessed from outside of the repository.
You wonder how or why that is a feature worth mentioning?
It enforces a clean separation. It is not possible to construct queries as a _client_ of the repository.
All queries that are to be executed _must_ be defined by the repository and thus be _named_.
In an application that makes heavy use of Rails _ActiveRecord_ this is not the case. There, any controller or other entity can create custom queries which tightly bind those objects to the underlying structure of your persisted data!

### Query
``query`` is the Lotus abstraction for building queries of any kind.
It has a nice chainable syntax to construct complex queries using ``where``, ``select``, ``or``, ``sort`` and ``limit`` among a few others.
An interesting feature is that the resulting query is just another object. 
Only when you call ``first`` or ``all`` on that query object is the actualy querz realised and performed against the database.
What benefit is that?
You can DRY up your queries by creating _intermediate_ queries and name them:

   self.authors()
     query_authors.all()
   end

   self.active_authors()
     query_authors.where(active: true).all()
   end

   private
   def self.query_authors()
     ...
   end

The query in ``query_authors`` should not be materialised untill either ``authors()`` or ``active_authors()`` is called.
Only those two methods call ``all``.
I had not seen this clean separation before, but being a big fan of the builder pattern, I really like it.

As a rule I would never let those ``queries`` leak out. 
In my eyes it is the repositories responsibility to materialize all queries and turn them into usable buisness objects.
Allowing those queries to cross the boundary to the business logic would undermine the purpose of the repository.

# Adapter

# Adding Lotus to my project
   *

## Current issues

# Outlook
