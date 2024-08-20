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
   - [Deployment Options: Transition to GitHub Pages](#deployment-options-transition-to-github-pages)
   - [Workflow Automation](#workflow-automation)
     - [Doxygen Automated Builds Workflow](#doxygen-automated-builds-workflow)
     - [Automated PR Preview Workflow](#automated-pr-preview-workflow)
     - [Challenges with PRs from External Forks](#challenges-with-prs-from-external-forks)
     - [Label Check Implementation for `pull_request_target` Trigger](#label-check-implementation-for-pull_request_target-trigger)
   - [Doxygen v1.12.0 Update: Enhancements and Open Source Impact](#doxygen-v1120-update-enhancements-and-open-source-impact)
3. [Key Improvements and Achievements](#key-improvements-and-achievements)
   - [Enhanced Documentation Quality](#enhanced-documentation-quality)
   - [Improved Documentation Accessibility](#improved-documentation-accessibility)
   - [Enhanced Visual and Functional Features](#enhanced-visual-and-functional-features)
   - [Efficient Onboarding and Project Management](#efficient-onboarding-and-project-management)
   - [Automated and Streamlined Processes](#automated-and-streamlined-processes)
4. [Closing Note](#closing-note)
   - [A Touch of Fun in the P4 Compiler Architecture Diagram](#a-touch-of-fun-in-the-p4-compiler-architecture-diagram)
   - [Reflections and Learnings](#reflections-and-learnings)
5. [Final Thoughts](#final-thoughts)
6. [References](#references)



## Project Overview

The primary objective of this project was to improve the documentation infrastructure of the P4 Compiler (p4c). This involved upgrading tools, automating deployment processes, enhancing the user interface, and ensuring that the documentation is both comprehensive and accessible. The project was aimed at making the documentation easier to maintain and more useful for both developers and users of the P4 language.

![image](https://github.com/user-attachments/assets/97be1b94-fcac-422f-b9e8-5da007948c0d)

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

## Documentation Development and Build Process
During the early stages of development, I utilized a personal repository with Netlify to create and share website mockups. This approach allowed for iterative testing and refinement before deploying to the official GitHub Pages site. The following mockups were created during this phase:
   - [Doxygen output with obsolete documentation](https://p4-compiler-docs.netlify.app/) This early mockup used Doxygen to generate basic documentation, which was hosted on Netlify.
     ![image](https://github.com/user-attachments/assets/c03dcc8e-31fa-4f47-b259-ba8e00da0490)
   - [Sphinx version with “pydata” theme](https://p4-docs.netlify.app/) An alternative mockup using Sphinx with the pydata theme was explored but later discarded in favor of Doxygen.
     ![image](https://github.com/user-attachments/assets/01bcdbb0-137d-4af1-899a-d25811aecc84)
   - [Doxygen + Doxygen Awesome CSS With Nav Bar](https://661bbb9fb70e8b584e959c0e--p4c-pototype3.netlify.app/) The mockup which was used as a base for the official documentation.
     ![image](https://github.com/user-attachments/assets/a1a3595c-ad41-49da-9221-53e55783b358)

### Deployment Options : Transition to GitHub Pages
After finalizing the mockups and build process, the documentation was deployed on GitHub Pages. This transition from Netlify to GitHub Pages streamlined hosting, ensured consistent integration with the main repository, and eliminated additional costs and administrative overhead. The choice was made after evaluating platforms like Netlify and Vercel and consulting with mentors.

## Workflow Automation 

### Doxygen Automated Builds Workflow
[PR - Configure: DOXYGEN for documentation generation](https://github.com/p4lang/p4c/pull/4701) <br>
[PR - Docs : Configure Doxygen GitHub pages deploy Action ](https://github.com/p4lang/p4c/pull/4702) <br>
An automated workflow was set up to handle Doxygen documentation builds. This process ensures that the documentation is updated consistently with the latest changes, providing a reliable and up-to-date reference for all contributors.

### Automated PR Preview Workflow
[PR - Feat: PR Preview Workflow via GitHub Pages](https://github.com/p4lang/p4c/pull/4848)
To make reviewing documentation changes smoother and more transparent, I introduced a new PR preview workflow. This update enhances how we assess documentation updates before they merge into the main branch by using GitHub Pages for live previews. This means we can see exactly how changes will look in real-time, making it easier to review and refine documentation.
![Preview of PR](https://github.com/user-attachments/assets/06c3998e-f67e-4376-b630-9ca31001a488)


#### Key Updates
- **PR Preview:** Automatically builds and deploys each documentation PR to a unique GitHub Pages URL.
- **Comment Updates:** Adds/Updates comments with preview links and build status.
- **Broken Builds:** Detects and reports any build issues.
#### Benefits
- **Immediate Feedback:** Real-time previews for faster review and adjustments.
- **Automatic Notifications:** Alerts for build statuses and failures.
#### Usage
When a PR is tagged with `documentation`, the workflow deploys a preview to GitHub Pages and posts a comment with the preview link and build status.

### Background : Challenges with PRs from External Forks  
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

### Doxygen `v1.12.0` Update: Enhancements and Open Source Impact
**Background on Doxygen v1.12.0 Feature**
Doxygen v1.12.0 introduced a critical feature enabling the use of GitHub-flavored comments, allowing Doxygen commands to be hidden in GitHub previews using the `<!--! ... -->` syntax. This feature was the direct result of a feature request [[3]](#3-feature-request--capability-to-render-github-flavor-markdown-comments) I submitted to address the issue of visible Doxygen commands cluttering the rendered views of Markdown documents(README Files) on GitHub.

#### Impact of the Update
- **Enhanced Documentation:** By adopting this new comment style, all Doxygen commands are now hidden in GitHub previews, making the documentation cleaner and more professional.
- **Updated Workflow:** I updated the Doxygen workflow action from `v1.11.0` to `v1.12.0`, ensuring that all documentation now takes advantage of this new feature. This update mandates the strict use of Doxygen v1.12.0 for consistent output across the project.
- **Dependency Management:** Dependencies were updated accordingly to support this new Doxygen version. This change requires all contributors to upgrade to `v1.12.0` to avoid confusion during documentation merges, particularly when using commands like `<!--!\include{doc} "../lib/README.md"-->` as they will not be processed by previous versions of doxygen.


## Key Improvements and Achievements

### Enhanced Documentation Quality
- **Organized Documentation:** Configured Doxygen and Doxygen Awesome CSS for better organization and clarity.
  <br>
  [PR - Docs : Configuring Doxygen Awesome CSS ](https://github.com/p4lang/p4c/pull/4737)
- **Interactive SVGs:** Replaced PNGs with interactive SVGs for high-quality, scalable diagrams. View  [Click to View Previous Diagrams](#https://p4-compiler-docs.netlify.app/class_p4_1_1_control_plane_a_p_i_1_1_p4_runtime_arch_handler_iface.html) | | [Click to view Updated Diagrams](#https://p4c-pototype3.netlify.app/class_p4_1_1_control_plane_a_p_i_1_1_p4_runtime_arch_handler_iface.html)
- **Dynamic Architecture Diagram:** Added a homepage diagram illustrating the P4 Compiler architecture with interactive links.
- Utilized **Graphviz's diagram generation** to visualize class and function relationships.

### Improved Documentation Accessibility
- **Documentation Sync and Cleanup:** Updated and fixed broken links to align with the latest codebase.
- **Centralized Hosting:** Deployed documentation to GitHub Pages for real-time updates and accessibility.
- **Permanent Links:** Added links to specific sections for easier navigation and sharing.
## Enhanced Visual and Functional Features
- **Dark Mode and Formatting:** Introduced dark mode and improved navigation bar formatting for a better reading experience.
- **Customized Theme:** Adopted a P4 color scheme for a consistent visual experience.

### Efficient Onboarding and Project Management
- **Contribution Guidelines:** Created detailed guidelines to assist new contributors with best practices and setup instructions.<br>
  [PR - [docs] Add Comment Style Guide](https://github.com/p4lang/p4c/pull/4573)
- **Changelog Integration:** Added changelogs to track changes and help contributors stay informed about project updates.<br>
  [PR - [Docs] Add initial CHANGELOG.md with changelogs from previous release](https://github.com/p4lang/p4c/pull/4708)
### Automated and Streamlined Processes
- **GitHub Actions Integration:** Developed actions to automate documentation builds and previews.
- **PR Review Enhancements:** Automated preview creation for pull requests, with status updates and links for efficient review.


## Closing Note

### A Touch of Fun in the P4 Compiler Architecture Diagram
In the spirit of keeping things lively, we came up with the idea of sneaking a little Easter egg into the P4 Compiler’s documentation during one of our team meetings. If you’re feeling curious (or just bored), try clicking on the P4 Compiler logo in the top left corner of the homepage. What happens next? Well, the architecture diagram decides to throw a tiny celebration by animating its lines, as if to say, “Surprise! You found us!”

![Interactive Animation Toggle in P4 Compiler Architecture Diagram](https://github.com/user-attachments/assets/da586665-08b7-42ff-93ea-461903161317)

This quirky touch isn’t just for giggles — it’s a subtle reminder that even in the serious world of compiler documentation, a bit of fun can go a long way. Plus, it might just keep you on your toes, wondering what other surprises might be hiding in the corners of the site. Spoiler: there aren’t any… yet.

### Reflections and Learnings
During this project, I often had to change my approach and come up with new ideas to solve problems. I learned how a bit of help from others can quickly solve issues that might take me hours to figure out on my own.

I learned the importance of learning in public. Initially, I felt skeptical about sharing my progress and failures openly, but the overwhelmingly positive response and genuine support from the community and mentors reassured me. This encouragement was instrumental in helping me work confidently towards the project.

#### Reflections on Open Source Collaboration
This experience highlighted the immense power of open-source tools and communities. The rapid incorporation of the feature request into Doxygen's latest release underscored the collaborative spirit and responsiveness that are the hallmarks of open source. It also reinforced the importance of contributing to open-source projects, as even small changes can have a significant impact on a wide range of users.

In this project, I saw firsthand how contributions from the community can lead to enhancements that benefit everyone. Working within an open-source ecosystem allowed me to both give back and learn from others, reinforcing the idea that open-source software isn't just about code — it's about community.


### What's Left to Do (Future Steps)
Documentation is an ever-evolving aspect of the project. Future work includes regularly updating the documentation to reflect new features, changes, and improvements.
The instructions for using Doxygen and maintaining the documentation are currently under review. Once finalized, they will assist all contributors in effectively updating and expanding the documentation. Continued attention to these guidelines, along with regular updates and automated changelog integration, will ensure the documentation remains accurate and comprehensive.

## Final Thoughts
This was one of the best experiences I’ve ever had. I sincerely thank Google and my project mentors for selecting me for the GSoC program and giving me the opportunity to discover how amazing coding can be.

## References

##### [1]: [Medium article on using Sphinx, Doxygen, and Breathe: C++ Documentation with Doxygen, CMake, Sphinx & Breathe](https://medium.com/practical-coding/c-documentation-with-doxygen-cmake-sphinx-breathe-for-those-of-use-who-are-totally-lost-part-2-21f4fb1abd9f)
##### [2] [Microsoft Dev Blog on functional C++ documentation: Clear Functional C++ Documentation with Sphinx, Breathe, and Doxygen](https://devblogs.microsoft.com/cppblog/clear-functional-c-documentation-with-sphinx-breathe-doxygen-cmake/)

##### [3]: [Feature Request : Capability to render GitHub flavor Markdown comments](https://github.com/doxygen/doxygen/issues/10959)
