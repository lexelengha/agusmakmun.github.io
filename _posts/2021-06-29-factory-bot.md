---
layout: post
title:  "Work with factory bot in Ruby"
date:   2021-06-29 18:00:00 +0200
categories: [ruby, factory-bot]
---

In test-driven development, data is one of the requirements for a successful and thorough test. In order to be able to `test` all use cases of a given method, object or feature, you need to be able to define multiple sets of data required for the test.

This is where the data factory pattern steps into test-driven development. `Data Factory` (or factory in short) is a blueprint that allows us to create an object, or a collection of objects, with predefined sets of values.

Let’s take a look at a common example — we have an Article factory and we need to have multiple sets of predefined values that represent its state or use cases. We might have the following variations:

* Unpublished article
* Published article
* Article scheduled to be published in the future
* Article published in the past

These are examples of predefined sets of values that need to be defined for an Article factory.

Let’s take another look at the example of an Article model introduced above:

```ruby
# app/model/article.rb
class Article < ApplicationRecord
  enum status: [:unpublished, :published]
end
```

You can create the following factory:

```sql
# spec/factories/articles.rb
FactoryBot.define do
  factory :article do
    trait :published do
      status :published
    end

    trait :unpublished do
      status :unpublished
    end

    trait :in_the_future do
      published_at { 2.days.from_now }
    end

    trait :in_the_past do
      published_at { 2.days.ago }
    end
  end
end
```

The above factory assumes the Article has the following attributes:

* `status` with value `:published` and `:unpublished`. For ActiveRecord enums, this field must be an `Integer`.
* `published_at` with type `DateTime`.

To use the factory, you can use any of the following statements inside your spec:

```ruby
# build creates an Article object without saving
build :article, :unpublished

# build_stubbed creates an Article object and acts as an already saved Article
build_stubbed :article, :published

# create creates an Article object and saves it to the database
create :article, :published, :in_the_future
create :article, :published, :in_the_past

# create_list creates a collection of objects for a given factory
# you can also use build_list and build_stubbed_list
create_list :article, 2
```

There are several best practices for using data factories that will improve performance and ensure test consistency if applied properly. The patterns below are ordered based on their importance:

* Factory linting
* Just enough data
* `Build` and `build_stubbed` over `create`
* Explicit data testing
* Fixed time-based testing