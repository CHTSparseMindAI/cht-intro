---
name: cht-lesson-generator
description: Generate a new CHT course lesson consistent with the existing style. Use when adding a new topic to the CHT Onboarding Course. Provides formatting rules, tone guidelines, structural templates, and paper citation conventions.
---

# CHT Lesson Generator

Generate a new lesson for the CHT Onboarding Course following the exact style of the existing 10 lessons in `lessons/`.

## Lesson Template

```markdown
# [Topic Title — Descriptive, Specific]

**Learning objective:** [One sentence: what the reader will understand after this lesson.]

---

## [First Major Section]

[Content — see Style Rules below.]

---

## [Second Major Section]

[Content.]

---

## Key Takeaways

- [Bullet 1 — actionable, memorable.]
- [Bullet 2.]
- [Bullet 3.]

**Previous:** [Previous Lesson Name](XX-previous-file.md)  
**Next:** [Next Lesson Name](XX-next-file.md)
```

## Style Rules

### Tone
- **Engineer/student-oriented**, not business or product.
- Coursera-style: educational, clear, self-contained.
- Never use: "new hire", "onboarding", "SparseMind", "our paper", "local folder", "local note".
- Prefer direct, precise technical language. Avoid marketing fluff.

### Structure
- Every lesson starts with `**Learning objective:**` on its own line, immediately after the title.
- Sections separated by `---` (horizontal rule).
- Use `##` for all section headings — never `###` (keep it flat, Coursera-style).
- Section headings are descriptive noun phrases or questions, not single words.
- End every lesson with `## Key Takeaways` followed by 2–4 bullet points.
- Final lines: `**Previous:** [link]` and `**Next:** [link]`.

### Content Guidelines
- **No exercises.** The course is read-only educational content.
- **No read time estimates.**
- **No "new hire" or onboarding language.** Address the reader as a general technical learner.
- Explain concepts from first principles. Assume ML knowledge but not CHT knowledge.
- Use analogies sparingly — only when they genuinely clarify.
- Prefer concrete technical descriptions over vague claims.

### Math
- Use KaTeX: `$inline$` for inline, `$$block$$` for display.
- Define all variables after formulas.
- Include plain-English intuition after every formula.

### Paper Citations
- Always cite papers with **bold name**, linked URL, venue, and year:
  - `**[Paper Title](URL)** (Venue Year)`
- Use the papers table in `README.md` as the authoritative source for links and venues.
- Do not reference "local papers", "source folder", or any internal file system.

### Tables
- Use Markdown tables for comparisons, vocabularies, or metric lists.
- Keep tables narrow (max 4 columns).
- Left-align text columns, right-align numeric columns with `---:`.

### Blockquotes
- Use `>` for key insights or memorable one-liners.
- Keep blockquotes to 1–2 sentences.

### Code Blocks
- Use fenced code blocks for pseudocode or architecture diagrams.
- Label with `text` (not a programming language) for pseudocode.
- Use `mermaid` for flowcharts.

### Navigation
- Every lesson ends with `**Previous:** [Name](file.md)` and `**Next:** [Name](file.md)`.
- Lesson 01 has no Previous. Lesson 10 (or last lesson) has no Next.
- Update neighboring lessons' navigation links when inserting a new lesson between existing ones.

## File Naming

- Format: `NN-topic-slug.md`
- NN is the lesson number (01–99).
- Slug is lowercase, hyphenated, descriptive.
- Examples: `01-why-sparsity-matters.md`, `04-the-cht-learning-loop.md`

## Before Writing a New Lesson

1. Read `README.md` for the papers table — cite papers using exactly those links.
2. Read adjacent lessons for navigation context and to avoid content overlap.
3. Match the depth level: lessons are ~100–180 lines of Markdown, covering one well-scoped topic.
4. Update `README.md` to include the new lesson in the Lessons table.
