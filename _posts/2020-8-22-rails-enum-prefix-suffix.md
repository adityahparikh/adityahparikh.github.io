---
layout: post
title: Rails enums with same values 
tags: ['Ruby on Rails', 'enum', 'prefix', 'suffix']
permalink: /posts/rails-enum-prefix-suffix
---

Rails 5 introduced a way to define multiple Enums with same values using: `_prefix` and `_suffix`.

In our ActiveRecord model we can define and use enums like this:

```ruby
class Organization < ActiveRecord::Base
  enum current_plan: [:plan_one, :plan_two, :plan_three]
end

organization.plan_one!
organization.plan_one? # => true
organization.plan_two? # => false
organization.current_plan # => 'plan_one'
```

You can use the `_prefix` or `_suffix` options when you need to define multiple enums with same values. If the passed value is true, the methods are prefixed/suffixed with the name of the enum. It is also possible to supply a custom value:

```ruby
class Organization < ActiveRecord::Base
  enum current_plan: [:plan_one, :plan_two, :plan_three], _suffix: true
  enum upcoming_plan: [:plan_zero, :plan_one, :plan_two, :plan_three], _prefix: :upcoming
end

organization.plan_one_current_plan!
organization.plan_one_current_plan? # => true

organization.upcoming_plan_two!
organization.upcoming_plan_two? # => true
```