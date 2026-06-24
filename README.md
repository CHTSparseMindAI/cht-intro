# SparseMind Courses

GitHub-native learning materials for SparseMind technology.

This repository is designed to be useful even before a website or LMS exists. Each course is made of Markdown lessons, progress checklists, practical labs, self-check quizzes, and source maps that can be read directly on GitHub, uploaded to Notion/Feishu, or later turned into a docs site.

## Available Courses

| Course | Audience | Start here |
|---|---|---|
| CHT New-Hire Curriculum | Research, engineering, product, BD, and strategy hires who need to understand SparseMind's CHT technology | [courses/cht/README.md](courses/cht/README.md) |

## How To Learn On GitHub

1. Open the course README.
2. Follow the lesson order.
3. Use the progress checklist to mark what you finished.
4. Do the labs in your own branch or local notes.
5. Use the quizzes for self-check.
6. End with the capstone presentation or memo.

## Suggested Repository Workflow

When you later turn this folder into a repo, learners can interact with the course through GitHub:

- use issues for lesson questions and reflections;
- submit a pull request for lab answers, summaries, or capstone memos;
- use Markdown checkboxes as a visible progress tracker;
- use GitHub Discussions for paper-reading notes if enabled;
- keep source papers and extracted text under each course's `sources/` folder.

## Course Design Principles

- Lessons are short enough to read in one sitting.
- Every lesson ends with an exercise.
- Labs turn concepts into artifacts.
- Quizzes include expandable answers.
- Claims are written carefully so research results do not become product overclaims.
- Source maps make it clear which papers support which lesson.

## Folder Layout

```text
SparseMindCourses/
├── README.md
├── courses/
│   └── cht/
│       ├── README.md
│       ├── COURSE_MAP.md
│       ├── lessons/
│       ├── workbook/
│       ├── labs/
│       ├── quizzes/
│       └── sources/
└── .github/
    ├── ISSUE_TEMPLATE/
    └── pull_request_template.md
```

## Status

The CHT course content was created from the local SparseMind paper folder and copied here on 2026-06-24. This is content-ready; you can create the actual GitHub repo later.

