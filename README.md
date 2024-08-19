# GSoC 2024 Report: P4 Compiler Documentation
The official report of [my project in Google Summer of Code 2024](https://summerofcode.withgoogle.com/programs/2024/projects/u2HpJKI2)  
**Mentors:** [Davide Scano](https://github.com/Dscano), [Fabian Ruffy](https://github.com/fruffy) <br> 
**Organization:** [The P4 Language Consortium](https://p4.org/)  <br>
**Contributor:** [Adarsh Rawat](https://github.com/AdarshRawat1)    <br>   

![Gsoc2024 - The P4 Language Consortium](https://github.com/user-attachments/assets/9a9ba7f0-772e-464f-a1e9-81d42286bc6e)


## Table of Contents
1. [Project Overview](#project-overview)
2. [Technical Considerations](#technical-considerations)
   - [Doxygen vs. Sphinx: Documentation Tools Comparison](#doxygen-vs-sphinx-documentation-tools-comparison)
   - [Personal Repository & Netlify for Builds](#personal-repository--netlify-for-builds)
   - [Deployment Options](#deployment-options)
   - [Workflow Considerations and Challenges](#workflow-considerations-and-challenges)
3. [Project Phases and Achievements](#project-phases-and-achievements)
   - [Phase 1: Development](#phase-1-development)
   - [Phase 2: GitHub Actions Integration](#phase-2-github-actions-integration)
4. [Key Improvements](#key-improvements)
   - [High-Quality Diagrams with Interactive SVGs](#high-quality-diagrams-with-interactive-svgs)
   - [Enhanced Visual Appeal](#enhanced-visual-appeal)
   - [Well-Organized Structure with Enhanced Features](#well-organized-structure-with-enhanced-features)
5. [Closing Note](#closing-note)
   - [A Touch of Fun in the P4 Compiler Architecture Diagram](#a-touch-of-fun-in-the-p4-compiler-architecture-diagram)
   - [Reflections and Learnings](#reflections-and-learnings)
6. [Final Thoughts](#final-thoughts)
7. [References](#references)


## Project Overview

For Google Summer of Code 2024, I developed an automated documentation system for the P4 Compiler. The project focused on improving the accessibility and organization of the P4 Compiler’s documentation, leveraging Doxygen for documentation generation and GitHub Actions for automation. The results include a centralized, up-to-date documentation site hosted on GitHub Pages, with enhanced navigation and interactive features.
![image](https://github.com/user-attachments/assets/b3d55755-51e6-4f17-8a3e-b18370a92e32)


## Technical Considerations
> [!NOTE]  
> Previews in this report are mockups and do not reflect the current state of the documentation. They were used for iterative development during the project.

### Doxygen vs. Sphinx: Documentation Tools Comparison

Initially, I considered using Sphinx with Doxygen and Breathe for documentation [[1]](#1-medium-article-on-using-sphinx-doxygen-and-breathe-c-documentation-with-doxygen-cmake-sphinx--breathe)[[2]](#2-clear-functional-c-documentation-with-sphinx-breathe-doxygen-cmake). However, after discussing with mentors and evaluating the tools, we decided to use Doxygen with Doxygen Awesome CSS for the following reasons:

- **Doxygen**: Provides straightforward and easy-to-generate documentation but lacks mobile responsiveness. Examples of Doxygen-generated docs can be found here:
  - [Eigen Documentation](https://eigen.tuxfamily.org/dox/)
  - [ALIB Documentation](https://alib.dev/)

- **Doxygen + Awesome CSS**: Addresses the mobile responsiveness issue and improves visual appeal with enhanced CSS styling. It allows for customization without modifying the HTML structure and includes dark mode support. You can view an example here:
  - [P4C Prototype Documentation](https://661bbb9fb70e8b584e959c0e--p4c-pototype3.netlify.app/)

- **Doxygen + Breathe / Exhale + Sphinx**: Sphinx offers greater flexibility and control over documentation structure compared to Doxygen’s markup. It's particularly useful for including tutorials. However, we decided against it for our project to avoid added complexity, especially since tutorials would be maintained separately. 

After discussions with mentors, it became clear that Doxygen with Doxygen Awesome CSS met all our needs while avoiding the additional complexity of incorporating Sphinx and Exhale. This choice allowed us to utilize a tool familiar to maintainers and contributors without introducing a new layer of complexity.

### Personal Repository & Netlify for Builds
1. Utilized a personal repository with Netlify for initial builds and sharing mockups. This approach allowed for iterative development and testing before deploying to GitHub Pages on the official repository.
2. Created mockups during iteration:
   - [Doxygen output with obsolete documentation](https://p4-compiler-docs.netlify.app/)
   - [Sphinx version with “pydata” theme](https://p4-docs.netlify.app/)
   - [Doxygen + Doxygen Awesome CSS With Nav Bar](https://661bbb9fb70e8b584e959c0e--p4c-pototype3.netlify.app/)
   - [[Current] — Doxygen + Doxygen Awesome CSS Side bar only theme](https://p4lang.github.io/p4c/)

### Deployment Options
After considering various deployment platforms such as Netlify and Vercel, GitHub Pages was selected for deployment. This choice was motivated by the desire to avoid additional hosting costs and administrative overhead. This decision was reached after consulting with mentors.

### Workflow Considerations and Challenges
#### Background : Commenting on PR   
Handling **pull requests (PRs) from external forks** presents unique challenges in GitHub Actions, primarily due to security restrictions imposed by GitHub. When a PR is triggered from a fork, the `GITHUB_TOKEN` generated for that workflow run has limited permissions, specifically lacking write access to the repository. This limitation is designed to prevent unauthorized actions that could potentially compromise the security of the repository.
As a result, actions such as commenting on a PR, which require write access, fails with an error message like `Resource not accessible by integration`.
#### Workaround 
**Using `pull_request_target` event instead of `pull_request`**: This event runs in the context of the base repository, where the workflow file is stored, and grants the `GITHUB_TOKEN` the necessary write permissions. 
However, this approach comes with its own set of risks, as it executes code from the forked repository with elevated privileges, which could potentially be malicious.

#### Background : Label Check Implementation for `pull_request_target` Trigger
In my GitHub Actions workflow, I encountered a limitation with the `pull_request_target` trigger. Specifically, the conditional check using contains on PR labels wasn't functioning as expected.

```bash
# This condition didn't work with the pull_request_target trigger
if: ${{ contains(github.event.pull_request.labels.*.name, 'documentation') }}
```
#### Workaround 
**Custom Label Check** To ensure that only pull requests with the 'documentation' label trigger the workflow, I implemented a custom label check using actions/github-script@v7(GitHub’s REST API).

This script iterates over the labels attached to the PR and returns true if the `documentation` label is present. This condition is then used to control subsequent steps in the workflow.
```bash 
- name: Check PR Label
  id: check-label
  if: ${{ github.event_name == 'pull_request_target' }}
  uses: actions/github-script@v7
  with:
    script: |
      const { data: labels } = await github.rest.issues.listLabelsOnIssue({
        owner: context.repo.owner,
        repo: context.repo.repo,
        issue_number: context.issue.number
      });
      const hasDocumentationLabel = labels.some(label => label.name === 'documentation');
      return hasDocumentationLabel;
```


## Project Phases and Achievements

### Phase 1: Development

#### Documentation Generation
- Configured Doxygen and Doxygen Awesome CSS for organized documentation.
- Utilized Doxygen’s diagram generation to visualize class and function relationships.
- Enhanced repository content organization for improved clarity.

#### Documentation Sync and Cleanup
- Updated documentation structure to match the latest codebase.
- Fixed broken links throughout the documentation.

#### Centralized Hosting on GitHub Pages
- Deployed documentation to GitHub Pages for real-time updates.

#### Interactive Architecture Diagram
- Added a dynamic homepage diagram illustrating the P4 Compiler architecture with links to relevant sections.

#### Onboarding and Changelog Integration
- Created comprehensive Contribution Guidelines to assist onboarding of new contributors, covering best practices, coding standards, and setup instructions.
- Added changelogs to the repository to keep track of changes, making it easier for contributors to follow updates and understand the project’s evolution.

### Phase 2: GitHub Actions Integration

#### Automated Build Process
- Developed a GitHub Action to automate documentation builds.

#### PR Review Enhancement
- Automated preview creation for pull requests, including preview link and deployment status in comments.
- Managed broken builds by updating PR comments with status information.

## Key Improvements

### High-Quality Diagrams with Interactive SVGs

- **Old Approach**: Diagrams were generated as PNGs in Doxygen. While functional, PNGs can become difficult to navigate, especially with complex graphs.
- **New Approach**: We’ve switched to interactive SVGs, which maintain high quality even when zoomed in or scaled. This change significantly enhances the usability of complex diagrams. <br>
  [Click to View Previous Diagrams](#https://p4-compiler-docs.netlify.app/class_p4_1_1_control_plane_a_p_i_1_1_p4_runtime_arch_handler_iface.html) | | [Click to view Updated Diagrams](#https://p4c-pototype3.netlify.app/class_p4_1_1_control_plane_a_p_i_1_1_p4_runtime_arch_handler_iface.html)

### Enhanced Visual Appeal
- **Dark Mode & Improved Formatting**: The updated documentation features a dark mode option and a better-formatted navigation bar, making the reading experience more comfortable and visually appealing.

### Well-Organized Structure with Enhanced Features
- **Permanent Links to Headings**: Added permanent links to specific sections of the documentation for easier sharing and navigation.
- **Customized Theme**: The theme is based on the P4 color scheme, offering a consistent visual experience.

## Closing Note

### A Touch of Fun in the P4 Compiler Architecture Diagram
In the spirit of keeping things lively, we came up with the idea of sneaking a little Easter egg into the P4 Compiler’s documentation during one of our team meetings. If you’re feeling curious (or just bored), try clicking on the P4 Compiler logo in the top left corner of the homepage. What happens next? Well, the architecture diagram decides to throw a tiny celebration by animating its lines, as if to say, “Surprise! You found us!”

![Interactive Animation Toggle in P4 Compiler Architecture Diagram](https://github.com/user-attachments/assets/da586665-08b7-42ff-93ea-461903161317)

This quirky touch isn’t just for giggles — it’s a subtle reminder that even in the serious world of compiler documentation, a bit of fun can go a long way. Plus, it might just keep you on your toes, wondering what other surprises might be hiding in the corners of the site. Spoiler: there aren’t any… yet.

### Reflections and Learnings
During this project, I often had to change my approach and come up with new ideas to solve problems. I learned how a bit of help from others can quickly solve issues that might take me hours to figure out on my own.

I learned the importance of learning in public. Initially, I felt skeptical about sharing my progress and failures openly, but the overwhelmingly positive response and genuine support from the community and mentors reassured me. This encouragement was instrumental in helping me work confidently towards the project.

### Doxygen Update and the Power of Open Source
Background on Doxygen v1.12.0 Feature
Doxygen v1.12.0 introduced a critical feature enabling the use of GitHub-flavored comments, allowing Doxygen commands to be hidden in GitHub previews using the `<!--! ... -->` syntax. This feature was the direct result of a feature request [[3]](#3-feature-request--capability-to-render-github-flavor-markdown-comments) I submitted to address the issue of visible Doxygen commands cluttering README files and other Markdown documents.

#### Impact of the Update
- **Updated Workflow:** I updated the Doxygen workflow action from v1.11.0 to v1.12.0, ensuring that all documentation now takes advantage of this new feature. This update mandates the strict use of Doxygen v1.12.0 for consistent output across the project.
- **Enhanced Documentation:** By adopting this new comment style, all Doxygen commands are now hidden in GitHub previews, making the documentation cleaner and more professional.
- **Dependency Management:** Dependencies were updated accordingly to support this new Doxygen version. This change requires all contributors to upgrade to `v1.12.0` to avoid confusion during documentation merges, particularly when using commands like `<!--!\include{doc} "../lib/README.md"-->` as they will not be processed by previous versions of doxygen.

#### Reflections on Open Source Collaboration
This experience highlighted the immense power of open-source tools and communities. The rapid incorporation of the feature request into Doxygen's latest release underscored the collaborative spirit and responsiveness that are the hallmarks of open source. It also reinforced the importance of contributing to open-source projects, as even small changes can have a significant impact on a wide range of users.

In this project, I saw firsthand how contributions from the community can lead to enhancements that benefit everyone. Working within an open-source ecosystem allowed me to both give back and learn from others, reinforcing the idea that open-source software isn't just about code — it's about community.

## Final Thoughts
This was one of the best experiences I’ve ever had. I sincerely thank Google and my project mentors for selecting me for the GSoC program and giving me the opportunity to discover how amazing coding can be.

### What's Left to Do (Future Steps)
Documentation is an ever-evolving aspect of the project. Future work includes regularly updating the documentation to reflect new features, changes, and improvements.
The instructions for using Doxygen and maintaining the documentation are currently under review. Once finalized, they will assist all contributors in effectively updating and expanding the documentation. Continued attention to these guidelines, along with regular updates and automated changelog integration, will ensure the documentation remains accurate and comprehensive.

## References

##### [1]: [Medium article on using Sphinx, Doxygen, and Breathe: C++ Documentation with Doxygen, CMake, Sphinx & Breathe](https://medium.com/practical-coding/c-documentation-with-doxygen-cmake-sphinx-breathe-for-those-of-use-who-are-totally-lost-part-2-21f4fb1abd9f)
##### [2] [Microsoft Dev Blog on functional C++ documentation: Clear Functional C++ Documentation with Sphinx, Breathe, and Doxygen](https://devblogs.microsoft.com/cppblog/clear-functional-c-documentation-with-sphinx-breathe-doxygen-cmake/)

##### [3]: [Feature Request : Capability to render GitHub flavor Markdown comments](https://github.com/doxygen/doxygen/issues/10959)
