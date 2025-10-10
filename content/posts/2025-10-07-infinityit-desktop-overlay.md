---
title: "InfinityIT Desktop Overlay"
date: 2025-10-07T00:00:00Z
draft: false
tags:
  - Project
  - Data Science
  - Development
  - Software Engineering
  - Go
  - C#
  - PowerShell
categories:
  - Portfolio
  - Technology
summary: "A detailed account of the development process behind the InfinityIT Desktop Overlay, focusing on design, implementation, and iterative improvements."
thumbnail: "path/to/your/thumbnail/image.jpg" # Replace with the actual path to your thumbnail image
---

---

## InfinityIT Desktop Overlay

### Project Overview

The creation of the InfinityIT Desktop Overlay began when our managing director noticed a transparent overlay on a colleague's desktop. Inspired by its utility and aesthetics, he asked if we could implement a similar solution for our managed customers. The goal was to design an unobtrusive informational overlay that displayed critical details, such as hostname, company logo, and contact information, on Windows 10 and 11 desktops.

### Requirement Gathering

To ensure the project aligned with company and customer needs, I conducted thorough discussions with stakeholders. We identified key requirements:

- A semi-transparent overlay with no interactive functionality, designed to be completely click-through.
- Essential content: hostname, InfinityIT logo, and contact details.
- Deployment parameters included a fixed position in the bottom right corner of the desktop, resembling a watermark, and automatic reinstallation if it was disabled or removed.

### Initial Design Process

I began the design phase by creating a mock-up in Canva. After several iterations and feedback sessions, we settled on a size of 400x200 pixels, utilizing company-branded fonts and the logo to ensure visual relevance.

### Development of the First Version

#### Code Implementation

For the initial development, I opted to combine a transparent PNG with C# coding in Visual Studio and WPF. Though this approach initially seemed straightforward, I encountered challenges that required problem-solving and persistence to create a functional overlay.

#### Deployment Challenges

During the deployment process, I encountered significant challenges due to the complexity of using C#. I wrote a batch script to verify whether the overlay was installed and to create a startup entry for automatic launch.

However, complications emerged because the overlay relied on .NET runtimes installed on target systems. This necessitated additional checks and the potential need to push the runtime installer to other machines. Additionally, the multitude of files generated after compilation complicated the packaging process for deployment.

To manage the installation effectively via Datto RMM, I created three batch scripts: 
1. A monitoring script to ensure the overlay was installed and running.
2. An installation script triggered by the monitoring component.
3. A removal script.

### Testing and Feedback

Extensive testing was conducted on internal systems to identify and resolve any issues before the system-wide launch to our customers. This phase involved critical reflection on the functionality and usability of the overlay.

### Transition to the Second Version

Within the first year, inconsistencies in deployment and high resource consumption from the C# implementation drove my decision to refactor. The previous overlay used approximately 80 MB of RAM and required significant dependencies, which hindered efficiency.

#### Key Objectives for Rebuild

The goals for the new version included:

- Eliminating .NET requirements.
- Reducing memory consumption substantially.
- Creating a single executable without appearing in the add/remove software section.
- Streamlining the installation process.

#### New Technology Stack

After exploring various options, I transitioned to using Go for the application along with PowerShell scripts for RMM tasks. This sought simplicity while allowing me to maintain control over the design and functionality.

### Development Insights

Developing in Go required direct interaction with the Windows API rather than relying on bundled libraries. This allowed for a tailored overlay experience that could function seamlessly across various Windows versions. 

I benefited from the simplicity of embedding fonts and designs directly within the code, which streamlined the process considerably. The resulting executable was reduced to just 3 MB, and memory usage dropped dramatically to around 3 MB when running.

### Deployment Improvements

The switch to PowerShell scripts significantly improved efficiency. Simplifying the deployment process required only copying the executable to a designated location and modifying a single registry entry to enable startup. Additionally, error handling in PowerShell made debugging easier.

### Update Process

For future updates, I implemented a straightforward version-checking methodology based on timestamps. By utilizing a date-based system in the PowerShell script, I ensured that any overwritten updates occurred seamlessly without unnecessary complexity.

### Conclusion

The InfinityIT Desktop Overlay project demonstrates the importance of adaptability and the continuous improvement of solutions. By refining the technology stack and deployment process, I created a more efficient and effective product that meets our customers’ needs while prioritizing simplicity and performance. This experience has also reinforced my commitment to ongoing learning and adaptation in a fast-paced technical landscape.

---
