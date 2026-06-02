---
layout: post
title: "Stubbing Rails.cache.fetch in Minitest"
description: "If you've got some code that's calling Rails.cache.fetch and you want to test that it's working properly, you can use the following method helper to stub the cache: def stub_rails_cache(&block)…"
date: 2023-06-24 15:59:19 +0000
image: /assets/posts/testing-rails-caching/feature.jpg
permalink: "/testing-rails-caching/"
original_url: "https://www.orimarash.com/testing-rails-caching/"
---

If you've got some code that's calling `Rails.cache.fetch` and you want to test that it's working properly, you can use the following method helper to stub the cache:

```ruby
def stub_rails_cache(&block)
  @cache = {}

  cache_strategy = ->(key, &block) do
    @cache[key] ||= block.call
  end
    
  Rails.cache.stub :fetch, cache_strategy, &block
end
```

Which you can use in your tests:

```ruby
stub_rails_cache do
  # In here, we're using an in-memory, naiive cache.
end
```

Note that there is another approach, but it doesn't work because of race conditions:

```ruby
test "caches" do
  # Use in-memory cache
  Rails.cache = ActiveSupport::Cache::MemoryStore.new

  # Caching tests here, but if you use this approach in multiple
  # tests then race conditions could make this fail.
ensure
  # Reset cache
  store = Rails.application.config.cache_store
  Rails.cache = ActiveSupport::Cache.lookup_store(store)
end
```
