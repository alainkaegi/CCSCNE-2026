# CCSCNE-2026 Workshop Environment Test Report

**Date:** 2026-03-14
**Tester:** Claude Code (claude-sonnet-4-6)
**Branch:** claude-test

---

## Overview

All four activities were attempted in sequence. Activities 1–3 were completed successfully without consulting the solutions. Activity 4 had no `assignment.txt` and appears to be a work-in-progress. The environment (Codespace, `plccmk`, `scan`, `parse`, `rep`) functioned correctly throughout.

---

## Activity 1 — Lexical Analysis

### Task

Extend `idents.plcc` to add `INTEGER`, `IF`, and `ELSE` tokens.

### Approach

- Added `token IF 'if'` and `token ELSE 'else'` before `IDENT` so the "first longest match" rule correctly handles reserved words (e.g., `if` → `IF`, `ifato` → `IDENT`).
- Added `token INTEGER '[0-9]+'` after `IDENT`.

### Result

Build succeeded. `scan < testprog.txt` produced correct output:

- Reserved words `if`/`else` recognized as `IF`/`ELSE`
- Words like `ifato`, `elseanor` correctly recognized as `IDENT`
- Integers recognized; `49.68` correctly split into `INTEGER '49'`, `!ERROR(".")`, `INTEGER '68'`

### Comparison with Solution

Two minor differences, no functional impact:

1. **Token order:** Solution places `INTEGER` before `IF`/`ELSE`; mine places it after. Since digits and letters are disjoint character classes, order doesn't matter.
2. **Regex style:** Solution uses `\d+`; mine uses `[0-9]+`. Equivalent in Java regex.

**Verdict: Correct and essentially identical to solution.**

---

## Activity 2 — Syntactic Analysis (Dangling Else)

### Task

Fix the ambiguous `if`-statement grammar in `if-stmt.plcc` by requiring all `if` statements to end with a new `endif` keyword.

### Approach

- Added `token ENDIF 'endif'` before `IDENT`.
- Changed `<stmt>IfStmt ::= IF <expr> THEN <stmt> <elsePart>` to append `ENDIF`.

### Result

`plccmk -c if-stmt.plcc` reported no LL(1) problems. `parse < testprog.txt` returned `OK` for all test cases including nested `if`/`else` constructs.

### Comparison with Solution

**Identical.** Same token, same grammar rule change.

**Verdict: Correct and identical to solution.**

---

## Activity 3 — Semantic Analysis (Prefix Evaluator)

### Task

1. Implement `evaluate()` for `Sub`, `Mul`, and `Div` classes.
2. Add unary negation operator `_` with token, grammar rule, and semantic method.
3. Replace the dummy `evaluate()` in `Expr` with an `abstract` declaration.

### Approach

- Added `token NEGOP '_'`, grammar rule `<expr>Neg ::= NEGOP <expr>opnd`.
- Replaced `return 999` dummy with `public abstract int evaluate();`.
- Added `@Override` `evaluate()` methods for `Sub`, `Mul`, `Div`, and `Neg`.

### Result

Both test files produced correct output:

| Expression | Expected | Got |
|---|---|---|
| `- 2 1` | 1 | 1 |
| `* / 45 5 - 3 10` | -63 | -63 |
| `_3` | -3 | -3 |
| `_ * + 12 6 5` | -90 | -90 |
| `* / 45 5 _ - 3 10` | 63 | 63 |

### Comparison with Solution

Minor cosmetic differences only:

| Aspect | My Solution | Official Solution |
|---|---|---|
| Token name | `NEGOP '_'` | `UMIOP '_'` |
| Class name | `Neg` | `Umi` |
| Unary field | `opnd` | `opnd1` |
| Old dummy code | Replaced inline | Commented out, abstract added below |
| `@Override` | Added | Omitted |

**Bug found in official solution:** `Sub.evaluate()` uses `+` instead of `-`:

```java
// Sol/Activity3-sol/prefix.plcc, line 89 — INCORRECT
return opnd1.evaluate() + opnd2.evaluate();
// Should be:
return opnd1.evaluate() - opnd2.evaluate();
```

This would cause all subtraction expressions to silently return wrong results (e.g., `- 2 1` returns `3` instead of `1`).

**Verdict: My solution correct; official solution has a bug in `Sub`.**

---

## Activity 4 — Postfix Evaluator

### Status: No assignment found

`Activity4/` contains `postfix.plcc`, `postfixx.plcc`, and `testprog.txt` but **no `assignment.txt`**. The activity appears incomplete.

### What was observed

- **`postfix.plcc`:** A working postfix evaluator for binary operators using an iterator-based approach.
- **`postfixx.plcc`:** An extended version that adds unary negation `_`, using an abstract `Op12` class with `Op1` (unary) and `Op2` (binary) subclasses — a pattern that mirrors Activity 3.
- Both files build cleanly and produce correct results against `testprog.txt`.

### Test Results (`testprog.txt`)

| Expression | Expected | Got |
|---|---|---|
| `2.` | 2 | 2 |
| `2 3 +.` | 5 | 5 |
| `2 3 + 4 -.` | 1 | 1 |
| `2 3 4 + -.` | -5 | -5 |

**Verdict: Environment works; activity needs an `assignment.txt`.**

---

## Issues Found

### Bugs

| Location | Issue |
|---|---|
| `Sol/Activity3-sol/prefix.plcc` line 89 | `Sub.evaluate()` uses `+` instead of `-` — wrong result for all subtraction expressions |

### Typos

| Location | Issue |
|---|---|
| `Activity2/assignment.txt` line 49 | "the the" — duplicate word |
| `Activity2/assignment.txt` line 27 | "parantheses" → "parentheses" |
| `Activity3/assignment.txt` line 24 | "evalutes" → "evaluates" |
| `Activity3/assignment.txt` line 48 | "analyis" → "analysis" |

### Year / Reference Mismatches

| Location | Issue |
|---|---|
| `README.md` line 1 | Title says "CCSCNE 2025" — should be 2026 |
| `PRE-WORKSHOP.md` lines 24/26 | References `CCSCNE-2025` repo URL — should be 2026 |

### Numbering Error

| Location | Issue |
|---|---|
| `PRE-WORKSHOP.md` | Two steps both numbered `5.` — second should be `6.`, shifting the current `6.` to `7.` |

### Clarity Issues

| Location | Issue |
|---|---|
| `Activity1/assignment.txt` | Step 3's explanatory paragraph is visually detached — consider indenting it as a continuation of the step |
| `Activity2/assignment.txt` line 12 | References nonterminal `'IfStmtElse'` — verify this matches the actual rule name in `if-stmt-1st-try.plcc` |
| `Activity3/assignment.txt` lines 62–63 | Third discussion question about `"--- 2 7"` would benefit from clarifying what `--` would mean as a token/operator |

### Missing Content

| Location | Issue |
|---|---|
| `Activity4/` | No `assignment.txt` — activity is incomplete |

---

## Environment Assessment

The Codespace environment worked flawlessly. All tools (`plccmk`, `scan`, `parse`, `rep`) were available and functional. Build times were fast and error messages were clear. The progression from Activity 1 (scanning) → Activity 2 (parsing) → Activity 3 (semantics) is well-structured and pedagogically sound. Activity 4 looks like a promising extension exercise once the assignment is written.
