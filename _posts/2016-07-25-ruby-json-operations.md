---
layout: post
title: Ruby JSON file operations
category: ruby
---

## Ruby JSON file operations


#### 1. Parse json and symbolize keys

```
require "json"
def parse_json_with_symbolized_keys(filename)
  JSON.parse(File.read(filename), :symbolize_names => true)
end
```

E.g.

```
$ cat hi.json
{"hello":"world","ruby":["sinatra","rails"]}
```

```
[1] pry(main)> parse_json_with_symbolized_keys("hi.json")
=> {:hello=>"world", :ruby=>["sinatra", "rails"]}
```

#### 2. Write a hash to a json file

```
require "json"

def write_hash_to_json_file(hash, filename)
   File.open(filename,"w") do |f|
     f.write(hash.to_json)
   end
end
```

E.g.

```
[9] pry(main)> hi = {:hello=>"world", :ruby=>["sinatra", "rails"]}
=> {:hello=>"world", :ruby=>["sinatra", "rails"]}
[10] pry(main)> write_hash_to_json_file(hi, "hello.json")
=> 44
```

```
$ cat hi.json
{"hello":"world","ruby":["sinatra","rails"]}
```

#### 3. Now, I want a human readable json output file

```
require "json"

def write_hash_to_pretty_json_file(hash, filename)
   File.open(filename,"w") do |f|
     f.write(JSON.pretty_generate(hash))
   end
end
```

E.g.

```
[19] pry(main)> hi = {:hello=>"world", :ruby=>["sinatra", "rails"]}
=> {:hello=>"world", :ruby=>["sinatra", "rails"]}
[20] pry(main)> write_hash_to_pretty_json_file(hi, "pretty_hello.json")
=> 66
```

let us take a look at the output file:

```
$ cat pretty_hello.json
{
  "hello": "world",
  "ruby": [
    "sinatra",
    "rails"
  ]
}
```
