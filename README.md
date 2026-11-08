# student-skills

A Claude Code plugin with three skills for students:

| Skill | What it does |
| --- | --- |
| `essay-check` | Grades your essay draft against the course rubric like a strict teacher: score per criterion, quotes from your text, the 3 fixes that add the most points. It does not rewrite the essay for you. |
| `doc-privacy` | Checks a .docx, .xlsx, .pptx or .pdf before you submit it: your name in the metadata, editing time, comments, tracked changes, hidden or white text, invisible zero-width characters. Can make a clean copy. |
| `exam-prep` | Turns your lecture notes or slides into a practice exam with an answer key, then quizzes you one question at a time and lists your weak spots. |

## Install

At the prompt of a Claude Code session:

```
/plugin install student-skills --marketplace econnx/student-skills
```

Answer `y` to add the marketplace, then pick a scope.

From a shell:

```
claude plugin install student-skills --marketplace econnx/student-skills
```

## Use

Just ask in your own words, Claude picks the right skill:

- "Check my essay `essay.docx` against the rubric in `criteria.pdf`"
- "Does `report.docx` have any hidden data before I send it?"
- "Make me a practice exam from the slides in `lectures/`"

## Related

- [DocPeek](https://econnx.github.io/docpeek/): the same file check in your browser, no install.

## License

MIT
