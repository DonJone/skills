---
name: texpdf
description: >
  Advanced LaTeX typesetting skill for academic PDFs with macOS
  environment expertise. Covers XeLaTeX + ctex for Chinese documents,
  page density control (widow/orphan penalties, line spread tuning),
  section numbering best practices, MacTeX installation pitfalls,
  and Python matplotlib chart styling for consulting-grade visuals.
  Use when the user asks to: (1) create academic reports or theses in
  Chinese/English, (2) troubleshoot XeLaTeX compilation issues on
  macOS, (3) optimize page density and avoid widows/orphans, (4) design
  high-end Python charts for LaTeX documents, (5) set up or fix the
  MacTeX LaTeX environment on macOS.
---

# Texpdf Skill — Advanced LaTeX Academic PDFs on macOS

Reference knowledge for long-form LaTeX academic document typesetting and macOS environment configuration. Provides battle-tested lessons for automated typesetting tasks focused entirely on high-quality PDF reports.

## 1. System & Environment

### 1.1 Hardware / OS
- **OS**: macOS (ARM64, Apple Silicon)
- **Shell**: Zsh
- **Package manager**: Homebrew (`brew`)

### 1.2 LaTeX Environment
- **Current setup**: MacTeX full distribution (installed via `brew install --cask mactex`).
- **Historical pitfall**: A prior `basictex` installation caused severe package conflicts and missing macros — especially `ctex` for Chinese support. The correct sequence is: uninstall basictex first (`brew uninstall --cask basictex`), then install the full `mactex`.
- **Sudo trap**: The MacTeX `.pkg` is ~6.9 GB and requires admin privileges (`sudo`) to unpack. AI agents cannot type invisible passwords in the terminal. When a `sudo` install is needed, hand it off to the user and wait 5–15 minutes (the installer gives no progress bar).
- **PATH refresh**: After installing MacTeX, `xelatex` is not immediately available. Before compiling, run `eval "$(/usr/libexec/path_helper)"` to reload the environment, or `xelatex` will fail with `command not found`.

### 1.3 Data Analysis & Charting
- **Python**: Python 3
- **Core libraries**: `pandas` (data processing), `matplotlib` (high-end data visualization).
- **Charting aesthetic**: Do not use matplotlib's default bare style. Adopt a "top-tier consulting / investment banking" look:
  - Remove top and right spines
  - Use minimal light gridlines (`grid`)
  - Define primary and secondary brand colors
  - Add area gradient fills
  - Increase font sizes and data labels

## 2. Core Typesetting Lessons

Battle-tested LaTeX lessons from delivering dense 18+ page academic reports:

### 2.1 Page Density & Widow/Orphan Control
Avoid padding page count with `\newpage` — this creates large white gaps and single-line orphan pages. Lock down these values in the preamble:
```latex
\clubpenalty=10000
\widowpenalty=10000
\displaywidowpenalty=10000
```
**Golden line spacing**: Avoid `\doublespacing` (too loose). Use `\linespread{1.45}` or `\linespread{1.5}` combined with `12pt` font and `2.9cm` margins. This maintains an academic journal feel while maximizing text density. The only correct way to fill more pages is to add substantive deep-dive analysis, not to stretch spacing artificially.

### 2.2 Section Numbering — Don't Double-Prefix
LaTeX has a built-in automatic section numbering system (1, 1.1, 1.1.1).
- **Wrong**: `\section{一、 Company Overview}` or `\subsection{2.1 Macro Analysis}` — this produces ridiculous output like `1 一、 Company Overview` and `2.1 2.1 Macro Analysis`.
- **Right**: Just write `\section{Company Overview}` — LaTeX handles the numbering automatically.

### 2.3 Chinese Compilation Engine
When the document contains Chinese characters, you **must** use the `xelatex` engine and declare `\usepackage[UTF8]{ctex}` in the preamble. Never use `pdflatex` for Chinese documents — it will produce either garbled text or a compilation failure.

## Workflow

When the user asks for LaTeX typesetting:
1. Verify the LaTeX engine: `xelatex` for Chinese content, `pdflatex` or `xelatex` for English-only.
2. Apply the typesetting rules from Section 2: widow/orphan penalties, proper line spread, and clean section titles.
3. If charts are needed, generate them with Python using the consulting-grade matplotlib style.
4. Compile and verify the PDF output.
