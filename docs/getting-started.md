---
layout: default
title: Getting Started
nav_order: 2
description: "Guide to getting started with Vortex Lab."
---

# Getting Started
{: .no_toc }

This guide covers how to set up, develop, and preview the documentation locally.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

To run and preview the documentation locally, ensure you have:

- **Ruby** (version 3.1 or higher)
- **Bundler** (`gem install bundler`)

## Local Development

1. Navigate to the `docs` directory:

   ```bash
   cd docs
   ```

2. Install dependencies:

   ```bash
   bundle install
   ```

3. Start the local Jekyll development server:

   ```bash
   bundle exec jekyll serve
   ```

4. Open your browser and navigate to:

   ```
   http://localhost:4000/vortex_lab/
   ```

{: .tip }
> Any changes you make to Markdown files in `docs/` will automatically trigger a rebuild while the development server is running.

---

## Adding New Pages

To create a new documentation page, add a Markdown (`.md`) file inside the `docs/` directory with front matter:

```markdown
---
layout: default
title: Your Page Title
nav_order: 3
---

# Your Page Title

Your content goes here...
```

### Callouts Example

{: .note }
This is an informational note callout.

{: .tip }
This is a helpful tip callout.

{: .warning }
This is a warning callout.
