---
name: session-wrap-up
description: Use when the user finishes a work session on a code project, to review changes, tidy notes, update README and requirements, and sync with GitHub without touching the code.
---

# Session wrap-up

For people who write their own analysis code (R, Python) and want help with the chores around it. Notes, documentation, requirements and version control. The code stays theirs. This skill never writes or changes it.

It runs when the user says they are done. For example "wrap up", "I'm done with X" or "/session-wrap-up". Talk to the user in their language.

## Hard rules
What you may touch:
- notes.md and README.md are the only files where you add or remove text. Keep it brief
- .gitignore, requirements.txt and renv.lock are technical files. You may create and update them
- In code, you only fix spelling in comments. Nothing else

What you must not do:
- Change code. No logic, formatting or style changes
- Add, remove or rewrite comments
- Create other documentation files
- Apply spelling fixes before showing them to the user
- Add Co-Authored-By or any other Claude signature to commits
- Force push

Critical warnings go in notes.md, never in the code. Quick annotations in comments (row counts, open questions) and commented-out alternative code are normal. Do not flag them.

## Before you start
- Work in the project folder. If you cannot reach it, ask the user to connect it
- If it is not clear which project it is, ask
- Read README.md and notes.md if they exist
- Skim a few scripts to learn the user's conventions. Use them to understand the changes, never to change their style
- If the folder is synced with a cloud service (OneDrive, Dropbox, iCloud, Google Drive), look for conflict copies. They usually carry a device name or the word "conflict". If there are any, warn the user before going on

## 1. Repository status
If there is no repository:
- Offer to initialise one
- Create a .gitignore with notes.md, data/, .DS_Store, .ipynb_checkpoints/, .Rhistory, .RData, .Rproj.user/, .Renviron, __pycache__/ and .env. Adapt it to the project
- Ask before creating the GitHub repository, and whether it should be public or private

If it exists, run `git status`, `git diff --stat` and `git log -5`. If the folder is cloud-synced, also run `git fsck`, because sync clients can damage the .git folder.

notes.md is never pushed. Check that it is in .gitignore. If it was pushed before, remove it with `git rm --cached notes.md`, which keeps the local file.

## 2. Review
- Summarise the changes by intent, not file by file
- Ask only about what you do not understand. Three questions at most
- Find problems, but do not fix them. Critical ones go to Warnings and improvable ones to To learn. Critical examples:
  - Credentials or API keys written in the code
  - Licensed or personal data about to be pushed
  - Absolute paths to the user's machine
  - Files over 50 MB
  - An object used where another one was clearly intended (for example, a copied block that kept the previous name)
  - Results that change with no change in the code

## 3. notes.md
It must be readable in 30 seconds. Always this shape and nothing else, with the headings in the user's language:

```
# Notes

## Pending
- 

## Warnings
- 

## To learn
- 

## Last session (YYYY-MM-DD)
- 
```

- One short line per item, except in To learn. No introductions or explanations
- Pending comes from the user's notes and the TODOs in the code. Remove what is done
- Warnings only if critical. If there are none, leave the section empty
- To learn holds one to three tips per session, taken from the user's own code. Each in one or two lines, with file and line, what to improve and how
- Add a minimal example if it helps, in the user's coding style
- Typical topics:
  - Repeated code that could be a function
  - Deprecated functions, such as `..count..` in ggplot2 (now `after_stat(count)`)
  - Duplicated or unused imports
  - `except` blocks that hide errors
  - Loops that could be vectorised
- Do not repeat tips that are already there. Remove the ones the user has applied. Five at most at any time
- The tips are for the user to learn. Never apply them to the code
- Last session is rewritten every time with one to three items. The history is in git
- Respect what the user's notes say. Do not add decisions or reasons that are not in the notes or the code
- If a note is unclear, ask instead of interpreting it
- Write in the language of the user's notes

## 4. README.md
Brief, in the language the README already uses. Touch only what changed.
- If a new script appears, add a row to the Pipeline table with its purpose in one sentence
- If a script is removed or renamed, update its row
- Mention the requirements files in one line
- Do not invent scientific content. If something is missing, ask
- Always include the AI statement section at the end, with the text below. If it is already there, leave it as it is

If there is no README, create a minimal one with these sections:
- Title
- Description, in two or three sentences
- Repository Structure, with the Pipeline table (script and purpose)
- Data
- License
- AI statement

Authors, Abstract, Citation, Funding and Acknowledgements only if the user provides the text.

AI statement text.

```
## AI statement

Claude (Anthropic) is used for editing and organisation tasks in this repository, not to generate code automatically. It reviews the changes, identifies possible errors and records them, with suggestions, in the author's private notes. It also updates this README and the requirements files, corrects spelling in code comments and prepares commits that the author approves before pushing.
```

## 5. Requirements
Record only the packages the code really uses, with the version installed on the user's machine.

Python in requirements.txt:
- Look for imports in .py files and in the code cells of .ipynb notebooks
- Drop the standard library and map import names to package names with `importlib.metadata.packages_distributions()` (for example, sklearn is scikit-learn)
- Pin versions with `==` and put the Python version as a comment on the first line
- Do not use `pip freeze`, because it lists everything installed
- If there are several environments (conda, venv), ask which one the project uses

R in renv.lock:
- Create it with `renv::lockfile_create(type = 'implicit', project = '.', prompt = FALSE)` and `renv::lockfile_write()`
- Do not activate renv in the project. No `renv::init()`, no .Rprofile and no renv/ folder
- If renv is not installed, ask before installing it

Update them at every wrap-up if there are new packages or versions have changed.

## 6. Commit and push
- Show the list of files going into the commit
- Propose a short, descriptive message in the user's language
- Wait for explicit confirmation before committing and pushing
- Run `git fetch` before pushing. If the remote has changes that are not local, warn the user and stop
- If authentication fails, tell the user to run `gh auth login` on their machine

## 7. Wrap-up
End with three lines at most:
- What was pushed
- What is still pending
- Any critical warning or new tip

## Style
Short sentences. No filler.
