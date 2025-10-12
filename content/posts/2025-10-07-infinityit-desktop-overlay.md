---
title: "Behind the Build - InfinityIT Desktop Overlay"
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

## Behind the Build: InfinityIT Desktop Overlay

### The Request

The idea for this project began when our Managing Director at InfinityIT, saw a subtle desktop overlay while visiting another business. It was semi-transparent and displayed some basic information on the desktop background. When he returned, he asked if I could create something similar and deploy it across all of our managed customer endpoints.

### Gathering Requirements

Before building anything, I needed to understand exactly what was expected. I met with the stakeholder and gathered the following requirements:

- Semi-transparent overlay
    
- Non-interactive and fully click-through
    
- Must display hostname, company logo, and contact information
    
- Must always appear in the bottom-right of the desktop
    
- Should be visually unobtrusive
    
- Must work on Windows 10 and 11
    
- Deployed and monitored using Datto RMM
    
- Should automatically reinstall and start again at logon if removed or stopped
    

### Version 1: The First Implementation

The initial version was built using C# with WPF in Visual Studio. For the visual design, I created a mockup in Canva using our company branding. I exported the design as a PNG with a transparent background and used C# to programmatically overlay it on the screen alongside the system hostname.

Although the visual side worked well enough, deployment quickly became problematic. The application relied on the .NET runtime, which meant every target system had to be checked for compatibility. If the runtime was not installed, the deployment script needed to push and silently install it before proceeding.

After compilation, the application generated dozens of files. These had to be packaged with an installer, which then needed to handle version removal, registry changes, and silent execution. The deployment process involved three separate batch scripts in Datto RMM: one for monitoring, one for installation, and one for removal. Debugging was difficult because batch scripting provided very little error handling.

Despite internal testing and successful deployment across a number of systems, the setup was too fragile and resource-heavy for what was essentially a passive overlay.

### Version 2: Rewriting for Simplicity and Performance

Within a year, I decided to refactor the project entirely. There were too many inconsistencies during updates and monitoring. The original implementation consumed approximately 80MB of RAM for a task that should have required almost none. It also required Visual Studio and produced a large number of files. These issues made maintenance frustrating and time-consuming.

I made a list of goals for the new version:

- Remove all .NET dependencies
    
- Reduce the memory footprint significantly
    
- Create a single, compact executable
    
- Avoid showing the application in Add/Remove Programs
    
- Improve script reliability and error handling
    
- Enable fast compilation and deployment
    
- Minimize the file size
    

After some testing with different languages, I settled on Go for the application and PowerShell for the deployment and monitoring scripts. This decision simplified everything. I no longer needed Visual Studio or an installer. The application was just a single `.go` file with 346 lines of code, compiled into a static 3MB executable.

### Technical Reflection

Using Go required a shift in approach. There are no high-level GUI frameworks like WPF, so I used direct Windows API calls to draw on the screen. This method felt more transparent and aligned with what I needed. There were likely third-party libraries available, but I wanted to avoid external dependencies and keep full control over what the application was doing.

Go also allowed me to embed the PNG and font directly into the binary using a simple directive. This ensured that everything needed was contained in a single file.

The final application used only around 3MB of RAM and worked across all tested versions of Windows 10 and 11. The compile time was nearly instant, and the file size stayed under 3.5MB, even with embedded assets.

### Simplified Deployment

Deployment became simple. Instead of installing dependencies or running complex installers, I just had to copy the executable to a specific folder and set a registry key to launch it at user login.

I replaced the old batch scripts with PowerShell to improve error handling. This change made it easier to detect problems and maintain consistency across installations.

### Handling Updates

Updating the overlay also required some thought. I explored version checks using command-line flags or resource files, but most methods felt unnecessarily complex. In the end, I used a simple file timestamp check. The PowerShell script would compare the installed file’s modified date with a known “current” version date. If the installed version was older, it would be replaced.

To avoid conflicts when the new version was compiled and deployed on the same day as an existing installation, I set the update script to check against the next day’s date. This ensured that any overlays installed earlier on that day would still be updated properly during deployment.

### A Note on AI

My early experience with C# relied heavily on AI tools to help with syntax and structure. However, I quickly found myself stuck in what I call prompt purgatory. AI answers were often vague, generic, or incorrect. They offered surface-level help but failed to handle deeper integration problems.

Eventually I decided to stop prompting and start reading. By going back to documentation and working directly with the Windows API, I was able to build something smaller, faster, and much more maintainable. The time spent digging into official sources paid off in clarity and control.

AI is still useful to me, but I make sure to start with pen and paper. I sketch the idea, plan the structure, and use AI only when I need a quick reference or second opinion.
