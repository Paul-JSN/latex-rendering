---
name: latex-rendering
description: Write LaTeX math in chat so it actually renders instead of showing raw code. Use whenever a reply contains any math or LaTeX (equations, proofs, matrices, linear algebra, calculus, physics), and whenever the user says math is not rendering, formulas look broken, or asks to "redo the LaTeX". Also use for 수식, 라텍스, 렌더링 오류. Apply it even if the user never mentions rendering.
---

# LaTeX Rendering in Chat

Most chat UIs process a message in two passes: a Markdown parser runs first, then a math renderer (usually KaTeX or MathJax) handles what is left. If Markdown touches a character before the math renderer sees it, the formula can break even when the LaTeX itself is valid, and the user sees raw code. These rules come from real failures. Follow them while writing, then run the checklist at the end before sending.

## Rules

### 1. Put every display block at the top level, alone
Write `$$ ... $$` on its own lines, with a blank line before and after. Do not put it inside a blockquote (`>`), a list item, a table cell, or an indented callout.

Why: Markdown handles `>` before math. A multi-line `$$` block inside a quote gets its start and end delimiters tangled, and the whole formula stops rendering. If a theorem or definition is styled as a quote, keep the text in the quote and move the equation out below it. If a list needs an equation, end the list, write the block at top level, then continue.

### 2. Use inline `$...$` carefully, and fall back if it prints raw
Short inline math is fine in most environments. If the user reports that `$...$` shows up as literal dollar signs, stop using it for the rest of the conversation:
- Put any real formula in its own `$$` block.
- Write trivial symbols (single variables, numbers, ≤, ≥, ε, δ) as plain text or Unicode.

Also avoid a bare `$` in prose (prices, for example), since it can pair with a later `$` and swallow text between them.

### 3. Keep the vertical bar `|` out of math
Markdown treats `|` as a table delimiter before LaTeX is parsed. The classic failure is a column spec like `\begin{array}{ccc|ccc}`.
- Use `\begin{pmatrix} ... \end{pmatrix}` instead of `array`.
- For an augmented-matrix divider, a conditional, or "such that", use `\mid`.
- For absolute value and norms, use `\lvert x \rvert` and `\lVert x \rVert`.
- For determinants, use `\begin{vmatrix} ... \end{vmatrix}`.

### 4. Do not stack lines over or under arrows
Constructs like `\xrightarrow{\substack{a \\ b}}` put a nested `\\` inside an argument, which many renderers and Markdown preprocessors misparse. Move the annotation (for example, row operations such as R2 ← R2 − R1) into a plain-text line outside the math. A short one-line label above an arrow is fine.

### 5. Split long math into short blocks
A single `$$` holding several matrices plus arrows is fragile. Use one block per step, with a short text line between steps saying what was done. A chain of equalities can share one block for a few lines; if it grows past that, split it.

### 6. General hygiene
- No blank lines inside a `$$` block. Many parsers end the block at the first blank line.
- Use `\\` line breaks only inside environments built for them (`aligned`, `cases`, matrices).
- Keep prose out of math. If words are unavoidable inside a formula, wrap them in `\text{...}`.
- Outside math, runs of `_` or `*` can be read as italics or bold. Put such expressions in a math block or in code formatting.

## When the user says it is still not rendering

Do not resend the same output. Work out which rule the broken part violated (inside a quote? contains `|`? block too long? stray `$`?), fix only that part, and say in one line what you changed. If none of the rules explains it, ask the user what they see (raw code, partial render, or an error message) and record the new cause so the pattern is not repeated.

## Pre-send checklist
1. Every `$$` block is top level, with blank lines around it (not in a quote, list, or table).
2. No stray `$` in prose, and no inline math if the user has reported it failing.
3. No `|` inside math (`\mid`, `\lvert`, `vmatrix` used instead).
4. Nothing multi-line inside `\xrightarrow` or similar.
5. Each block holds one step or a short chain.
6. No blank lines inside any block.
