# Grammar, Correctness, and Clarity Review

Files reviewed: `README.md`, `PRE-WORKSHOP.md`, `Activity1/assignment.txt`,
`Activity1/scanner-algo.txt`, `Activity2/assignment.txt`, `Activity3/assignment.txt`,
`report.md`, `instructional-design-review.txt`.

---

## Errors and Typos

### PRE-WORKSHOP.md

**Duplicate step number (line 35)**
Two consecutive steps are both numbered `5.`. The second should be `6.`, and the current `6.` should be `7.`.

```
5. Wait for GitHub to build your codespace...     ← this stays as 5
5. Test the codespace by trying each...           ← should be 6
6. Stop the codespace by clicking...              ← should be 7
```

**Bare URLs without scheme (lines 20, 22)**
`github.com` is used as a link URL without `https://`. Line 26 correctly uses the full URL. For consistency and correctness:

```
- Sign up for a [GitHub](github.com) account.
+ Sign up for a [GitHub](https://github.com) account.

- Sign in to [GitHub](github.com).
+ Sign in to [GitHub](https://github.com).
```

---

### Activity2/assignment.txt

**Misspelling (line 27)**
```
- parantheses
+ parentheses
```

**Duplicate word (line 49)**
```
- Notice how the the parse trace starts with the *start symbol*
+ Notice how the parse trace starts with the *start symbol*
```

---

### Activity3/assignment.txt

**Misspelling (line 24)**
```
- evalutes to 2, not -2, so order is important.
+ evaluates to 2, not -2, so order is important.
```

**Misspelling (line 48)**
```
- for semantic analyis, and (B) how subclass names are used
+ for semantic analysis, and (B) how subclass names are used
```

---

### report.md — Incorrect "Year / Reference Mismatches" entries

The report's "Year / Reference Mismatches" section (lines 163–168) claims:

> `README.md` line 1 — Title says "CCSCNE 2025" — should be 2026
> `PRE-WORKSHOP.md` lines 24/26 — References `CCSCNE-2025` repo URL — should be 2026

These claims are **wrong**. Both files already say 2026:
- `README.md` line 1: `# PLCC Workshop - CCSCNE 2026`
- `PRE-WORKSHOP.md` line 26: `https://github.com/ourPLCC/CCSCNE-2026`

This entire section of the report should be removed, and the corresponding entries in the "Issues Found" summary table should be removed as well.

---

## Clarity Issues

### Activity1/assignment.txt

**Step 3 body is visually detached (lines 16–18)**
The paragraph explaining what `plccmk -c` does sits flush-left with no indentation, making it look like a separate paragraph rather than continuation of step 3. Indenting it (or separating it from the numbered list with a blank line and indent) would make the structure clearer.

**`ctrl-D` appears twice**
The EOF note at lines 56–57 duplicates what `scanner-algo.txt` already says. Not wrong, but consider whether both are necessary.

---

### Activity2/assignment.txt

**`<elsePart>` vs. `IfStmtElse` (lines 11–12)**
Line 12 references the rule name `IfStmtElse`, which exists in `if-stmt-1st-try.plcc` (line 23). The usage is accurate. No change needed — just confirming correctness.

---

### Activity3/assignment.txt

**Reflection questions follow the "stop your workspace" instruction (lines 40–53)**
The three thought-provoking discussion questions appear after "This is the last in-workshop exercise. Make sure you stop your workspace before leaving the workshop." Students who follow instructions sequentially may never reach the questions. Moving the questions before the stop instruction would improve engagement.

---

### Activity1/scanner-algo.txt

**Slightly awkward phrasing (lines 53–55)**
```
- The scanner does not actually emit the EOF token in a way that
- the parser can detect it.
+ The scanner does not emit the EOF token in a form the parser can detect.
```

---

### README.md

**Abbreviation "Pres" in schedule (lines 18, 23)**
`10m Pres` appears twice. Expanding to `10m Presentation` (or `10m Pres.` with a period) would be clearer to first-time readers unfamiliar with the abbreviation.

---

### instructional-design-review.txt

**`**=` semantics description (line 89)**
The review describes `**=` as meaning "one or more of `<stmt>`". This should be verified against the PLCC documentation — if `**=` means "zero or more", the description is incorrect and could mislead readers.

---

## Summary Table

| File | Line | Issue | Severity |
|---|---|---|---|
| `PRE-WORKSHOP.md` | 35 | Duplicate step number `5.` — second should be `6.`, third `7.` | Error |
| `PRE-WORKSHOP.md` | 20, 22 | Bare `github.com` URLs missing `https://` scheme | Minor |
| `Activity2/assignment.txt` | 27 | "parantheses" → "parentheses" | Typo |
| `Activity2/assignment.txt` | 49 | "the the" → "the" (duplicate word) | Typo |
| `Activity3/assignment.txt` | 24 | "evalutes" → "evaluates" | Typo |
| `Activity3/assignment.txt` | 48 | "analyis" → "analysis" | Typo |
| `report.md` | 163–174 | "Year / Reference Mismatches" section is factually wrong — both files already say 2026 | Correctness |
| `Activity3/assignment.txt` | 40–53 | Discussion questions buried after "stop your workspace" instruction | Clarity |
| `Activity1/assignment.txt` | 16–18 | Step 3 explanatory paragraph visually detached | Clarity |
| `README.md` | 18, 23 | "Pres" unexpanded | Clarity (minor) |
| `Activity1/scanner-algo.txt` | 53–55 | Slightly awkward phrasing | Style |
| `instructional-design-review.txt` | 89 | `**=` described as "one or more" — verify against PLCC docs | Verify |
