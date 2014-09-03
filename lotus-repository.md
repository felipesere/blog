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
The repository is the gateway between whatever persistance system you use and your business entities and domain logic.
An important concept to grasp here is taht _persistance_ does not automatically mean SQL. 
Repositories in your application should be tied to a single business entity and provide different methods to retrieve, store, delete and update entities.


### Query
### Adapter

# Adding Lotus to my project

## Boilerplate And Configuration
## Actual code

## Current issues

# Outlook
