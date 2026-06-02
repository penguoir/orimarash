---
layout: post
title: "Just look at their code"
description: "One of my clients was searching to hire another developer. I was responsible for one of the steps in this hiring process: I verified the candidate could write good code. Through this process, I…"
date: 2024-01-15 18:35:14 +0000
image: /assets/posts/just-look-at-their-code/feature.png
permalink: "/just-look-at-their-code/"
original_url: "https://www.orimarash.com/just-look-at-their-code/"
---

One of my clients was searching to hire another developer. I was responsible for one of the steps in this hiring process: I verified the candidate could write good code. Through this process, I developed my own preferred way to test candidates.

I was only responsible for verifying that the candidate could write good code. The client was responsible for everything else, like making sure the candidate wasn't an arse, could speak English, could legally work for the company, etc. Another essential skill I was not testing is whether the developer could independently transform a loose design document into an implementation plan. This also was handled earlier in the hiring pipeline.

At first, I checked whether candidates could write good code by conducting a technical interview. All I did in this interview was ask them technical questions. I'd make sure they know about model-view-controller architecture, make sure they could use Git (ask me why!), and test-driven development. After an interview finished, I'd usually have a low-confidence rating for the candidate. My rating was low confidence because I could only base my confidence on what they _said_.

I eventually asked one candidate, "Can you _show_ me some code you've worked on and talk me through it?" They shared their code, explained its context, and walked me through the constraints they were working under while writing it. We then had a fascinating discussion that clearly revealed the candidates' opinions on what good and bad code looked like.

From then on, I conducted all my technical interviews using the golden question: "Can you show me some of your code?"

I didn't mind if the code they showed me looked ugly. I only cared whether their internal classifier for good/bad code was calibrated.

Some candidates didn't have much to say about their code, so I found a good follow-up question, "If you had time to re-write this function, how would you do it?"

I asked them to show me their code rather than show them some of my own because, as a developer, it isn't necessary to look at code and quickly understand what it's doing. It's okay to take time to get acquainted with the system. I was worried that some developers wouldn't have any side projects, but all of them did. On second thought, I don't think a developer should apply for any role until they've written _some_ code, so this makes sense.

At some point, the client wanted me to spend less time on interviews so I could divert more time to other priorities. Instead of setting up interviews, the client directly asked candidates to share some code they'd written and forwarded it to me. Although this was a faster method for assessing candidates, I lacked some of the context that often influences the code's elegance, which lowered my confidence in my ratings.

One thing this approach doesn't measure is the overall competency of a developer, which includes proficiency in business, product, and soft skills. For example, if the code they're writing is entirely unnecessary (but clean), they'll pass the "show me your code" test, even though they aren't a competent developer. Competent developers don't write unnecessary code. So, this test is great for measuring the developer's coding style rather than their overall software engineering.

I want to mention that, of course, I'm influenced by many biases when using this approach to assess candidates. For example, I will likely give a higher score to a developer who writes code similar to mine. My code is neat, but I'm not perfect, so this assessment might return false positives for programming geniuses whose style differs from mine.

I actually stole this question from [Andrejus K](https://www.linkedin.com/in/andrejusk/), who interviewed me a few years ago using this approach. Other than that one interview, I haven't ever been asked this question. I don't see why it's not used more often in the industry. Please email me if you know!
