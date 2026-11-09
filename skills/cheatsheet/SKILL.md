---
name: cheatsheet
description: "Turn a syllabus, lecture notes or slides into a one-page A4 formula sheet for an exam that allows one, laid out in two columns and ready to copy by hand or print. Use when the user asks for a cheat sheet, crib sheet, formula sheet, an A4 page of formulas, or what to write on the sheet they may bring to a test."
---

# Cheat sheet

Build the one page a student is allowed to bring to a test. The goal is the most useful page, not the most text: the act of choosing what goes on it is half the studying.

## Steps

1. **Find the rules.** Ask for the syllabus or the test rules if they are not given, and read what the sheet may contain: size (A4 or letter), one side or both, handwritten or printed, theory only or worked examples too. Follow those rules exactly and say which ones you followed. If nothing is stated, assume one A4 side, formulas and definitions only.
2. **List the topics on the test.** Take them from the syllabus week by week, or from the materials if there is no syllabus. Show the list first so the student can strike out what they already know cold.
3. **Pick what goes on the page.** For each topic keep only what is hard to remember under stress:
   - formulas with every condition attached (where a rule applies, sign conventions, "x → +∞ and x → −∞ separately")
   - the steps of a standard method as a short numbered list (for example: domain → derivative → critical points → sign table → extrema)
   - the one or two mistakes the course warns about
   Leave out anything the student said they know, long proofs, and worked examples unless the rules allow them.
4. **Lay it out.** Two columns, topics in the order of the syllabus, a bold heading per topic, no full sentences. Write formulas in plain Unicode (x², √, ∞, ≈, ∫, Σ, →) so they survive copying by hand.
5. **Make it fit.** Write the sheet as one HTML file, `cheatsheet.html`, using the template below, and tell the student to open it and print to PDF. If it runs past one page, cut the least useful lines first rather than shrinking the font below 8 pt, and say what you cut.
6. **Finish with three lines** under the sheet, not on it: the topics most likely to cost points, and what to practise tonight.

## Rules

- Only put on the sheet what is in the student's materials or is standard for the course. Never invent a formula; if you are unsure of one, mark it with "check" and tell the student.
- Every formula needs its conditions. A formula without "when it applies" is how points are lost.
- Write in the language of the course unless the student asks for another.
- If the rules say handwritten, remind the student to copy it by hand: a printed sheet can be taken away.

## Template

```html
<!doctype html><meta charset="utf-8"><title>Cheat sheet</title>
<style>
@page { size: A4; margin: 8mm; }
body { font: 8.5pt/1.25 "Helvetica Neue", Arial, sans-serif; columns: 2; column-gap: 6mm; margin: 0; }
h2 { font-size: 9.5pt; margin: 4pt 0 1pt; break-after: avoid; border-bottom: 0.5pt solid #999; }
p, li { margin: 0 0 1.5pt; }
ol, ul { padding-left: 11pt; margin: 0 0 2pt; }
.warn { color: #b00; }
</style>
<h2>1. Topic</h2>
<p>formula — when it applies</p>
<p class="warn">common mistake</p>
```
