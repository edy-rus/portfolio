---
title: "Behind the Build - Automated Blog Generator"
date: 2025-10-12T00:00:00Z
author: Edy Rustad
draft: false
tags:
  - Project
  - Automation
  - Development
  - Low-Code
  - LLM
  - n8n
  - PowerShell
  - OpenAI
  - WordPress
categories:
  - Portfolio
  - Technology
summary: "A detailed breakdown of how I automated InfinityIT’s blog publishing workflow using n8n, OpenAI, and WordPress. Improving consistency, reducing cost, and maintaining brand quality at scale."
thumbnail: "images/cover.png" # Replace with the actual image path
---


# Behind the Build: Automated Blog Generator

## The Beginning

When I started at InfinityIT, there was no regular blog content. The director wanted to improve our SEO footprint while also giving current and potential clients something useful to read. I began writing weekly posts, usually around 500 words, based on either news I came across or topics suggested by the director.

The process was entirely manual. I would write the content, clean it up using AI tools, then move on to WordPress where I had to do all the SEO formatting, tagging, and Yoast tweaking. On top of that, I needed to generate a featured image using Leonardo.ai and make sure the layout worked well on the site.

Each post took about two hours of focused work. Since my job involves everything from technical support to programming, automation, and client-facing roles, it became harder to justify that time every week. Some weeks the blog simply did not happen.

---
## The Problem

The issue was not the quality. The posts were decent, but the effort-to-output ratio was too high. From a business point of view, it was costing us an average of $90 per blog post.

There was also inconsistency. Without a regular schedule, the SEO value was reduced. The manual process of using early AI tools was clunky and slow. I would often have to prompt, wait, review, and then re-prompt again. Since I am not an editor by trade, the review process felt like a constant uphill battle.

At that point, it became obvious that the whole thing needed to be automated.

---
## Building the System

I built a full workflow using **n8n** and **OpenAI's API**, directly integrated with our **WordPress site**.

The system runs twice a week as part of a broader daily automation pipeline. On Tuesdays and Thursdays, it triggers the blog workflow. It selects a topic, generates a post using ChatGPT, and formats it to match our structure and tone.

To maintain brand quality without resorting to high-cost LLMs, I built a two-step pipeline using **GPT-4o Mini** for content generation and **DALL·E 2** for image creation. This kept the API usage cost extremely low.

Instead of relying on a single large prompt sent to the latest high-parameter models, the system generates a blog post using GPT-4o Mini and then sends it to a second validation step. A separate AI agent checks the output against a set of brand and tone guidelines, and assigns it a score. If the score is too low, the content is rejected and regenerated with the included critique. This allowed me to use smaller models without compromising output quality.

The featured image is generated using **DALL·E 2**, with a combination of a fixed style prompt and content-aware context. It avoids text so it will not conflict with the semi-transparent overlay text used on our website. Though the person who approves and publishes posts sometimes swaps out the image, they occasionally ignore the no-text rule, which creates the very conflict I designed the AI to avoid.

Once the post and image are ready, they are sent as a **draft** to WordPress for review. Notifications are sent to our Microsoft Teams channel with status updates or any error messages.

---
## Challenges

Although n8n is a low-code platform, not everything was drag and drop. I had to RTFM more than once, especially for handling raw HTTP requests, custom headers, and JSON payloads.

There were a few edge cases around image formatting, prompt tuning, and post validation. But once everything was wired up, the system became very reliable.

---
## Results

The automation has been running since March. In that time, it has created two posts per week, with featured images, consistent tone, and zero manual prompting. The only human involvement now is the final approval step before publishing.

Because I used **GPT-4o Mini** and **DALL·E 2**, and avoided complex single-shot prompts to expensive models, the operating cost has remained incredibly low.

We loaded $5 USD onto the OpenAI account when it launched. Six months later, we still have $3.50 remaining.

The system paid for itself within the first two weeks, and it continues to save me time every week, letting me focus on higher-value work.
