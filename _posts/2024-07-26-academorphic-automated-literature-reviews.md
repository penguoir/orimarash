---
layout: post
title: "Academorphic -- automated literature reviews"
description: "Academorphic, a tool for university students working on their dissertation, is designed to simplify the process of writing literature reviews. It's a fork of Morphic (GitHub). Academorphic…"
date: 2024-07-26 11:11:18 +0000
image: /assets/posts/academorphic-automated-literature-reviews/feature.png
permalink: "/academorphic-automated-literature-reviews/"
original_url: "https://www.orimarash.com/academorphic-automated-literature-reviews/"
---

Academorphic, a tool for university students working on their dissertation, is designed to simplify the process of writing literature reviews. It's a fork of Morphic ([GitHub](https://github.com/miurla/morphic)). Academorphic gathers, summarises, and answers questions about academic papers, then generates a PDF literature review for you.

I built it as part of the Bath Hack 2024 with my two teammates: Arnau Ayerbe and Varniethan Ketheeswaran.

## **Features**

**Title-Based Search on ArXiv**: Academorphic starts by asking for the title of your dissertation or research paper. Using this title, it searches the ArXiv database, a repository of pre-print papers, to find relevant literature.

![]({{ "/assets/posts/academorphic-automated-literature-reviews/img-0.png" | relative_url }})

**Summarization and Analysis**: Once the relevant papers are found, Academorphic summarizes the key points of each paper. It then uses Retrieval-Augmented Generation (RAG) to answer specific questions about the literature.

![]({{ "/assets/posts/academorphic-automated-literature-reviews/img-1.png" | relative_url }})

**Interactive Follow-Up Queries**: If the title you provide is too vague or broad, Academorphic prompts you with follow-up questions to narrow down the search.

![]({{ "/assets/posts/academorphic-automated-literature-reviews/img-2.png" | relative_url }})

**PDF Generation**: After compiling and analyzing the relevant literature, Academorphic allows you to generate a PDF of your literature review with a click.

![]({{ "/assets/posts/academorphic-automated-literature-reviews/img-3.png" | relative_url }})

We used a new React library made by Vercel to build the UI. It lets you directly connect streamed LLM responses to React components, so you get snappy responses. Here's a link:

[Introducing AI SDK 3.0 with Generative UI support – Vercel](https://vercel.com/blog/ai-sdk-3-generative-ui)
