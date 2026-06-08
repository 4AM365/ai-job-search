# Resume Templates and Tailoring Guide

<!-- SETUP: Profile statements and section ordering are personalized by running /setup -->

## Template: LaTeX moderncv (Banking Style)

All resumes use the moderncv LaTeX package with the "banking" style and "blue" color scheme, on **US Letter** paper (`letterpaper`).

**Output file:** `cv/main_<company>.tex`
**Compile with:** **lualatex** on MiKTeX/TeX Live. pdflatex often fails on modern MiKTeX installs with `fontawesome5` font-expansion errors; lualatex handles the same sources cleanly.
**Master reference:** `cv/main_example.tex` (comprehensive resume with all competencies, experience, and achievements - use as source when building targeted resumes)

### Compile command

```bash
cd cv && lualatex -interaction=nonstopmode main_<company>.tex
```

Expected output: `Output written on main_<company>.pdf (N pages, ...)` where **N is 1 or 2** (see "Page Budget" below). Anything more than 2 pages is a failure that must be fixed before presenting to the user.

## Document Structure

```latex
\documentclass[11pt,letterpaper,sans]{moderncv}
\moderncvstyle{banking}
\moderncvcolor{blue}

% Force both first and last name AND section headings to render in moderncv
% blue (color1). Default banking on lualatex+MiKTeX leaves these black, which
% looks inconsistent with the rest of the blue accent scheme.
\renewcommand*{\firstnamestyle}[1]{{\fontsize{34}{36}\bfseries\upshape\color{color1}#1}}
\renewcommand*{\lastnamestyle}[1]{{\fontsize{34}{36}\bfseries\upshape\color{color1}#1}}
\renewcommand*{\sectionstyle}[1]{{\sectionfont\color{color1}#1}}

\usepackage[utf8]{inputenc}
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,
    urlcolor=blue,
    pdftitle={[YOUR_NAME] - Resume},
    pdfpagemode=FullScreen,
}
\usepackage[scale=0.77]{geometry}
\usepackage{import}

% Personal data (US convention: city + state only, no street address)
\name{[FIRST_NAME]}{[LAST_NAME]}
\address{[CITY, ST]}{}{}
\phone[mobile]{[YOUR_PHONE]}
\email{[YOUR_EMAIL]}
\extrainfo{\href{[YOUR_LINKEDIN_URL]}{LinkedIn}, \href{[YOUR_GITHUB_URL]}{GitHub}}

\begin{document}
\makecvtitle

% 1. Profile statement (1-3 sentences, tailored per role)
% 2. Skills section
% 3. Education section
% 4. Professional Experience section
% 5. Selected Publications (if applicable)
% 6. Honors and Awards (if applicable)
% (US resumes omit a References section - do not add one)

\end{document}
```

### Color overrides

The three `\renewcommand*` lines in the preamble are required on lualatex+MiKTeX. Without them the firstname, lastname, and section headings render in black even though `\moderncvcolor{blue}` is set, which looks inconsistent with the rest of the blue accent scheme (links, bullet markers, contact icons). The override forces all three to use `color1` (moderncv's accent colour, which becomes blue under `\moderncvcolor{blue}`). Both names render bold; if you prefer the firstname in regular weight, change the firstnamestyle override from `\bfseries` to `\mdseries`. Don't drop the override - on most modern installs the defaults render visibly wrong.

### Spacing inside itemize lists (important)

**Do not place `\vspace{...}` between `\item` entries in an `itemize` list.** Even though the source looks symmetric, this pattern occasionally produces a noticeably oversized gap before a single item: the inter-item `\vspace` creates a paragraph break that interacts unpredictably with the list's internal `\itemsep`, so LaTeX renders one of the gaps wider than the rest. Remove the inter-item `\vspace` and let `itemize` use its native uniform spacing.

```latex
% WRONG - intermittently produces an oversized gap before one bullet
\begin{itemize}
\item \textbf{Foo}: ...
\vspace{1pt}
\item \textbf{Bar}: ...
\vspace{1pt}
\item \textbf{Baz}: ...
\end{itemize}

% RIGHT - uniform spacing using the list's native itemsep
\begin{itemize}
\item \textbf{Foo}: ...
\item \textbf{Bar}: ...
\item \textbf{Baz}: ...
\end{itemize}
```

Two related patterns are fine and should be kept:
- `\vspace{1pt}` immediately after `\section{...}` (between section heading and first item) - this is between the heading and the list, not between list items.
- `\vspace{3pt}` between top-level `\cventry` blocks in Professional Experience or Education - this gives breathing room between roles and renders consistently.

## Section-by-Section Tailoring

### Profile Statement / Elevator Pitch (Best Practice)
This is the most important section to customize. It appears right after `\makecvtitle`.

Write 5-7 lines that function as an "elevator pitch": a concise, compelling introduction explaining why you're qualified for *this specific role*. Focus on what the employer gains from hiring you.

**Create 2-3 profile statement templates for your main role types:**

<!-- SETUP: These are populated based on your background -->
**For [YOUR_PRIMARY_ROLE_TYPE] roles:**
> [YOUR_PROFILE_STATEMENT_TEMPLATE_1]

**For [YOUR_SECONDARY_ROLE_TYPE] roles:**
> [YOUR_PROFILE_STATEMENT_TEMPLATE_2]

### Core Competencies / Skills Section (Best Practice)
Reorder and emphasize based on the role. Use bold category labels.

List **5-7 key competencies** in bullet format, tailored to the specific job. For each competency, briefly explain how it adds value to the position.

### Education
- Always include your highest degrees
- For senior roles, keep education brief (dates and titles only)
- Include thesis topics when relevant to the target role

### Professional Experience
- Rewrite bullet points to emphasize aspects most relevant to the target role
- Use 4-6 bullets for most recent role, 3-4 for previous, 2-3 for older
- **Emphasize measurable results** where possible: "Reduced processing time by X%", "Model adopted by the team"

### Handling Employment Gaps (Best Practice)
If there is a gap in your employment history:
- The gap should be explained matter-of-factly if needed
- Describe how professional development continued during the gap
- Frame as deliberate skill-building and career repositioning

### Publications
- Include Google Scholar link if applicable
- Select 3-4 most relevant publications (not always all of them)
- For non-academic roles, keep brief

### Honors and Awards
- Keep format brief, one line each

### References (US convention: omit)
- **US resumes do not include a References section.** Do not list references and do not add "References available upon request" - it wastes a line and is assumed.
- If an employer explicitly asks, provide 3 references on a separate sheet (name, title, company, email, phone), not on the resume.

## ATS-Friendliness (US market)

Most US employers run resumes through an Applicant Tracking System (ATS) that parses the PDF into plain text before a human ever sees it. Keep the moderncv template, but write so the parse stays clean:

- **Mirror the posting's keywords.** If the job says "Python, AWS, Kubernetes," use those exact terms (not just "cloud" or "containers") somewhere the parser will read them - ideally in Skills and in an experience bullet.
- **Use standard section headings:** "Experience" / "Professional Experience", "Education", "Skills" / "Core Competencies", "Publications". ATS parsers key off these; avoid cute or non-standard headings.
- **Keep critical info as real text, never inside an icon or graphic.** moderncv renders contact details as text (good); do not move email/phone into image badges.
- **No multi-column layouts for parsed content** and no tables to hold experience or skills - parsers read them out of order. moderncv's single-column banking style is already safe; keep it.
- **Spell out then abbreviate** the first time for important terms ("Machine Learning (ML)") so both forms are searchable.
- **Standard fonts and bullet characters only** (the template's defaults are fine). Avoid special glyphs as bullets.

## Compile-and-Inspect Loop (MANDATORY)

After writing the resume and before presenting to the user, always compile and visually inspect the PDF. Iterate until the layout is clean. Workflow:

1. Run `lualatex -interaction=nonstopmode main_<company>.tex`
2. Check the output page count: must match the page budget (1 page default, up to 2 for senior/extensive profiles - see "Page Budget" below). Never more than 2.
3. Read the PDF via the Read tool and visually inspect every page
4. Check for **orphaned entries**: a `\cventry` title line must never sit alone at the bottom of a page with its bullets on the next page

### Fixing common page-break problems

**Problem: entry title at the bottom of a page, bullets orphaned to the next page**
Add `\needspace{5\baselineskip}` immediately before the problematic `\cventry`:
```latex
\needspace{5\baselineskip}
\item{\cventry{YEAR--YEAR}{Role Title}{Organization}{Location}{}{...}}
```
Include `\usepackage{needspace}` in the preamble.

**Problem: one trailing section spills just past the page budget (e.g., Honors alone on a new page)**
Add `\enlargethispage{2-3\baselineskip}` before a late section (e.g., before `\section{Honors and Awards}`) to stretch the last page by a few lines. This is the standard LaTeX rescue for near-miss overflows.

**Problem: significant content overflows past the page budget**
Cut content — do not compress geometry or `\vspace`. See "Relevance-weighted cutting" below for the rule.

**Problem (2-page budget only): a thin, half-empty second page**
Either tighten to a clean single page (preferred), or restore the highest-relevance item that was previously cut. A resume that ends a few lines into a 2nd page looks incomplete - a full single page is better.

## Page Budget - Adaptive 1-2 Pages (US convention)

US resumes are **1 page by default**. Use 2 pages **only** when the candidate's record genuinely fills it - typically a senior professional (roughly 10+ years), an academic/PhD profile with publications, or someone with many directly relevant roles. When in doubt, target 1 page. Never exceed 2 pages, and never leave a 2nd page that is less than ~half full - tighten to a clean single page instead.

**Decide the budget first** (1 vs 2 pages) based on the profile and the target role, then fit content to it:

| Section | 1-page budget | 2-page budget |
|---------|---------------|---------------|
| Profile / summary | 2-3 lines | 3-4 lines |
| Skills | 4-5 items, 1 line each | 5-7 items, 1-2 lines |
| Most recent role | 3-4 bullets | 4-5 bullets |
| Previous role | 2-3 bullets | 2-3 bullets |
| Older roles | 1-2 bullets | 2 bullets |
| Education | titles + dates only | 2-3 entries |
| Publications | top 1-2 (or omit) | 2-3 entries |
| Awards | 2-3, single line each | 3, single line each |
| References | omit entirely | omit entirely |

**If in doubt, cut rather than squeeze.** Reducing `\vspace` or geometry scale to force-fit content makes the resume look cramped.

## Relevance-weighted cutting (the right way to shrink a resume)

**Cut by signal, not by section.** Static priority lists ("remove oldest education first, then shorten the earliest role...") are wrong when a relevant "lower-priority" item is competing with an irrelevant "higher-priority" item. An older-role bullet that speaks directly to the posting is worth more than a recent-role bullet that does not.

For every candidate line, score three things:

1. **Relevance to THIS posting** — does the line hit a named tool, keyword, or stated responsibility in the job ad?
2. **Uniqueness** — is it the only place this claim appears, or is it duplicated elsewhere in the resume?
3. **Narrative load** — does the cover letter depend on it? If cutting the line would force you to rewrite a cover-letter paragraph, it is load-bearing.

Cut the lowest-total-score line first, regardless of which section it sits in.

### Practical order of cuts (easiest → last resort)

1. **Redundancy.** If an achievement appears in both Core Competencies AND a role bullet, the Core Competencies version is usually the cleaner cut (the experience bullet is more concrete evidence).
2. **Profile-statement fluff.** A sentence that just restates what Publications or Skills will show. ("Peer-reviewed publications on X..." is already a Publications entry — profile can claim it once and stop.)
3. **Low-relevance experience bullets.** A bullet about work that does not touch posting keywords, wherever it sits. This cuts across sections before touching the structural list.
4. **Low-relevance supporting content.** An older-role bullet that does not speak to the target role. A certification that does not touch the posting's stack. A language entry that can be condensed to one line.
5. **Low-relevance publications.** Keep 1-2 publications that best match the posting. Cut the rest before touching experience bullets.
6. **Last-resort structural cuts.** Oldest education entry, tightening an older role to 2 bullets, collapsing Certifications into a single line. These only happen if the relevance-weighted cuts above have already been exhausted.

### Pitfalls to avoid

- Do not mechanically cut from the bottom of a static section list without checking relevance. "Cut the oldest role first" is wrong if that role is literally about the skill the posting asks for.
- Do not cut the one concrete example the cover letter leans on. Relevance is measured against the cover letter you wrote, not just the job posting — interviewers will have read both.
- Do not cut to fit if the fit is borderline (2.02 pages). Prefer `\enlargethispage{2-3\baselineskip}` on a late section for near-misses; reserve content cuts for genuine overflow (content on page 3 that is more than a single trailing section).

## Recommended Section Order

The section order varies by role type:

**For technical / data science / ML roles:**
1. Profile statement / elevator pitch
2. Core competencies / Skills
3. Professional Experience (reverse chronological)
4. Education (reverse chronological)
5. Publications & Awards (if applicable)

**For domain-specific / specialist roles:**
1. Profile statement / elevator pitch
2. Core competencies / Skills
3. Education (reverse chronological) - credentials are a key qualifier
4. Professional Experience (reverse chronological)
5. Publications & Awards (if applicable)

(US resumes omit a References section in all orderings. Languages belong on the resume only if relevant to the role - e.g. a bilingual-required posting.)
