# Rails Tips and Tricks

This repo will be used as a log of tips and tricks I beleive are worth knowing for both beginners to Ruby on Rails. Especially as a developer that has switched from another language and is not used to having alot of the Rails `magic`. ✨

## General

Was looking at what attributes are available for ActiveRecord migrations and found this [https://api.rubyonrails.org/classes/ActiveRecord/Migration.html#class-ActiveRecord::Migration-label-Available+transformations](https://api.rubyonrails.org/classes/ActiveRecord/Migration.html#class-ActiveRecord::Migration-label-Available+transformations)

`# frozen_string_literal: true` is the magic comment for opting in string literal immutability
Or for singular use `.freeze`

If you use `--no-helper --no-assets --no-template-engine` with rails generate for a controller or model it will literally just give you the controller and relevant tests.
Rather than adding scss, helpers, and all the views for routes

Rails has this awesome thing where you can benchmark your queries. Their helper for it is `Benchmark.measure { Thing.all.count }`

    size_measure = Benchmark.measure { Permit.all.size }
        size = Permit.all.size
        puts "SIZE: #{size} : #{size_measure}"
    
        count_measure = Benchmark.measure { Permit.all.count }
        count = Permit.all.count
        puts "COUNT: #{count} : #{count_measure}"
    
        length_measure = Benchmark.measure { Permit.all.length }
        length = Permit.all.length
        puts "LENGTH: #{length} : #{length_measure}"

Looks like you can do some pretty cool stuff with it [https://ruby-doc.org/stdlib-2.6.5/libdoc/benchmark/rdoc/Benchmark.html#method-c-realtime](https://ruby-doc.org/stdlib-2.6.5/libdoc/benchmark/rdoc/Benchmark.html#method-c-realtime)

To remove pretended fields from errors use `base` instead, admittedly this tripped me up as one of the first tutorials I 
followed added the key prefix which would then result in a error such as `Not Found The record was not found`.

Rails.ajax if using UJS without jquery (ie Rails 6.0+ ships without jquery by default)

Routes can be found in addition to the console command at /rails/info/routes

Using `form.number_field` from FormHelpers will force an integer constraint on.
To resolve this you just need to pass a float into the step similar to this `<%= form.number_field :price, value: @price, step: 0.1 %>`

To people that are used to writing the regex to validate numbers Rails has this built in `numericality: { only_integer: true }`
will use the following regex `/\\A[+-]?\\d+\\z/`

A useful method for both shortening code and useful for associations that may return Null such as optionals.
https://api.rubyonrails.org/classes/NilClass.html#method-i-try

When generating data feeds a useful method to improve performance is https://api.rubyonrails.org/v5.1/classes/ActionController/ConditionalGet.html#method-i-stale-3F
this sets either the `etag` or the `last_modified` on the response and checks it against the client. If it is not stale it will
return `304 Not Modified`

Law of Demeter (Principle of Least Knowledge). In Rails, this could be summed up as "use only one dot".  @invoice.customer.name for example breaks the Law of Demeter

Found a neat little trick to simplify code called the safe navigation operator see below

`user && user.authenticate(params[:session][:password]) into user&.authenticate(params[:session][:password])`

## Turbolinks

If you have an issue where Javascript is not loaded when navigated to via a link but works when you refresh the page. It is an issue with `turbolinks`
You can fix this by adding `document.addEventListener("turbolinks:load", function()`

[https://www.google.com/amp/s/davepaola.com/2018/09/30/stripe-elements-and-turbolinks-5/amp/](https://www.google.com/amp/s/davepaola.com/2018/09/30/stripe-elements-and-turbolinks-5/amp/)

## Debugging 

Rails comes with an awesome tool called byebug where it stops the execution and allows you to check what
variables are in scope, what the params are. If you dealing with unfamiliar code such as a new codebase or gem you use `n`
to step through the the code see what is being executed. 

This is very useful for gems such as Devise and it's extensions where files are not explorable within a codebase unless generating them. :)

You can put a byebug in a template (seems very bizzare).

Often using the Rails Console can be a great way of quickly verifying if a Record exists or if certain syntax is valid if you are unfamiliar
with how something works. 

Two very useful commands I noticed when using byebug are:

`var instance -- Shows instance variables of self or a specific object.`

`var local  -- Shows local variables in current scope.`

## Rails Console

If you want to test a snippet of code out or try out new methods without affecting anything you can run `rails c --sandbox`. 
Upon exiting the console it will rollback any changes that have been made. This can also be a fantastic tool for debugging.

Prepending a command in the Rails console with `pp` pretty prints it.

To look at code to test ratio you can run `rake stats`

You can use the Rails `view helpers` inside the rails console by calling `helper.some_method(arg)`

## Gems 

[https://github.com/presidentbeef/brakeman/blob/master/README.md](https://github.com/presidentbeef/brakeman/blob/master/README.md)

A fantastic gem for creating cron jobs to run ActiveJob tasks with Ruby syntax ie `every 1.day do`[https://github.com/javan/whenever](https://github.com/javan/whenever) (Credit to Craig for showing me this one)

Adding `echo 'gem: --no-document' >> ~/.gemrc`
Stops the gems documentation from being installed which makes a difference to `bundle install` times

## Books 

- Rails Antipatterns: Best Practise Ruby on Rails Refactoring
- Learn Web Development with Rails by Michael Hartl (Sixth Edition)

## Podcasts
Podcast from Chris Oliver from GoRails - Thoughts on best practices, design patterns and good/bad code.
[https://open.spotify.com/episode/34HFMewDTXQftmSvg4fIlP?si=6PKr4gJkSbWG3dZ4aO720w](https://open.spotify.com/episode/34HFMewDTXQftmSvg4fIlP?si=6PKr4gJkSbWG3dZ4aO720w)

https://open.spotify.com/episode/5nbM4bOEInUxEEt09unuy6?si=38lGK2CcSs2abaXYTsvB0A

https://devchat.tv/ruby-rogues/rr-426-dockerized-development-environments-with-julian-fahrer/

## Testing 

Also if you use mysqli, rails 6 adds different mysqli files (as it seperates them when running in parallel) to exclude in your gitignore 
as they are not currently added by default. 

One thing I have found especially useful is setting up a more advanced testing environment using Guard and Minitest Reporters.


