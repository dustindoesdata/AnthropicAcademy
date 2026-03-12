# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a learning documentation repository tracking progress through [Anthropic Academy](https://github.com/dustindoesdata/AnthropicAcademy). It contains notes, exercises, summaries, and certificates — not a software project with a build system.

## Structure

Three learning tracks, each with subdirectories per course:

- `developer/` — Claude Code, Claude API, MCP, Agent Skills
- `cloud-and-platform/` — Amazon Bedrock, Google Cloud Vertex AI
- `ai-fluency/` — Fluency frameworks for various audiences

Each course folder follows this layout:
```
<course-name>/
├── notes/        # Markdown notes organized by lesson
├── exercises/    # Code exercises, prompt labs, API examples
├── summaries/    # Course-level takeaways
├── certificates/ # Completion certificates
└── README.md     # Per-course progress tracker
```

## Content Conventions

- Notes are raw markdown, organized by lesson within `notes/`
- Course `README.md` files track per-section completion status using checkboxes
- The main `README.md` tracks overall course completion across all tracks
- Exercises may include Python, TypeScript, or Jupyter notebooks as courses progress

## No Build System

There is no build, lint, or test pipeline — this is a documentation repo. Workflow is purely git-based: edit markdown, commit, push.
