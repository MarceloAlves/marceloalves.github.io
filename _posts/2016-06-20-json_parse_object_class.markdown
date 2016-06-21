---
layout: post
title:  "JSON.parse with object_class"
date:   2016-06-20 17:00:00
categories: notes ruby
banner_image: ""
featured: false
comments: true
---

You can pass an `object_class` to `JSON.parse` which will use that class to create an object. Can pass `OpenStruct` to create a simple object or something like an ActiveRecord class.

```ruby
json_data = '{"id": 1,"name": "A green door","price": 12.50,"tags": ["home", "green"]}'

parsed_json = JSON.parse(json_data, object_class: OpenStruct)

p parsed_json.name
# "A green door"
```
