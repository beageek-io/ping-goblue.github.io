---
layout: post
title: Use multiple controllers in a Sinatra application
category: ruby
---

Writing a Sinatra app in the Classic way is very easy. What you need is to create a file
called `app.rb`:

```ruby
# app.rb

require "sinatra"

get "/" do
  "hello, world"
end

```

And then, create a `Gemfile` in the same directory of `app.rb` and run `bundle install --path vendor/bundle` to install dependencies:

```ruby
# Gemfile

source 'https://rubygems.org'

ruby "2.2.2"

gem "sinatra", "1.4.6"
gem 'thin', "1.7.0"
```

In your terminal, run `ruby app.rb` to start the server. If you run
`curl localhost:4567` in your terminal, you will get `hello, world`.

When your application grows larger, you need add more endpoints and business
logic inside `app.rb`. However, it becomes very hard to manage when it comes
to a certain size.

Today, I am going to share you a way to break down endpoints into different controllers. To do so, we will need to write
the Sinatra app in the Modular way.

Rewrite `app.rb` in the Modular way:

```
# app.rb
require "sinatra/base"


class App < Sinatra::Base
  get "/" do
    "hello, world"
  end
end
```

Create a new file called `config.ru` in the same directory of `app.rb`:

```
# config.ru
require "./app"

run App
```

In you terminal, run `rackup config.ru -p 4567` to start the server and if you
run `curl localhost:4567`, you will get the same result, `hello, world`.

Now, I want all my endpoints related to users in a controller called
`UsersController`. Let's create a new file called `users_controller.rb` in the
same directory of `app.rb`:

```ruby
# users_controller.rb

class UsersController < Sinatra::Base
  get "/users/:id" do
    route = env["sinatra.route"]
    response = {:endpoint => route}.to_json

    content_type :json
    status 200
    body response
  end

  post "/users" do
    route = env["sinatra.route"]
    response = {:endpoint => route}.to_json

    content_type :json
    status 200
    body response
  end

  delete "/users/:id" do
    route = env["sinatra.route"]
    response = {:endpoint => route}.to_json

    content_type :json
    status 200
    body response
  end
end
```

And then update `app.rb` to include `users_controller.rb`:

```ruby
# app.rb

require "sinatra/base"
require "json"

require "./users_controller"

class App < Sinatra::Base
  use UsersController
  get "/" do
    "hello, world"
  end
end

```

Start the app in the terminal: `rackup config.ru -p 4567` and you can hit your users endpoints:

```bash
$ curl -X GET localhost:4567/users/uuid
{"endpoint":"GET /users/:id"}

$ curl -X DELETE localhost:4567/users/uuid
{"endpoint":"DELETE /users/:id"}

$ curl -X POST localhost:4567/users
{"endpoint":"POST /users"}
```

Of course, you can keep adding more controllers, for example, `OrdersController`.

Here is all the code in my [github](https://github.com/pingzh/multiple-controllers-in-sinatra-application)
