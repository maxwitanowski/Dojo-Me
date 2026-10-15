# Dojo-Me

A Claude Code skill that works out where you are on **the 10 levels of Claude Code**, shows you a personal roadmap to the next one, and can build the first step with you.

Run `/dojo-me` and Claude:

1. **Scans your setup**: CLAUDE.md files, skills, MCP servers, memory, hooks, subagents, automation, and the projects you actually work on.
2. **Asks three quick questions** about what you use Claude Code for.
3. **Places you on the 10 levels**. The levels are guidelines and Claude makes the final call: a missing foundation becomes a gap to fill rather than a demotion, and a one-off doesn't lift you a level.
4. **Opens a dashboard** in your browser with your level, what you have, the gaps worth filling, and the steps to reach the next level.
5. **Offers to build the first step** with you.

| Level | Name | |
|---|---|---|
| 1 | Terminal Tourist | Installed, no setup yet |
| 2 | Grounded | CLAUDE.md, git, plan mode, a clean context habit |
| 3 | Connected | MCP servers, CLIs and a browser |
| 4 | Skilled | Your own skills for repeated tasks |
| 5 | Context Architect | Docs, memory and imports |
| 6 | System Builder | Chained skills, subagents, hooks, approval gates |
| 7 | Pipeline Engineer | Headless runs, CI, the Agent SDK |
| 8 | Multi-Agent Operator | Parallel sessions in worktrees and the cloud |
| 9 | Always On | Scheduled routines and event-triggered agents |
| 10 | Swarm Architect | Orchestrators with strong guardrails |

## Install

Clone it into your Claude Code skills folder. The folder must be called `dojo-me`.

**macOS / Linux**

```bash
git clone https://github.com/maxwitanowski/Dojo-Me.git ~/.claude/skills/dojo-me
```

**Windows (PowerShell)**

```powershell
git clone https://github.com/maxwitanowski/Dojo-Me.git "$env:USERPROFILE\.claude\skills\dojo-me"
```

Then start a new Claude Code session.

To update later:

```bash
git -C ~/.claude/skills/dojo-me pull
```

## Use

```
/dojo-me            # assess, roadmap, then optionally build
/dojo-me --assess   # just show your level
/dojo-me --build    # skip to building the next step
```

Run it again whenever you've levelled up.

## Privacy

The scan reads files on your own machine as part of your normal Claude Code session. Your results are written to `~/Desktop/dojo-me-results.html` on your computer and nowhere else. The dashboard loads its fonts from Google Fonts when you open it.

## What's in here

```
SKILL.md                         the skill: scan, scoring, roadmaps, build steps
references/dojo-me-template.html the dashboard template (no personal data)
references/dojo-mascot.png       sensei artwork
references/dojo-icon.png         logo icon
```

## Credits

Based on the 10 Levels of Claude Code framework by the [GenAI Circle](https://thegenaicircle.com) community.

## License

[MIT](LICENSE)
