---
layout: post
title: "ActiveCortex"
description: "ActiveCortex is a Ruby gem that makes it faster to integrate OpenAI with Ruby on Rails applications. Ruby on Rails is a framework for building web apps. It's been around for 20 years and has big…"
date: 2024-02-16 14:47:36 +0000
image: /assets/posts/activecortex/feature.webp
permalink: "/activecortex/"
original_url: "https://www.orimarash.com/activecortex/"
---

**ActiveCortex is a Ruby gem that makes it faster to integrate OpenAI with Ruby on Rails applications.**

Ruby on Rails is a framework for building web apps. It's been around for 20 years and has big names like Shopify and Github using it. It's also my favourite way to build products.

One of my clients that uses Rails also wanted to integrate with OpenAI's ChatGPT. They make many calls to ChatGPT across the application for various use cases. Typically, standard programming techniques mean I don't have to re-write the integration each time; I can reuse the same code for each integration.

But, specifically for ChatGPT, I noticed that I couldn't really make this work. The reused code was either too specific or to general. When it was too specific, I couldn't reuse the code across the integrations. When it was too general, the resulting code was ugly. I think this happened because the client has varying use cases for ChatGPT, so it's hard to generalise appropriately.

I noticed that some of my other projects--such as my dissertation--would also benefit from using this generalised code. I talked with the client about extracting my work and publishing it as an open-source project. That way, I could spend more time coding without overcharging them; they agreed. The result is the ActiveCortex package.

From anecdotal experience, using ActiveCortex cuts down on about 150 lines of code. But I think the best benefit is how neat the new code is!

```
class Document
  ai_generated :summary
end
```

That's all the code you need to automatically generate a summary for a document using OpenAI.

I'm hoping to add more features to this package, such as:

-   Function calls
-   Generating images
-   Using images in a prompt

Check out the code and more examples here: [https://github.com/penguoir/active\_cortex](https://github.com/penguoir/active_cortex)
