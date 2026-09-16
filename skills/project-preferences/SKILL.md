---
name: project-preferences
description: Apply repository-specific preferences when planning, implementing, or reviewing changes in this project.
---

<!--
This file is part of AI Prompt Database
Copyright © 2023-2026 The Regents of the University of Michigan
Licensed under the GNU Free Documentation License v1.3 or later.
See <https://www.gnu.org/licenses/fdl-1.3.html>. See README for full license information.
-->

# AI Prompt Database

## Project preferences

Use this skill when planning, implementing, or reviewing changes in this repository.
Keep all project-specific preferences and workflows in this single file. This skill
supplements `AGENTS.md` and cannot weaken its security, privacy, accessibility,
licensing, testing, or authorization rules.

### Purpose and scope

AI Prompt Database is an open-source library of reusable generative AI prompt
examples for UMGPT, Maizey, ChatGPT, Gemini, Claude, and other assistants. It is
a content repository: almost every file is a Markdown page documenting one prompt,
its template, and real example outputs. There is currently no build step and no
application code.

### Environment and structure

- Each top-level category folder (`business`, `clinical`, `coding`, `data-analysis`,
  `education`, `just-for-fun`, `research`, `web-development`) holds one Markdown file
  per prompt.
- New prompt pages start from [`_template.md`](../../_template.md) and keep its
  section order: title, contributors, description, template, examples.
- Prompt pages use `<pre><code>` for templates, `<var>` for user-supplied values,
  `<kbd>` for example prompts, and `<samp>` for example outputs.
- Follow the repository's existing file and folder naming conventions when adding
  new files: lowercase kebab-case Markdown filenames inside a category folder.
- The repository is served as static content; a GitHub Pages interface is planned.
  Do not introduce components that need a server, database, or private API keys.

### Setup and verification

There is nothing to compile. Verify changes by previewing the Markdown rendering
and checking that relative links and image URLs resolve.

### Project constraints

- Prompt examples are published verbatim, so review every contributed prompt and
  output for secrets, PHI, and personal data before it is committed. Use synthetic
  examples where the original interaction contained real details.
- Example outputs come from real AI assistants. Do not silently edit them into
  something the model never produced; trim or annotate instead.
- AI Prompt Database™ is a trademark of The Regents of the University of Michigan.
  Preserve the trademark, copyright, license, and citation boilerplate in the
  README and page footers.
- Contributions arrive by pull request or email (see
  [`CONTRIBUTING.md`](../../CONTRIBUTING.md)); keep that page in sync with any
  change to the submission process.

### Project skills

- [create-media-pack](../create-media-pack/SKILL.md): repository branding and
  media pack generation.

### Conclusion

Keep the repository simple: one well-formed Markdown page per prompt, safe
example content, and intact licensing.

### Additional resources

- [Project instructions](../../AGENTS.md)
- [Skills index](../../SKILLS.md)
- [Contribution guide](../../CONTRIBUTING.md)
