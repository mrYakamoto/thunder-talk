# Sidekiq gem

*gem for moving  a long running job into a background process.

## handles multiple jobs concurrently using threads

## Resque is a similar background processes gem. The only reason to use Resque instead would be to avoid concurrent multi-threads. Sidekiq requires writing 'thread-safe' code

## uses redis to manage its job queue, so you have to install redis first.
- use homebrew with 'brew install redis'
- start up redis with the given command after install

## add 'sidekiq' to the Gemfile

## create seperate worker class
- create new directory called workers inside of app/
- touch file like worker_name.rb
- create a class inside like
  `'class SidekiqWorker'
  '  include Sidekiq::Worker'
    def perform
    end
  'end'`
- 'sidekiq_options retry: false' in the class will prevent sidekiq from running the process again if an error is raised.
- def perform contains the actual action you want done in the background. Move that action into the perform method
- call the perform method with
  'SidekiqWorker.perform_async'
  - don't pass complex objects in as parameters, pass simple data and find the object in the worker.

  - to be 'thread safe', avoid using class level data(outside methods) that's mutable.

  - the pool size limit in the database config file defaults to 5. You should bump this up to 25 which is the most jobs sidekiq will run at the same time.

## 'bundle exec sidekiq' in the terminal to start sidekiq running


there are cool features documented in the wiki.
## you can schedule time for a job to be performed, like clearing a cache for example
## you can prioritize queues that will make sure certain methods are executed first

## you can include a sinatra browser interface with sidekiq
  -call 'mount Sidekiq::Web, at '/sidekiq' in the routes
  -require 'sidekiq/web' at the top of routes
  -include 'sinatra', require: false in Gemfile
  -include 'slim' in Gemfile
  -you should password protect this page in production.

## Sidekiq uses 'Celluloid' for its multithread concurrency. Check out 'Celluloid'


![sidekiq code example#](http://s21.postimg.org/50szwdsnb/Screen_Shot_2016_01_23_at_8_38_47_PM.png)

