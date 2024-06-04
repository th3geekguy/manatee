# Contributing

Welcome to our technical knowledge base! To ensure consistency and readability, please follow this formatting guide when contributing content. Our knowledge base uses Markdown for formatting, so familiarity with Markdown syntax is essential. Here’s a comprehensive guide to help you create well-structured and clear documentation.

## Formatting

### Title
- Use a single `#` followed by a space for the main title.
```
# Main Title
```

### Headings
- Use `##` for second-level headings, `###` for third-level headings, and so on.
```
## Second-Level Heading
### Third-Level Heading
```

### Table of Contents
- If the document is long, include a table of contents at the top. Use bullet points and link to the headings.
```
- [Section 1](#section-1)
- [Section 2](#section-2)
```

### Bold and Italics
- Use `**bold**` for bold text and `_italics_` for italic text.
```
**This text is bold**
_This text is italicized_
```

### Lists
- Use `-` or `*` for unordered lists, and `1.`, `2.` for ordered lists.
```
- Item 1
- Item 2

1. First item
2. Second item
```

### Code
- Inline code: Use backticks for inline code snippets.
```
Use the `docker run` command to...
```
- Code blocks: Use triple backticks for code blocks. Specify the language for syntax highlighting.
```
 ```bash
 docker run -it ubuntu
 ```
```

### Links
- Use `[link text](URL)` for hyperlinks.
```
For more information, visit [Docker's official site](https://www.docker.com).
```

### Images
- Use `![alt text](URL)` for images. Include alt text for accessibility.
```
![Docker Logo](https://www.docker.com/assets/images/logo.png)
```

### Tables

- Use `|` to create tables. Separate headers and rows with `|` and `-`.
```
| Column 1 | Column 2 |
|----------|----------|
| Data 1   | Data 2   |
```

### Blockquotes

- Use `>` for blockquotes.
```
> This is a blockquote.
```

### Horizontal Rules

- Use `---` for horizontal rules to separate sections.
```
---
```

### Admonitions/Call-Outs

These are great for adding a short blurb of content without interrupting the flow of the article. Think of them as an aside or a quick note.

#### Special Notes/Tips

- Use `!!! note` to create an admonition note.
```
!!! note
    Note that this section may not apply for all customers (e.g. those running Windows).
```
_renders as_
!!! note
    Note that this section may not apply for all customers (e.g. those running Windows).

- Use `!!! tip` to create an admonition tip.
```
!!! tip
    Remember to update your Docker images regularly.
```
_renders as_
!!! tip
    Remember to update your Docker images regularly.

#### Warnings
- Use `!!! warning` for warnings.
```
!!! warning
    Deleting system files can cause your OS to become unstable.
```
_renders as_
!!! warning
    Deleting system files can cause your OS to become unstable.


#### Example Section
- Include practical examples where applicable.
```
!!! example "Example: Running a Container"
    To run an Ubuntu container, use the following command:
     ```bash
     docker run -it ubuntu
     ```
```
_renders as_
!!! example "Example: Running a Container"
    To run an Ubuntu container, use the following command:
     ```bash
     docker run -it ubuntu
     ```

### Metadata

#### Author Information
- Include author information at the end of the document.
```
---
**Author:** John Doe  
**Date:** 2024-05-17
```

#### Revision History
- Keep a log of revisions at the bottom.
```
## Revision History
- **2024-05-17:** Initial version by John Doe.
```

### Submission
- Submit your contributions via the designated repository (GitHub).
- Ensure your Markdown renders correctly by previewing it before submission.

### Review Process
- All contributions will undergo a review process for quality and accuracy.
- Be prepared to make revisions based on feedback.

## Article Guidelines

When writing a technical knowledge base article, it's crucial to start with a clear and detailed structure. This ensures the article addresses the necessary aspects of the issue and provides a comprehensive solution. Here are some guiding questions to help structure your article:

### What is the Problem We are Addressing?

- Clearly define the issue or question that the article aims to solve.
  - What specific problem are users encountering?
  - Why is this problem significant?
  - Who is likely to encounter it?
  - How critical is the issue?

!!! example

    ### Problem Statement
    Users are experiencing issues with Docker containers not connecting to the SSH server from within the container environment.

### What Environments Does This Apply To?

- Specify the environments, systems, and versions where the problem and solution are applicable.
  - Which operating systems are affected?
  - Are there specific versions of the software involved?

!!! example

    ### Applicable Environments
    - Operating Systems: Windows 10, WSL
    - Docker: All versions
    - SSH Server: General configuration

### What Is the Procedure To Take?

- Outline the steps taken to address the problem.
  - What are the detailed procedures for resolving the issue?
  - Include commands, code snippets, and configurations.
  - Provide output where applicable and helpful (e.g. what to look for in the output)
  - Provide diagnosis commands and notes if needed.

!!! example

    ### Procedure
    1. Verify network settings.
    2. Adjust Docker container network configuration:
    ```bash
    docker network inspect bridge
    ```
    3. Configure SSH settings within the container.

### What Additional Information Should We Know?

- Provide any extra details that can help understand the context or solution.
  - Are there any prerequisites or dependencies?
  - Any relevant background information?
  - What was the situation in which you used the steps to address?

!!! example

    ### Additional Information
    Ensure that the SSH server is configured correctly and that the necessary ports are open. This is especially true for containers running the `closed-firewall` docker image.

### Are There Any Edge Cases Where This Would Not Apply?

- Identify scenarios where the provided solution may not work.
  - Are there exceptions or special conditions?
  - Are there situations where this would break the existing setup?
  - How should these be handled (if known)?

!!! example

    ### Edge Cases
    - If using a custom network driver, these steps might differ.
    - Ensure there are no firewall rules blocking the connection.

### What Notes Do You Have for Our Internal Engineers?

- Include internal notes for engineers who might need to further investigate or improve the solution.
  - Are there known limitations?
  - Any internal references or documentation?
  - Specific case(s) where this originated?
  - Should this be only stepped through with customer rather than shared?

!!! example

    ### Internal Notes
    - Review the network bridge configuration in complex environments.
    - Reference internal ticket #12345 for background on this issue.

### Should This Be Shared with Customers?

- Decide if the article should be made available to customers or kept for internal use only.
  - Is the information sensitive or too technical for general users?
  - Should it be published in the public knowledge base?
  - TODO: we will write some mechanism wherein the document will be __not__ shareable

!!! example

    ### Customer Sharing
    This article is suitable for sharing with customers as it addresses common connectivity issues with clear steps.

### Example Outline

Here's how a complete outline might look using the questions above:

```
# Troubleshooting SSH Connectivity in Docker Containers

## Problem Statement
Users are experiencing issues with Docker containers not connecting to the SSH server from within the container environment.

## Applicable Environments
- Operating Systems: Windows 10, WSL
- Docker: All versions
- SSH Server: General configuration

## Procedure
1. Verify network settings.
2. Adjust Docker container network configuration:
 ```bash
 docker network inspect bridge
 ```
3. Configure SSH settings within the container.

## Additional Information
Ensure that the SSH server is configured correctly and that the necessary ports are open.

## Edge Cases
- If using a custom network driver, these steps might differ.
- Ensure there are no firewall rules blocking the connection.

## Internal Notes
- Review the network bridge configuration in complex environments.
- Reference internal ticket #12345 for background on this issue.

## Customer Sharing
This article is suitable for sharing with customers as it addresses common connectivity issues with clear steps.
```

By following this structured outline, you ensure your article is thorough, well-organized, and provides valuable information to both users and internal teams.
By adhering to this guide, you help maintain a high standard of quality and consistency in our technical knowledge base. Thank you for your contributions!
