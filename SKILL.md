---
name: dojo-me
description: Assess your Claude Code level (1-10) and get a personalized roadmap to the next one
user-invocable: true
---

# Level Up

The 10 Levels of Claude Code. Scan your environment, determine where you actually are, get a focused roadmap to the next level, and optionally build the first step right now.

This is NOT the generic "10 Levels of Claude" (browser, desktop, projects). This is specifically about Claude Code mastery. Level 1 is "just installed it." Level 10 is genuinely rare.

**Usage:**
- `/dojo-me` - Full flow: assess, roadmap, build
- `/dojo-me --assess` - Just show your current level
- `/dojo-me --build` - Skip assessment, jump to building

---

## How It Works

```
Scan Environment → Determine Level → Show Roadmap → Build First Step
       ↓                 ↓                ↓               ↓
   Phase 1           Phase 1          Phase 2          Phase 3
   (automatic)       (+ 3 questions)  (next level)     (optional)
```

Designed to be run repeatedly. Every time you level up, the assessment updates.

---

## The 10 Levels of Claude Code

| Level | Name | What It Means (Plain English) |
|-------|------|-------------------------------|
| 1 | Terminal Tourist | You just installed it. You're typing prompts like it's a chatbot in a terminal. No setup, no config. That's fine, everyone starts here. |
| 2 | Grounded | You have a CLAUDE.md so Claude knows who you are and how you work. You work in git, plan before you build, and clear context between tasks. |
| 3 | Connected | Claude reaches your real tools through MCP servers or CLIs (tickets, docs, repos, chat) and drives a real browser to test its own work and research live sites. You're not copy-pasting context anymore. |
| 4 | Skilled | You've written your own skills. Repeated tasks are a single `/command`, and Claude picks the right skill automatically when a task matches. |
| 5 | Context Architect | You've built a knowledge system: docs, memory, imports and examples. Claude draws on everything you've taught it, and output improves over time. |
| 6 | System Builder | Skills chain together. Subagents work in parallel with their own context. Hooks enforce rules automatically, and approval gates mean nothing ships without your sign-off. |
| 7 | Pipeline Engineer | You run Claude from scripts and CI: headless mode, JSON output, the GitHub Action, the Agent SDK. Claude is a component in your stack, not just a chat window. |
| 8 | Multi-Agent Operator | Several Claude sessions run in parallel, each in its own git worktree or cloud session, with different roles. You coordinate them like a team lead. |
| 9 | Always On | Claude runs whether you're at your desk or not: scheduled routines, `/loop`, event-triggered CI agents, monitoring. It's infrastructure now. |
| 10 | Swarm Architect | Orchestrator agents plan the work, spawn workers, verify results and keep going until the goal is met, inside strong guardrails. Genuinely rare. |

Most users land between Level 2 and 5. Level 6 is strong. Level 8+ is rare.

---

## Phase 1: Assess Your Level

### Step 1.1: Environment Scan

Silently scan the user's environment. Do NOT ask permission for each check. Just read what's available.

**Be thorough.** Config files only tell half the story. Look at the work itself: open the files you find and judge their quality, look at the user's real projects and how actively they're worked on, and look for evidence of how they actually use Claude day to day. If the home directory is large, delegate the broad sweep to a subagent (for example the Explore agent) and keep only its conclusions. Collect specific evidence for every claim you make; you'll quote it back to the user.

**Check all of these:**

```
1. CLAUDE.md (check ALL locations)
   - Check: CLAUDE.md, .claude/CLAUDE.md, CLAUDE.local.md, ~/.claude/CLAUDE.md
   - Does any exist? How many locations are used?
   - How many lines total? (< 10 = minimal, 10-50 = basic, 50-150 = solid, 150+ = advanced)
   - Does it use @imports or point to docs/memory files?
   - Does it define what "done" means (tests, lint, review)?

2. Skills / Commands / Plugins
   - Check .claude/skills/*/SKILL.md and ~/.claude/skills/*/SKILL.md (modern format)
   - Check .claude/commands/*.md and ~/.claude/commands/*.md (legacy slash commands)
   - Check installed plugins (~/.claude/plugins/) and any plugin the user authored
   - Count total self-built skills (installed third-party skills don't count towards Level 4)
   - Read 2-3 to check complexity:
     * Simple (< 20 lines, single prompt) = Level 4
     * Medium (20-80 lines, multi-step, bundled scripts/references) = Level 4-5
     * Complex (80+ lines, phases, approval gates, tool calls) = Level 6+
     * Skills that call other skills or orchestrate subagents = Level 6-7

3. MCP Configuration
   - Check .mcp.json in current directory (project scope)
   - Check ~/.claude.json for mcpServers (user scope) and per-project entries
   - Optionally check managed config if enterprise
   - Count configured MCP servers across all locations
   - Note CLIs Claude is told to use in CLAUDE.md (gh, aws, gcloud, etc.): these count as connections too
   - Categorize: data (docs, drives), comms (chat, email), dev (GitHub, CI), browser (Playwright, Chrome), specialty
   - Browser access counts here too: Playwright MCP, Claude in Chrome, Playwright/Puppeteer in package.json, screenshot or PDF scripts

4. Memory / Context Structure
   - Check ALL of these (any one counts):
     * `memory/` directory (with subdirs like patterns/, customers/, examples/)
     * `memory.md` file (single-file knowledge base)
     * `docs/` directory with structured knowledge (PRD.md, architecture.md, conventions.md, ADRs, feature specs)
     * Claude Code auto memory in ~/.claude/projects/*/memory/
   - How deep? (flat files = Level 5, nested dirs with patterns/examples = Level 5-6)
   - Does the CLAUDE.md reference, import or route to these knowledge files?

5. Quality Gate Signals (Level 6+)
   - Hook configurations in .claude/settings.json or ~/.claude/settings.json (PreToolUse, PostToolUse, Stop, etc.)
   - Subagent definitions in .claude/agents/ or ~/.claude/agents/
   - Hooks are quality gates, not infrastructure. They belong here, not at Level 9.

6. Automation Signals (Level 7+)
   - Shell scripts or package.json scripts that call `claude -p` / `claude --print`
   - `--output-format json` or `stream-json` usage
   - .github/workflows/ using the Claude Code GitHub Action
   - Code importing the Claude Agent SDK

7. Multi-Agent Signals (Level 8+)
   - Git worktrees in use (`git worktree list` shows more than one)
   - tmux config or scripts launching multiple Claude sessions
   - Multiple agent role definitions; evidence of cloud/background sessions
   - VPS or remote deployment evidence

8. Always-On Signals (Level 9-10)
   - Scheduled routines (/schedule), cron jobs, launchd or systemd services calling Claude
   - Scheduled or event-triggered GitHub Actions running Claude
   - Background agents running 24/7 (pm2, systemd)
   - Autonomous loop scripts (spec + test suite patterns), workflow scripts
   - Orchestration code where agents spawn and manage other agents

9. Real Work and Craft (informs every level)
   - Projects across the machine: look for repos and project folders in ~/code, ~/Projects, ~/dev, ~/src, ~/Desktop and ~/Documents (a few levels deep)
   - Activity: `git log` frequency and recency, number of active repos, branches and pull requests
   - Sophistication: read READMEs, package files and a sample of source. Are there tests, CI, a clear architecture? Is it a toy or a real product?
   - How Claude is used: number and recency of sessions in ~/.claude/projects/, which projects they're in, and whether skills, subagents or MCP tools appear in that work
   - Quality of what they've built with Claude: is a skill a one-line prompt or a well-structured workflow? Is a "pipeline" a real script or a one-off command?
```

### Step 1.2: Context Questions

Ask the user these 3 questions using the `AskUserQuestion` tool so they appear as interactive prompts (not plain text). All 3 questions should be asked in a single `AskUserQuestion` call with `multiSelect: false` on each. Provide 3-4 options per question plus the automatic "Other" option for free-text input.

1. **What do you mainly use Claude Code for?** — Options: Coding, Content creation, Research, Automation (+ Other)
2. **What's your biggest friction right now?** — Options: Output quality, Context limits, Don't know what's possible, Speed (+ Other)
3. **If you could automate one thing you do repeatedly, what would it be?** — Options: Deployment, Code review, Research & reports, Testing (+ Other)

### Step 1.3: Determine Level

The levels are **guidelines, not a ladder of gates**. You make the final call on where the user sits, based on how they actually work. Do it in two passes.

**Pass 1: judge every level on its own.** For each of the 10 levels, decide `solid`, `partial` or `missing` from the evidence, using the signals below. Judge depth and real use, not just whether a file exists. Note which of the level's requirements are met.

| Level | What it looks like (signals, not hard requirements) |
|-------|-----------------|
| 1 | Claude Code is installed and used. |
| 2 | A CLAUDE.md that says something useful. Works in git. Knows the core controls (plan mode, /clear, /compact). |
| 3 | 1+ working MCP server (project or user scope), or CLAUDE.md wires Claude to CLIs for real tools. Real data pulled into sessions. Browser access (Playwright MCP, Claude in Chrome) counts here too. |
| 4 | 3+ self-built skills in .claude/skills/ or .claude/commands/ (project or user level), used regularly. |
| 5 | Structured knowledge files: `docs/`, `memory/`, `memory.md`, or active auto memory, AND CLAUDE.md references or imports them. Context architecture, not just a config file. |
| 6 | Complex multi-phase skills (80+ lines) with approval gates. Skills chain together. Subagent definitions in .claude/agents/ or clear subagent usage. Hook configurations for quality gates. |
| 7 | Scripts or CI calling `claude -p`. JSON output piping. Claude GitHub Action or Agent SDK integration. |
| 8 | Parallel sessions: multiple git worktrees or tmux sessions running Claude, distinct agent roles, background or cloud sessions. |
| 9 | Scheduled routines, cron/launchd jobs or scheduled CI running Claude. Background agents. Claude as persistent infrastructure. |
| 10 | Autonomous execution loops. Orchestrators spawning and managing worker agents. Budgets, sandboxes, kill switches and rollback in place. |

**Pass 2: place the user.** Choose the level that best describes how they genuinely work today. Rules of thumb:

1. **A missing foundation doesn't cap you.** Someone running orchestrated agent swarms with budgets and kill switches is a Swarm Architect even if they never wrote a CLAUDE.md. The missing CLAUDE.md becomes a *gap to fill*, not a demotion.
2. **A shallow signal doesn't lift you.** One basic landing page or a single 10-line skill doesn't make someone a System Builder. Look for depth, quality and repeated use.
3. **Weigh four things:** depth (how sophisticated is the work?), breadth (how much of the surrounding levels is covered, even informally?), habit (is there evidence it's used regularly, not once?) and integration (do the pieces work together?).
4. **When torn between two levels, pick the lower one** and say exactly what would tip it.
5. **Write down your reasoning** in one or two sentences. It goes in the `placement` field and the user will read it.

**Gaps** are levels below the placement that aren't `solid`. Always surface them: they're usually the fastest wins.

**Important:** Having lots of files doesn't automatically mean advanced. 50 simple skills is still Level 4. Installed third-party skills and plugins show curiosity, but they don't count as self-built. The complexity and integration between components matters more than quantity.

### Step 1.4: Present the Assessment

Show the user their level with clear reasoning. The goal is to make them feel understood, not judged. Explain WHY each thing matters, not just that you found it.

```
## Your Claude Code Level: [X] / 10

**[Level Name]**

### Why I placed you at Level [X]:
[Your placement reasoning in plain words: what tipped it, and what you weighed. For example: "You're clearly running real pipelines and parallel agents, so you're past the middle levels even though a couple of foundations are missing."]

### Level by level:

Walk through the levels bottom to top. Give each one a verdict and the evidence behind it:

**Level 1 (Terminal Tourist): ✓ Solid.**
You have [specific thing], so you're clearly not just typing into a terminal with no setup.

**Level 2 (Grounded): ○ Gap to fill.**
[If it's a gap below their level, say so kindly: "You're well past this in practice, but there's no CLAUDE.md yet. Ten minutes on one would make every session start informed."]

**Level 3 (Connected): ✓ Solid.**
You have [X] MCP servers: [list them]. That means Claude can directly [what those connections enable].

[Continue for every level. Use ✓ Solid, ◐ Partial or ○ Not yet. Levels above their placement can be partial too: say what's already there.]

**Level [X+1] ([Name]): Next up.**
[Explain plainly what's missing and WHY it matters. Don't just say "no headless scripts found." Say something like: "Right now you're doing everything interactively. You open Claude, type a command, wait for output. Level 7 is about making Claude work without you sitting there: a script or CI job that calls Claude, processes the output, and saves it somewhere. I didn't find anything like that in your setup."]

### The short version:
[One paragraph summary, including any gaps worth filling first.]

### Where that puts you:
Most Claude Code users land between Level 2 and 5. At Level [X], you're [ahead of most / in the middle / at the frontier / etc.].
```

**Tone rules:**
- Talk to them like a knowledgeable friend, not a grading rubric
- Explain things like you're talking to someone smart who just hasn't seen this particular thing before
- If something is impressive, say so genuinely
- If something is messy or could be better, say that too, but constructively
- Use "you" and "your", not "the user"
- Avoid jargon without explanation. If you mention "headless mode" also say "that means running Claude from a script instead of typing into the terminal"
- The goal is: after reading this, they understand exactly what they have, what they're missing, and why the next level matters

If `--assess` flag was used, stop here. Otherwise continue to Phase 2.

---

## Phase 2: Your Roadmap

Show ONLY the next level transition. Do not dump all 10 levels. Focus is everything.

If there are gaps below the user's level, list them first as quick wins (one line each, with the single most useful fix), then show the next level's roadmap.

**Pick the matching section below based on assessed level.** The dashboard template contains the same content, so keep the two in sync when editing.

---

### Level 1 → Level 2: Get Grounded

**What this unlocks:** Claude knows who you are and how you work, so every session starts informed instead of from zero.

**Your roadmap:**

1. **Create your CLAUDE.md** (10 minutes)
   - This is the most important file you'll write. Claude reads it at the start of every session.
   - Run `/init` and Claude drafts one from your project. Then make it yours: who you are, your stack, how you like answers, and what "done" means (for example, tests pass).
   - Put personal preferences in `~/.claude/CLAUDE.md` so they apply to every project.

2. **Work in a git repo** (2 minutes)
   - Claude Code works best in git: every change is visible, reviewable and reversible.
   - Run `git init` or open an existing repo, then let Claude write commit messages and manage branches for you.

3. **Learn the core controls** (5 minutes)
   - **Shift+Tab** cycles modes, including **plan mode**. Plan first, build second. A great plan gives better results, and it's fine to throw away a bad one.
   - `/clear` starts fresh. Use it between tasks.
   - `/compact` summarises a long conversation to free up space.
   - `/context` shows what's filling Claude's context window.
   - `/model` switches models: the most capable for hard problems, faster ones for routine work.
   - **Esc** stops Claude mid-step. **Esc twice** rewinds to an earlier point.

4. **Do real work** (10 minutes)
   - Skip toy problems. Give Claude something you actually need done.
   - Watch whether it follows your CLAUDE.md. When it gets the same thing wrong twice, add a rule.

**Common mistake:** A vague CLAUDE.md. "Be helpful" means nothing. "Run the tests before saying you're done" changes behaviour.

**Keep it lean:** Every line of CLAUDE.md is loaded into every session. Keep it short and specific, and move detail into linked docs (that's Level 5).

**Community tip:** "The quality of your repository dictates the quality of your output." Zig-zagging leaves a messy repo, and a messy repo steers Claude badly. Ten degrees off now is a hundred degrees off later.

---

### Level 2 → Level 3: Connect Your Tools

**What this unlocks:** Claude reads your real tools (tickets, docs, repos, chat) and drives a real browser, so you stop copy-pasting context.

**What is MCP?** The Model Context Protocol is a standard plug that lets Claude talk to an app's API. Instead of you copying a ticket into the chat, Claude reads the ticket itself.

**Your roadmap:**

1. **Pick your first connection** (5 minutes)
   - Choose the tool you copy-paste from most: your issue tracker, docs, chat, or email.
   - You don't always need an MCP. If the tool has a CLI (like `gh`, `aws` or `gcloud`), Claude can use it directly. Mention it in your CLAUDE.md.

2. **Add it** (5 minutes)
   ```bash
   # most modern MCP servers are remote (HTTP)
   claude mcp add --transport http <name> <server-url>

   # then, inside Claude Code, sign in and check status
   /mcp
   ```
   - Add `--scope project` to save it to `.mcp.json` so your team gets it too, or `--scope user` to use it in every project.

3. **Test it with a real question** (5 minutes)
   - "Summarise the open tickets assigned to me."
   - "What changed in the last five merged pull requests?"
   - "Watch my CI build, fix any failures and retry."
   - Two sources is where it gets powerful: "Using the ticket *and* the pull request discussion, write the release notes."

4. **Give Claude a browser** (5 minutes)
   - Two good options: the **Claude in Chrome** extension, which works in your own Chrome with your logins, or the **Playwright MCP** for a separate, scriptable browser.
   ```bash
   claude mcp add playwright -- npx @playwright/mcp@latest
   ```

5. **Let Claude check its own work** (10 minutes)
   - The biggest unlock for quality: "Start the dev server, open the page, click through sign-up and fix anything broken." Claude checks screenshots and console errors, and loops until it works.
   - The same browser lets Claude read live sites and docs instead of relying on its training, and render HTML into screenshots and PDFs.

6. **Stay safe** (ongoing)
   - MCP servers and browsers act as you. Only install servers from sources you trust, check what access they ask for, and start read-only where you can. In your logged-in browser, keep Claude on sites you trust and watch anything that sends, buys or deletes.

**Fewer is better:** Tool search loads tool definitions only when needed, but every server still adds noise. Start with one or two you use daily, and switch off the rest in `/mcp`.

**Best first combos:**
- Developers: GitHub + your CI tool + issue tracker + Playwright
- QA: Playwright + GitHub + issue tracker
- Product owners: issue tracker + docs/wiki + GitHub
- Designers: Figma + whiteboard + issue tracker

**Community tip:** Connections plus a browser is where Claude starts replacing paid SaaS tools. People have cancelled deck, design and email tools after building their own workflows.

---

### Level 3 → Level 4: Build Your Skills

**What this unlocks:** Repeatable workflows. Write a task down once and Claude follows it every time, when you type its name or automatically when the task matches.

**Your roadmap:**

1. **Find your most repeated task** (5 minutes)
   - What do you keep explaining? "Review this for bugs and security issues." "Summarise this codebase's architecture." "Update the docs for the feature we just built." If you've said it twice, it should be a skill.

2. **Create the skill** (10 minutes)
   - A skill is a folder containing a `SKILL.md` file. The **description** tells Claude when to use it.
   - Save it in `~/.claude/skills/` to use everywhere, or `.claude/skills/` to share it with the repo.
   ```markdown
   # .claude/skills/review/SKILL.md
   ---
   name: review
   description: Review the current changes for bugs, security issues and style. Use before committing.
   ---

   # Review

   1. Run git diff and read every changed file
   2. Check for bugs, security issues and missing tests
   3. Report findings by severity, with file:line references
   ```

3. **Run it and iterate** (5 minutes)
   - Type `/review`. The first version won't be perfect, so fix whatever's off in the file.
   - Skills can also bundle scripts and reference files next to `SKILL.md`; Claude only loads them when needed.

4. **Build two or three more** (ongoing)
   - Most people start with three: one to research, one to create, one to review. Before long they start feeding into each other.

5. **Share them** (ongoing)
   - Commit skills to your repo so your team gets them, or package skills, agents and hooks into a **plugin** that others install with `/plugin`.

6. **Read before you install** (ongoing)
   - Skills can run code. Treat a skill from the internet like a pull request from a stranger: read it first.

**Community tip:** After five or more skills, output goes from hit-or-miss to reliable. Skills are an open format, so many other agent tools can use them too.

**Terminology:** Older `.claude/commands/*.md` files still work as simple slash commands. Skills are the modern format, and they can be triggered automatically.

---

### Level 4 → Level 5: Become a Context Architect

**What this unlocks:** A knowledge system Claude draws on, so output improves with every project instead of resetting each session.

**What is context architecture?** A set of files that tells Claude how your world works: conventions, decisions, examples of good work. It's the handbook you'd give a new hire on day one, instead of re-training them every morning.

**Your roadmap:**

1. **Write down how you work** (20 minutes)
   - Pick the shape that fits. Engineers usually want `docs/`; other roles often prefer a `memory/` folder. One topic per file.
   ```
   docs/
   ├── PRD.md            # what the product is and where it's going
   ├── architecture.md   # how the pieces fit together
   ├── conventions.md    # code style, naming, testing rules
   ├── features/         # one spec per feature
   └── adr/              # decisions, including what you chose NOT to do

   memory/
   ├── customers/        # one file per client or stakeholder
   ├── patterns/         # approaches that work
   └── examples/         # your best past work, as a reference
   ```

2. **Route Claude to it** (10 minutes)
   - Keep CLAUDE.md short and point it at the detail. `@path` imports load a file every session, so save them for essentials. For heavier docs, give the path and say when to read it.
   ```markdown
   # CLAUDE.md
   Follow the conventions in @docs/conventions.md
   Before starting a feature, read docs/architecture.md
   Before client work, read memory/customers/<client>.md
   ```

3. **Let Claude remember** (5 minutes)
   - Tell Claude "remember that we always use pnpm" and it saves the note to memory for future sessions. Open `/memory` to see and edit what gets loaded.

4. **Close the loop** (ongoing)
   - After each project, ask Claude to update the docs with what you learned. Month one feels slow; by month two it compounds.

**Pro tip:** Context is a budget. Quality drops as the window fills, so check `/context`, `/clear` between tasks, and keep always-loaded files lean.

**The formula:** Context quality = output quality. This isn't prompt engineering; it's context engineering.

---

### Level 5 → Level 6: Build Systems, Not Skills

**What this unlocks:** Skills, subagents and hooks working together like an assembly line, with checkpoints where you approve before anything ships.

**What does "systems" mean?** At Levels 4 and 5 you run one skill at a time. At Level 6 they connect: research feeds creation, creation gets reviewed. Subagents handle parts in parallel, and hooks enforce rules automatically.

**Your roadmap:**

1. **Add phases and approval gates** (15 minutes)
   - Break long skills into phases with checkpoints so Claude can't go off-track for ten minutes before you notice.
   ```markdown
   ## Phase 1: Research
   ...
   **Approval gate:** show me what you found and wait for my OK.

   ## Phase 2: Build
   ...
   **Approval gate:** show me the draft and wait for my OK.

   ## Phase 3: Review
   ...
   ```

2. **Chain your skills** (10 minutes)
   - Make the output of one skill the input of the next: `/research` writes `brief.md`, `/build` reads it, `/review` checks the result.

3. **Use subagents** (10 minutes)
   - A subagent is a separate Claude with its own context window. It does the digging and reports back a summary, keeping your main session clean.
   - Create specialists with `/agents` (saved in `.claude/agents/`), each with its own instructions and tools.
   - Ask for parallel work directly: "Use three subagents to investigate these in parallel."

4. **Add hooks** (10 minutes)
   - Hooks are commands that run automatically at fixed points: before a tool runs, after a file is edited, when Claude finishes. Unlike instructions, they *always* run.
   - Typical uses: run the linter after edits, block changes to secrets, run tests before Claude stops. Set them up with `/hooks` or in `.claude/settings.json`.
   ```json
   {
     "hooks": {
       "PostToolUse": [{
         "matcher": "Edit|Write",
         "hooks": [{ "type": "command", "command": "npm run lint" }]
       }]
     }
   }
   ```

5. **Build in quality** (ongoing)
   - Put review checklists inside your skills and point Claude at examples of "good". Output should be ready to ship, not "good enough".

**Community tip:** Level 5 to 6 is where Claude Code stops feeling like a tool and starts feeling like a team. The key is the orchestration layer, not more skills.

**Power move:** "Split this plan into phases with independent, testable tasks, and run them in parallel with subagents." Fast, but it uses a lot of tokens.

---

### Level 6 → Level 7: Programmatic Pipelines

**What this unlocks:** Claude works without you sitting there: from scripts, from CI, or inside your own apps.

**What is headless mode?** Running Claude from a script instead of a conversation. You hand it a task, it does the work and exits. It's the difference between a phone call and an email.

**Your roadmap:**

1. **Try headless once** (5 minutes)
   - `-p` means "do this task and exit". `--allowedTools` pre-approves what Claude may use, so it never stops to ask.
   ```bash
   claude -p "Summarise this week's changes and write them to CHANGES.md" \
     --allowedTools "Read,Write,Bash(git log:*)"
   ```

2. **Get structured output** (10 minutes)
   - Ask for JSON and pipe it into the next step: gather, analyse, report, each one automatic.
   ```bash
   claude -p "List the TODO comments in src/ as a JSON array" \
     --output-format json | jq -r '.result'
   ```

3. **Put Claude in CI** (15 minutes)
   - Run `/install-github-app` to set up the Claude GitHub Action. After that you can mention `@claude` in a pull request or issue to get a review, a fix or an implementation.

4. **Build your own** (ongoing)
   - The **Claude Agent SDK** (Python and TypeScript) gives you the same agent loop that powers Claude Code, inside your own scripts, services and products.

**Using skills from scripts:** In a headless run, describe the task in plain English or point Claude at the skill: "Follow .claude/skills/review/SKILL.md".

**Community tip:** Level 7 is where most people doing real automation land. It's the practical ceiling for most use cases. Beyond here, you're building infrastructure.

---

### Level 7 → Level 8: Multi-Agent Operations

**What this unlocks:** Several Claude sessions working in parallel, each on its own task, with you coordinating like a team lead.

**Your roadmap:**

1. **One worktree per agent** (10 minutes)
   - Git worktrees give each session its own checkout of the repo, so parallel agents never overwrite each other's files.
   ```bash
   git worktree add ../app-feature-a -b feature-a
   git worktree add ../app-bugfix -b bugfix
   # open a separate Claude session in each folder
   ```

2. **Give each session a role** (15 minutes)
   - Builder, test-writer, reviewer. Each gets its own instructions and stays in its lane. Work passes between them through files, branches and pull requests.

3. **Move work off your laptop** (10 minutes)
   - Run long jobs as background tasks while you keep working, or hand them to cloud sessions (Claude Code on the web) that come back as pull requests.

4. **Coordinate** (ongoing)
   - The hard part isn't running agents; it's keeping them out of each other's way. Use a shared task list, small pull requests, and a human review before anything merges. tmux is still handy for watching many sessions in one terminal.

**Community tip:** "We've all become endpoint facilitators for our agents. We're context generators, for now." This level takes discipline. Agents without coordination create chaos, not productivity.

---

### Level 8 → Level 9: Always On

**What this unlocks:** Claude runs on a schedule, so routine work is done before you show up.

**Your roadmap:**

1. **Schedule a task** (15 minutes)
   - `/schedule` creates **routines**: cloud agents that run on a cron schedule, no laptop required.
   - Or run locally with cron or launchd calling `claude -p`:
   ```bash
   # local alternative: weekdays at 7am
   0 7 * * 1-5 cd ~/project && claude -p "Write today's briefing to briefing.md"
   ```

2. **Recurring checks** (5 minutes)
   - `/loop` repeats a prompt on an interval within a session. Good for babysitting a deploy or a long-running job.
   ```
   /loop 10m check the deploy and tell me if anything fails
   ```

3. **React to events** (15 minutes)
   - With the GitHub Action, Claude can run when a pull request opens, an issue is labelled, or on a nightly schedule.

4. **Monitor and alert** (15 minutes)
   - Have Claude check your systems or data sources daily and only ping you when something needs attention.

**Stay safe:** Unattended agents should get the minimum tools and permissions they need, and log everything they do.

**Community tip:** Most people don't need this level. If your work is project-based, Levels 6 and 7 are the sweet spot. Level 9 is for running continuous operations.

---

### Level 9 → Level 10: Swarm Architecture

**What this unlocks:** You set a goal; an orchestrator breaks it down, spawns workers, checks their output and keeps going until it's done.

**What is a swarm?** At Level 8 you set up each session yourself. At Level 10 a coordinating agent does it: plans the work, fans tasks out to worker agents, verifies each result and assembles the whole.

**Your roadmap:**

1. **The autonomous loop** (advanced)
   - Give Claude a spec plus tests that define "done". It picks a task, does it, runs the tests, commits on green, retries on red, and repeats until everything passes.

2. **Orchestrator and workers** (advanced)
   - One agent plans and verifies; subagents do the work in parallel, each in its own worktree. Use workflows for repeatable multi-agent runs, or the Agent SDK to build your own orchestrator.

3. **Agents talking to agents** (advanced)
   - Agents coordinate through a shared task board (files, issues or branches): one writes tasks, others claim them, do the work and mark them done.

4. **Guardrails first** (advanced)
   - Run in a sandbox, never your main checkout
   - Set clear limits on what it can change
   - Cap attempts, time and token spend
   - Keep a kill switch within reach
   - Have a human review before anything reaches production

**Beware the hype:** Swarm frameworks come and go. Try them, learn from them, improve on them, but they aren't magic, and they can run up a hefty bill.

**Community tip:** Level 10 is experimental. Things break. The value is in the learning. This isn't "set and forget"; it's "build, monitor, iterate".

---

## Generate Dashboard

After completing Phase 1 (assessment) and Phase 2 (roadmap), ALWAYS generate an HTML dashboard and open it in the browser. This is not optional. Every `/dojo-me` run ends with a visual dashboard.

### Instructions

The dashboard uses a pre-built HTML template. Search these locations in order (first match wins):
1. `references/dojo-me-template.html` next to this SKILL.md file (check here first)
2. `~/.claude/skills/dojo-me/references/dojo-me-template.html` (user-level install)
3. `<project-root>/.claude/skills/dojo-me/references/dojo-me-template.html` (project-level install)
4. `~/.agents/skills/dojo-me/references/dojo-me-template.html` (universal agents install)

The template contains everything static: styling (a dark theme with code-rain hero), the logo, the roadmap for every level, tips and code examples. You only need to inject a small JavaScript object with the user's assessment data.

1. Find and read the `dojo-me-template.html` file from one of the locations above
2. Build the `ASSESSMENT` JavaScript object (see schema below)
3. Replace the line `// ASSESSMENT_DATA_PLACEHOLDER` in the template with `var ASSESSMENT = { ... };` containing the assessment data
4. Write the result to `~/Desktop/dojo-me-results.html`
5. Open it: run `open ~/Desktop/dojo-me-results.html` via Bash (or `xdg-open` on Linux, `start` on Windows)
6. Tell the user: "Your dojo-me dashboard is open in the browser."

### ASSESSMENT Object Schema

```javascript
var ASSESSMENT = {
  level: 4,                    // The assessed level (1-10)
  name: "Skilled",             // The level name
  summary: "You've got...",    // The "short version" paragraph from Step 1.4 (HTML allowed)
  placement: "You're running real skills daily, so...", // Why this level, 1-2 sentences (HTML allowed)
  arsenalNote: "I can see you've got a lot of coding experience...", // Personal paragraph for "What you have"
  roadmapNote: "You're not quite at Context Architect yet...",       // Personal paragraph for the next-level roadmap
  levels: [                    // Your verdict on every level, 1-10 in order
    { n: 1, status: "solid",   note: "You use Claude Code daily across three projects.", met: [true] },
    { n: 2, status: "partial", note: "You plan and work in git, but there's no CLAUDE.md yet.", met: [false, true, true] },
    // ... one entry per level; status is "solid" | "partial" | "missing"
    // met: one boolean per requirement in the template's checklist for that level (see below)
  ],
  capabilities: [              // One entry per detected capability
    {
      title: "CLAUDE.MD",      // Category name (uppercase)
      level: 2,                // Level this capability counts towards (links it to that level's panel)
      found: true,             // true = detected, false = missing
      detail: "33 lines, project-specific<br>No global CLAUDE.md", // HTML description
      note: "Solid start. Add a definition of done and it'll pull its weight." // Your personal take, shown when the card is clicked
    },
    {
      title: "MEMORY",
      level: 5,
      found: false,
      detail: "No memory/ or docs/ directory<br>No patterns or knowledge files"
    }
    // ... more capabilities
  ],
  context: {                   // Personalization data from environment scan + questions
    role: "developer",         // From Q1: coding/content creation/research/automation/etc.
    friction: "output quality",// From Q2: output quality/context limits/speed/etc.
    automationWish: "deployment", // From Q3: what they'd automate
    detectedMcps: ["GitHub"],  // MCPs found in their config
    suggestedMcps: [           // MCPs they should add next, based on role + existing setup
      "Issue tracker — read tickets, update status",
      "Playwright — let Claude test its own work in a browser"
    ],
    detectedSkills: ["dojo-me"], // Skills found in their setup
    suggestedFirstSkill: "a /deploy skill that runs your deployment, watches the build, and reports back",
    repoLanguages: ["typescript", "markdown"] // Languages detected in the repo
  }
};
```

**Personalization rules for the `context` object:**
- `suggestedMcps`: Recommend MCPs based on their role AND what they already have. Don't suggest MCPs they already have. Developers should get GitHub, their CI tool, their issue tracker. Product owners should get their issue tracker and docs/wiki. QAs should get Playwright, GitHub. Pick 2-3 most impactful.
- `suggestedFirstSkill`: Based on Q3 (what they'd automate), suggest a specific skill name and description. Make it concrete: not "a deployment skill" but "a `/deploy` skill that triggers your CI pipeline, monitors the build, and reports back."
- `repoLanguages`: Detected from file extensions in the current project.
- `placement`, `arsenalNote`, `roadmapNote`, every level `note` and every capability `note`: write them to the user, in your own words, about *their* setup. Name specific things you found. Be honest and warm, like the example: "I can see you've got a lot of coding experience, but you're not quite at Context Architect yet. Here's what would get you there." One to three sentences each; no generic filler.
- `levels[].met` follows the dashboard's checklist for each level, in this order:
  1: installed · 2: CLAUDE.md, git, core controls · 3: MCP or CLIs, real data, browser · 4: 3+ skills, used regularly · 5: knowledge files, CLAUDE.md routes to them · 6: phased skills with gates, chained skills, subagents, hooks · 7: scripts calling claude -p, JSON piping, Action or SDK · 8: worktrees or tmux, roles, background or cloud · 9: scheduled runs, unattended agents · 10: autonomous loops, orchestrators, guardrails
- Capability `level` values: CLAUDE.MD and GIT = 2, MCP / CONNECTIONS = 3, SKILLS = 4, MEMORY = 5, HOOKS & SUBAGENTS = 6, AUTOMATION = 7, MULTI-AGENT = 8.

The template's JavaScript will use these values to:
- Set the level number, name, summary and file card in the hero section
- Render capability cards with Found/Missing badges; clicking a card opens a panel with what it is, why it matters, what the scan found, your `note`, steps to get or improve it, and a link to its level
- Build the level track and level list from your `levels` verdicts: solid levels, gaps below the placement, partial progress above it (every level is clickable: what it takes with per-item ticks, your note on where the user stands, and the steps to reach it; capabilities with a matching `level` appear in that panel)
- Show your `arsenalNote` under "What you have", your `roadmapNote` on the next-level roadmap, and a "Gaps to fill" strip when there are gaps
- Render the roadmap for the next level from the template's built-in content
- Show personalized recommendation boxes (MCP suggestions on the Level 2 → 3 roadmap, skill suggestions on Level 3 → 4)
- Build the "All 10 levels" grid with the correct highlighting

---

## Phase 3: Build It Now

**Ask the user:** "Want me to build the first step of your roadmap right now?"

If yes, execute the matching build step below. If no, end with the roadmap summary.

---

### Build: CLAUDE.md Starter (for Level 1)

Create a CLAUDE.md in the current project root (or `~/.claude/CLAUDE.md` for personal, cross-project preferences) using the context questions:

```markdown
# [Project/Business Name]

## About
- **Role:** [from Q1 answer]
- **Main work:** [from Q1 answer]
- **Key tools:** [detected from environment]

## How to work
- [Address their biggest friction from Q2]
- Definition of done: [tests pass / lint clean / reviewed]
- Plan before large changes

## Communication Style
- Be direct and actionable
- Use plain language

## Project Context
[Based on files and structure detected]

## What I Want to Automate
- [ ] [From Q3 answer]
```

Adapt this template. Make it specific to the user. Don't use it verbatim. Show it to them before saving.

---

### Build: First Connection (for Level 2)

Based on Q1, recommend and configure their first MCP or CLI connection:

1. Determine the best MCP (or CLI) for their use case
2. Add it with `claude mcp add` (choose project or user scope)
3. Walk through authentication via `/mcp`
4. Test with a real query

---

### Build: First Skill (for Level 3)

Based on Q3 (what they'd automate):

1. Create `.claude/skills/<name>/SKILL.md` (or `~/.claude/skills/<name>/` for personal use)
2. Write a clear `description` so Claude knows when to use it, then the steps and output format
3. Test it by running the command
4. Suggest 2-3 more skills they should build next

---

### Build: Context Architecture (for Level 4-5)

1. Create a `docs/` or `memory/` structure that fits their work
2. Seed it with initial files based on their work
3. Update CLAUDE.md to import essentials and route to the rest
4. Show how the feedback loop works (including auto memory and `/memory`)

---

### Build: Multi-Phase Skill Upgrade (for Level 5-6)

1. Take their most-used existing skill
2. Rewrite it with phases, approval gates and subagent calls
3. Add a subagent definition in `.claude/agents/` and one quality-gate hook
4. Test the upgraded version

---

### Build: First Automation (for Level 6-7)

1. Create a script that calls `claude -p` for a routine task (with `--allowedTools` and, if useful, `--output-format json`)
2. Test it end-to-end
3. Show how to run it from CI (GitHub Action) or on a schedule

---

### Build: Advanced Optimization (for Level 7+)

For power users:
1. **Context budget audit:** How much context is consumed on startup (check `/context`)? Optimize.
2. **Skill architecture review:** Which skills could chain? Build connections.
3. **Memory file health check:** Stale files? Outdated patterns? Clean up.
4. **MCP health check:** All configured MCPs actually working? Test each one.
5. **Pipeline review:** Where could headless runs, routines or CI replace interactive sessions?

---

## Community Wisdom (from GenAI Circle)

**On context windows:**
Break tasks into smaller chunks and `/clear` between them. Auto-compact (the community calls it "the lobotomy") loses important detail. Better to do 5 focused sessions than 1 marathon.

**On getting started:**
Your CLAUDE.md is 80% of the early value. Spend 30 minutes on it before anything else. Claude can only be as good as the context you give it.

**On MCP management:**
Fewer is better. Tool search keeps startup context lean, but don't load MCPs you don't use daily.

**On replacing SaaS:**
Track what you're paying for monthly. Try replacing one tool per week. Members have cancelled deck, design, email and enrichment tools after building equivalent workflows.

**On building your knowledge system:**
The pattern is Claude Code + MCP + markdown docs/memory + git. Your system gets smarter with every project. The first month feels slow. By month two, the compounding kicks in.

**On autonomous work:**
Start supervised. Remove guardrails gradually. The people who trust too fast burn tokens on bad output. The people who never trust miss the productivity multiplier.

**On the levels:**
There's no shame in being at Level 1. The jump from 1 to 4 takes a weekend. The jump from 4 to 6 takes a few weeks of daily use. Most people find their sweet spot between 5 and 7. Going higher means running infrastructure, and that's only worth it if your work demands it.

---

## After Running This

Come back and run `/dojo-me` again whenever you're ready for the next step. The assessment updates based on your actual environment.

---

**Version:** 6.0.0
**Based on the 10 Levels of Claude Code by the GenAI Circle community** (thegenaicircle.com)
