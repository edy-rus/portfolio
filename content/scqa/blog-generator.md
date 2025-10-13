---
title: "Automated Blog Generator"
image: "/images/automated-blog-generator.png"
---
## Project Summary: Automated Blog Generator

**Situation:**  
To improve SEO and provide value to customers, I was tasked with writing regular blog posts for InfinityIT. The expectation was one post per week, formatted for WordPress with proper SEO tags, featured images, and brand alignment. All posts were created manually, from content to image to publication.

**Complication:**  
Due to the wide scope of my role, it became increasingly difficult to dedicate the two hours of focused time each week required to complete a post. The process of prompting early AI tools, reviewing results, generating images, and formatting for WordPress was time-consuming. Inconsistent publishing reduced the blog's impact, and the internal cost of each post was high.

**Question:**  
How could I automate the entire workflow to deliver consistent, high-quality, brand-aligned blog content without creating generic AI output or increasing operational costs?

**Answer:**  
I built an automated system using n8n, GPT-4o Mini, DALL·E 2, and the WordPress API. The pipeline runs twice weekly and generates blog content from a curated brand brief. A second AI agent scores the output for tone and structure. Posts that fail are automatically regenerated with adjustments. Featured images are created using DALL·E 2 with prompt constraints to fit website requirements. The system posts drafts directly to WordPress and sends update notifications via Microsoft Teams. By using smaller, efficient models and a scoring mechanism to ensure quality, the total cost has remained extremely low while output consistency and quality have increased significantly.
