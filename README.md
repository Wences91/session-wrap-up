# Session wrap-up

A Claude skill for people who write their own analysis code and tend to skip the chores around it. When you finish a work session, Claude reviews what changed, tidies your notes, updates the README and the requirements files, and syncs the project with GitHub.

It never writes or changes your code. In code files it only fixes spelling in comments, and only after you have seen the fix.

## What it does

- Summarises the changes of the session and asks only about what it does not understand
- Keeps a private `notes.md` (never pushed) with pending tasks, critical warnings, learning tips from your own code and a short log of the last session
- Updates `README.md` briefly
- Records the packages your code uses, with versions, in `requirements.txt` (Python) and `renv.lock` (R)
- Asks once, for one repository or for all your projects, whether to add an AI statement to the README and whether to credit Claude as co-author in commits. The answer is kept in the local git config
- Prepares the commit, waits for your approval and pushes. Never force pushes
- Warns about cloud sync conflicts (OneDrive, Dropbox, iCloud, Google Drive) and checks the integrity of `.git`

## Installation

In the Claude apps, compress the `session-wrap-up` folder as a ZIP (the folder must be the root of the ZIP). Then go to **Customize > Skills**, click **+**, choose **Create skill** and then **Upload a skill**.

In Claude Code, copy the `session-wrap-up` folder to `~/.claude/skills/`.

## Usage

When you finish working on a project, tell Claude something like "wrap up this session" or type `/session-wrap-up`. Claude needs access to the project folder.

## License

MIT

## AI statement

This skill was drafted with Claude (Anthropic) and reviewed by the author.
