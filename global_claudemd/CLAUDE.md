# CLAUDE.md

##. General
never edit or create files outside the current working directory or its subdirectories — if a change seems to require it, stop and ask
don't change the current working directory — run commands with absolute paths or subshells instead of `cd`
when I refer to "the app" (or "this app", "the application"), I almost always mean the app built by the current repo — not Claude Code, the Claude desktop app, or any Anthropic product. Investigate the repo first before asking which app I mean.
the bilby logo is available at `/Users/samd/.claude/assets/bilbylogo.png` — use this path whenever you need the logo
bilby colors (sampled from the logo): red `#892522`, black `#171616`, white `#f8f8f8`

don't use em-dashes

When explaining things in the chat please write in ASD-STE100. 

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Coding guidelines

- use uv pip to install python packages not alone pip, unless uv pip doesn't work
- don't use classes in your code
- Never put code blocks (Python, bash or other) in markdown cells. All code goes in code cells; markdown cells contain prose only.
- when asked about a function in `bilbystats`, read the source (locate via `python3 -c "import bilbystats; print(bilbystats.__file__)"` then grep for `def <name>`) rather than guessing or asking the user for the signature
- MATLAB is installed at `/Applications/MATLAB_R2024b.app` (not on PATH). Run scripts headless with `/Applications/MATLAB_R2024b.app/bin/matlab -batch "..."`.

## 3. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 5. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

## 6. Communication

**Say what's needed. Nothing more.**
 - Default to two paragraphs or fewer. Prose only — code blocks, diffs, file contents, and command output don't count toward this. If a fuller explanation is genuinely needed, say so in one line and offer to expand rather than expanding unprompted.
- Skip preamble and trailing summaries — I can read the diff.
- Brief status updates at key moments (found something, changed direction, hit a blocker). Don't narrate deliberation.
- For open-ended questions, give a recommendation plus the main tradeoff in 2–3 sentences. Don't implement until I agree.
- If asked about previous connservations consult the conversation-log.json file in the current repo if it is available. 
- Use British English. 


## 7. Git

- When asked to commit, write real, descriptive commit messages.
- Keep them concise — a clear summary of the change, not exhaustive.
- Don't include Claude as an author of the change — just the user.
- Don't create a new branch unless asked to by the user.


## 8. Maths
In chat responses, write math as inline Unicode glyphs rather than LaTeX
(− ∑ᵢ pᵢ log pᵢ, not $-\\sum_i p_i \\log p_i$). Use fenced code blocks with
manual alignment for matrices. Avoid combining diacritics — write "x̄" as
"mean(x)" or "x_bar". This applies to chat only: in files, use whatever
markup the file format calls for.

