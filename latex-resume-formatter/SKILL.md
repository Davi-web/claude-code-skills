---
name: latex-resume-formatter
description: >
  Edit a LaTeX resume to match a job description. Use this skill whenever
  the user says things like "tailor my resume", "update my resume for this job",
  "help me change my resume to the job description", or shares a .tex resume file
  alongside a job posting. Triggers on any combination of: resume + job description,
  .tex file + role/company, or requests to rewrite/improve resume bullets.
---

# LaTeX Resume Editor

Args: arg1: .tex file, arg2: string -> output: .pdf file in Downloads folder

## Use Case
Tailor a LaTeX resume to a specific job description — rewriting and tightening bullet points to match the role while preserving personal information and document structure.

## Inputs Required
- A `.tex` resume file
- A job description (text, URL, or pasted content)

## Rules
- **Never modify** personal information (name, email, phone, links, address)
- **Never invent** metrics or achievements — only sharpen what's already there
- **Only shorten** bullets that are clearly over 2 rendered lines; leave borderline ones alone
- **Preserve** all LaTeX commands, custom macros, and document structure
- **Never add** tools or skills the candidate has no apparent experience with

## Current skills I know
Languages: Java, Python, TypeScript, SQL, C++
Databases: MySQL, PostgreSQL, MongoDB
Frameworks: Spring Boot, React, Redux-Saga, Material-UI, Next.js, Express, FastAPI, Tailwind CSS
Other: Git, Elasticsearch, Supabase, AWS(EC2, S3, Lambda), Docker, Jenkins, Postman, Claude Code

## Steps

### 1. Read the Files
- Read the `.tex` file provided as arg1
- Copy it to a temp file: `cp <arg1> <dir>/resume_temp.tex` (same directory as the original)
- All edits are made to `resume_temp.tex` — never modify the original `.tex` file
- Read the job description

### 2. Analyze
- Identify all `\item` bullets across each role/section
- Extract key requirements, skills, and keywords from the job description

### 3. Edit Each Bullet
Apply all of the following:
- **Tailor**: mirror keywords and language from the job description where truthful. Bold keywords if they are not already.
- **Shorten**: trim bullets clearly over 2 lines (remove filler, combine redundant clauses)
- **Strengthen**: start with a strong past-tense action verb; use the format *Action + What + Result*
- **Reorder**: move the most relevant bullets to the top of each role
- **Clean up**: consistent punctuation (periods or no periods — pick one), consistent past tense for past roles, fix double spaces

### 4. Update Technical Skills Section
- **Reorder** language/tool listings to front-load what the JD emphasizes (e.g., if JD requires C++, list it first)
- **Surface** skills the candidate already has that match the JD but weren't prominent (e.g., Docker, Bash)
- **Add** JD keywords the candidate demonstrably knows but didn't list (infer from their experience/projects). You can add or remove keywords based on the JD.
- **Remove or deprioritize** tools clearly irrelevant to the role (move to end or omit)


### 5. Output
- Compile `resume_temp.tex` with tectonic: `tectonic resume_temp.tex`
- Rename the output: `mv <dir>/resume_temp.pdf ~/Downloads/David_Ha_Resume.pdf`
- Delete the temp file: `rm <dir>/resume_temp.tex`
- Summarize changes: bullets edited, keywords added, skills reordered

### Learn
- Everytime we run this skill. learn from each error message we encountered and what the solution was and add it to the STEPS section so we know next time how to solve it.

### Known Errors & Fixes

**Error:** `glyphtounicode:7: Undefined control sequence` / `halted on potentially-recoverable error`
- **Cause:** Tectonic uses XeTeX, not pdflatex. `\input{glyphtounicode}` and `\pdfgentounicode=1` are pdflatex-only primitives that crash under XeTeX.
- **Fix:** Before running `tectonic`, comment out both lines in the `.tex` file:
  ```
  % \input{glyphtounicode}
  % \pdfgentounicode=1
  ```

**Note:** `pdflatex` is not installed by default on macOS. If the user doesn't have it, use `tectonic` (install via `brew install tectonic`) with the fix above.