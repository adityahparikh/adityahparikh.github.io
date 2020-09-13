---
layout: post
title: Rails dynamic number of arguments to method 
tags: ['Ruby on Rails']
# permalink: /posts/rails-enum-prefix-suffix
---

Sometimes we come in an situation where we may not know the number of arguments passed to the method. We can use single splat operator(*) and two splat operators(**) before the arguments passed as parameters to the method

```ruby
# processed array type arguments
def some_method_with_one_splat(*fruits)
  return fruits
end

#processed hash type arguments
def some_method_with_two_splat(**information)
  return information
end

$ some_method_with_one_splat('apples', 'bananas')
=> ["apples", "bananas"]

$ some_method_with_one_splat(name: 'apples', color: 'red')
=> [{:name=>"apples", :color=>"red"}]

$ some_method_with_two_splat(name: 'apples', color: 'red')
=> {:name=>"apples", :color=>"red"}

# Two splat operator works with only one hash
$ some_method_with_two_splat({name: 'apples', color: 'red'}, {name: 'bananas', color: 'yellow'})
=> ArgumentError: wrong number of arguments (given 1, expected 0)

```
Paramters with double splat operator works with only hashes. It will throw argument error if any other type is passed.

```ruby
$ some_method_with_two_splat('apples', 'bananas')
=> ArgumentError: wrong number of arguments (given 2, expected 0)

$ some_method_with_two_splat('apples')
=> ArgumentError: wrong number of arguments (given 1, expected 0)

```

Parameters with double splat is optional
Let's rewrite the above example and use the usual parameters instead of double splat operator

```ruby
def some_method_usual(information)
  return information
end

$ some_method_usual(name: 'apples', color: 'red')
=> {:name=>"apples", :color=>"red"}

```

What’s the difference? Why should we use a parameter with the double splat operator if we can get the same result without it? The uniqueness of using a parameter with the double splat operator is that it’s optional. In other words, if we do not pass a hash, then there will be no error, and a local variable inside a method will refer to an empty hash ({}).

```ruby
def some_method_with_two_splat(**information)
  return information
end
$ some_method_with_two_splat
=> {}
``

You can even use one splat and two splat arguments in single method which can be used when you don't know how many arguments you want to process. Keep the two splat operator at last in the list of parameters

```ruby

def some_method(arg_1, *arg_2, **arg_3)
  return arg_1, arg_2, arg_3
end

# just one argument passed
$ some_method('hello')
=> ["hello", [], {}]

# more than one arguments passed
$ some_method('hello', 'apple', 'banana')
=> ["hello", ["apple", "banana"], {}]

# hash fields also passed
$ some_method('hello', 'apple', 'banana', first_name: 'aditya', last_name: 'parikh')
=> ["hello", ["apple", "banana"], {:first_name=>"aditya", :last_name=>"parikh"}]

```