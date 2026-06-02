---
layout: post
title: "Turbo Lazy-Loading Scroll — How 37Signals do it"
description: "Inside Campfire, I prompted cursor \"How do we do infinite loading in the account users display?\" Here's the result: The infinite loading for account users is implemented using Turbo Streams and…"
date: 2025-10-03 09:23:22 +0000
image: /assets/posts/turbo-lazy-loading-scroll-how-37signals-do-it/feature.jpg
permalink: "/turbo-lazy-loading-scroll-how-37signals-do-it/"
original_url: "https://www.orimarash.com/turbo-lazy-loading-scroll-how-37signals-do-it/"
---

Inside Campfire, I prompted cursor "How do we do infinite loading in the account users display?" Here's the result:

The infinite loading for account users is implemented using Turbo Streams and lazy-loading Turbo Frames with the `geared_pagination` gem. Here's the complete flow:

### 1\. **Controller Setup** (`app/controllers/accounts/users_controller.rb`)

```ruby
def index
  set_page_and_extract_portion_from User.active.ordered, per_page: 500
end
```

The `set_page_and_extract_portion_from` method comes from the `geared_pagination` gem (version 1.2.0) and creates a `@page` object with:

-   `@page.records` - the current batch of users
-   `@page.last?` - whether this is the last page
-   `@page.next_param` - the parameter for the next page

### 2\. **Initial Display** (`app/views/accounts/edit.html.erb`)

```erb
<turbo-frame id="account_users">
  <%= render partial: "accounts/users/user", collection: @page.records, as: :user %>
  <%= render "accounts/users/next_page_container", page: @page.next_param unless @page.last? %>
</turbo-frame>
```

This renders:

-   The first batch of users (up to 500 per page)
-   A "next page container" if there are more users to load

### 3\. **Lazy Loading Container** (`app/views/accounts/users/_next_page_container.html.erb`)

```erb
<%= turbo_frame_tag :next_page_container, loading: :lazy,
      src: account_users_path(page: page, format: :turbo_stream), class: "flex center" do %>
  <div class="spinner center"></div>
<% end %>
```

This creates a **lazy-loading Turbo Frame** that:

-   Shows a spinner initially
-   Automatically loads when it comes into the viewport (`loading: :lazy`)
-   Makes a request to `account_users_path` with the next page parameter
-   Requests the response in `turbo_stream` format

### 4\. **Turbo Stream Response** (`app/views/accounts/users/index.turbo_stream.erb`)

```erb
<%= turbo_stream.replace :next_page_container, partial: "accounts/users/user", collection: @page.records, as: :user %>

<% unless @page.last? %>
  <%= turbo_stream.append :account_users, partial: "accounts/users/next_page_container", locals: { page: @page.next_param } %>
<% end %>
```

When the lazy frame loads, this:

1.  **Replaces** the spinner with the new batch of users
2.  **Appends** a new "next page container" to the bottom (if there are more pages)

### 5\. **How the Infinite Loading Works**

1.  **Initial Load**: First 500 users are displayed with a lazy-loading frame at the bottom
2.  **Scroll Trigger**: When user scrolls near the bottom, the lazy frame enters the viewport
3.  **Automatic Request**: Turbo automatically makes a request for the next page
4.  **Stream Update**: The response replaces the spinner with new users and adds another lazy frame
5.  **Repeat**: Process continues until all users are loaded (`@page.last?` is true)

### Key Benefits of This Approach:

-   **No \[Extra\] JavaScript Required**: Uses native browser intersection observer through Turbo's lazy loading \[This isn't quite right. Turbo streams uses Javascript. But there definitely is the benefit that you don't have to write any _custom_ javascript\]
-   **Seamless UX**: Users see a spinner briefly, then new content appears automatically
-   **Performance**: Large page size (500 users) reduces the number of requests
-   **Progressive Enhancement**: Works without JavaScript (would fall back to regular pagination)
-   **Accessibility**: Maintains proper focus management and screen reader compatibility

This implementation is elegant because it leverages Turbo's built-in lazy loading capabilities rather than requiring custom JavaScript for scroll detection and AJAX requests.
