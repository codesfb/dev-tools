# Agent Skills Guide

A practical, categorized reference for installing and using agent skills (`SKILL.md` folders) with Claude Code, Codex, Cursor, Gemini CLI and other agents that follow the Agent Skills standard.

- **Last researched:** November 2026. Repositories change quickly, so check each README before installing.
- **How to extend this file:** every category ends with an `ADD NEW SKILLS HERE` marker, and [section 6](#6-adding-a-new-skill) has a copy-paste template.

---

## Contents

1. [Install basics](#1-install-basics)
2. [Skill index](#2-skill-index)
3. [Categories](#3-categories)
   - [3.1 Design direction and process](#31-design-direction-and-process)
   - [3.2 Landing pages and conversion](#32-landing-pages-and-conversion)
   - [3.3 Motion and animation](#33-motion-and-animation)
   - [3.4 Polish and design review](#34-polish-and-design-review)
   - [3.5 Immersive and award-style experiences](#35-immersive-and-award-style-experiences)
   - [3.6 Design systems and themes](#36-design-systems-and-themes)
   - [3.7 Frontend engineering and testing](#37-frontend-engineering-and-testing)
   - [3.8 Game art, sprites and maps](#38-game-art-sprites-and-maps)
   - [3.9 Visual assets and media](#39-visual-assets-and-media)
   - [3.10 Documents and files](#310-documents-and-files)
   - [3.11 Meta: building skills and MCP servers](#311-meta-building-skills-and-mcp-servers)
4. [Recommended stacks](#4-recommended-stacks)
5. [Security and good practice](#5-security-and-good-practice)
6. [Adding a new skill](#6-adding-a-new-skill)

---

## 1. Install basics

A **skill** is a folder with a `SKILL.md` (YAML frontmatter with `name` and `description`, then instructions) and optional references or scripts. The agent loads a skill when the task matches its description.

### Option A: the `skills` CLI (recommended)

Needs Node.js. It detects your installed agents and asks where to install.

```bash
npx skills add <owner>/<repo>                      # all skills in a repo
npx skills add <owner>/<repo> --skill <name>       # one skill
npx skills update -g                               # update global installs
npx skills remove <name> -g                        # uninstall
```

### Option B: manual copy (no Node)

```bash
git clone https://github.com/<owner>/<repo>.git /tmp/<repo>
mkdir -p .claude/skills
cp -r /tmp/<repo>/skills/<skill-name> .claude/skills/
```

### Where skills live

| Scope | Claude Code | Codex CLI / Replit | Gemini CLI | Grok Build |
|---|---|---|---|---|
| Project | `.claude/skills/` | `.agents/skills/` | `.gemini/skills/` | `.grok/skills/` |
| Global | `~/.claude/skills/` | `~/.agents/skills/` | see Gemini CLI docs | `~/.grok/skills/` |

Restart the agent after installing so the skills load. In Claude Code, run `/skills` to list what is loaded.

### Claude Code plugin marketplaces

Some repos ship as plugins:

```
/plugin marketplace add <owner>/<repo>
/plugin install <plugin>@<marketplace>
```

Use the plugin **or** the CLI for a given skill, not both, or it may appear twice.

### Agents without native skill support (for example Aider)

Load the `SKILL.md` as read-only context instead:

```bash
aider --read .claude/skills/<skill>/SKILL.md
# or inside Aider:
/read-only .claude/skills/<skill>/SKILL.md
```

---

## 2. Skill index

Quick lookup. Details and install commands are in section 3.

| Category | Skill | Repository |
|---|---|---|
| Design direction | `frontend-design` | anthropics/skills |
| Design direction | `web-design-engineer` | ConardLi/garden-skills |
| Design direction | `impeccable` | pbakaus/impeccable |
| Design direction | `design-engineering` | HKTITAN/design-engineering |
| Design direction | `design-taste-frontend` | Leonxlnx/taste-skill |
| Landing pages | `landing-page-design` | elayadesign/ai-design-skills |
| Motion | `emil-design-eng`, `animate`, `animation-vocabulary`, `apple-design` | emilkowalski/skills |
| Polish and review | `make-interfaces-feel-better` | jakubkrehel/make-interfaces-feel-better |
| Polish and review | `review-animations`, `improve-animations` | emilkowalski/skills |
| Immersive | `build-awwwards-quality-sites` | MengTo/Skills |
| Immersive | `storytelling-web` | wang1212/storytelling-web |
| Design systems | `theme-factory`, `brand-guidelines` | anthropics/skills |
| Frontend engineering | `webapp-testing`, `web-artifacts-builder` | anthropics/skills |
| Frontend engineering | `pick-ui-library` | emilkowalski/skills |
| Game art | `generate2dsprite`, `video2dsprite`, `generate2dmap`, `generate2dmedia`, `codeart2d` | 0x0funky/agent-sprite-forge |
| Visual assets | `canvas-design`, `algorithmic-art` | anthropics/skills |
| Visual assets | `gpt-image-2`, `web-video-presentation`, `beautiful-article` | ConardLi/garden-skills |
| Documents | `docx`, `pdf`, `pptx`, `xlsx` | anthropics/skills |
| Meta | `skill-creator`, `mcp-builder` | anthropics/skills |

---

## 3. Categories

### 3.1 Design direction and process

Skills that give the agent a design process before it writes code, so output does not default to the generic template look (same font everywhere, purple gradients, cards inside cards).

| Skill | Repository | Use it for |
|---|---|---|
| `frontend-design` | anthropics/skills | Choosing a distinct visual direction, type roles and layout concept. Official baseline. |
| `web-design-engineer` | ConardLi/garden-skills | Gathering references and style recipes first, declaring a design system before coding, shipping an early v0, then a pre-delivery checklist and scored critique. Uses web search, so it works best when search is available. |
| `impeccable` | pbakaus/impeccable | A design vocabulary with 24 commands (steer, review, finish, plan) plus a deterministic detector for "AI slop". Details below. |
| `design-engineering` | HKTITAN/design-engineering | A navigable skill graph on surfaces, components and layout details. |
| `design-taste-frontend` | Leonxlnx/taste-skill | Anti-template rules for landing pages, portfolios and redesigns. The repo also has `image-to-code` and `redesign-existing-projects`. |

```bash
npx skills add anthropics/skills --skill frontend-design
npx skills add conardli/garden-skills --skill web-design-engineer
npx skills add HKTITAN/design-engineering
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

#### Impeccable in detail

```bash
npx impeccable install          # from your project root; detects tools, offers the design hook
npx impeccable update           # refresh an existing install
npx impeccable install --providers=claude,cursor --scope=project   # non-interactive
```

Claude Code plugin alternative: `/plugin marketplace add pbakaus/impeccable`.

After installing, reload your tool and run `/impeccable init` once per project (`$impeccable init` on Codex). It writes `PRODUCT.md`; `/impeccable document` writes `DESIGN.md`.

| Group | Commands |
|---|---|
| Steer | `bolder`, `quieter`, `distill`, `colorize`, `typeset`, `layout`, `animate`, `delight`, `overdrive` |
| Review | `critique` (scored UX review), `audit` (accessibility, performance, theming, responsive) |
| Finish | `polish`, `harden`, `clarify`, `adapt`, `optimize`, `onboard` |
| Plan | `shape`, `init`, `document`, `extract` |

```
/impeccable critique the pricing cards
/impeccable polish settings
```

Detector without an agent (CI or terminal):

```bash
npx impeccable detect src/
npx impeccable detect http://localhost:5173
```

Notes:
- Keep `PRODUCT.md`, `DESIGN.md`, `config.json` and `design.json` in git. The README lists `.gitignore` entries for the rest of `.impeccable/`.
- Claude Code hooks run even when you deny the model's own commands. Review installed hooks before unattended runs.
- Live mode edits a local checkout only. Do not use it on a deployed site.
- Anonymous telemetry can be disabled with `npx impeccable telemetry off` or `DO_NOT_TRACK=1`.

<!-- ADD NEW SKILLS HERE (3.1 Design direction and process) -->

---

### 3.2 Landing pages and conversion

| Skill | Repository | Use it for |
|---|---|---|
| `landing-page-design` | elayadesign/ai-design-skills | One offer, one audience, one action. Covers intake questions, page structure, conversion copy, SEO and a visual system (type, spacing, radius, motion). |

```bash
npx skills add elayadesign/ai-design-skills --skill landing-page-design
```

The repo credits motion ideas from Emil Kowalski's and Leon Lin's skills. Check its README for other skills it adds over time.

<!-- ADD NEW SKILLS HERE (3.2 Landing pages and conversion) -->

---

### 3.3 Motion and animation

Repository: **emilkowalski/skills** (MIT).

| Skill | Use it for |
|---|---|
| `emil-design-eng` | Main skill: when to animate, easing and duration, component feel, general polish. |
| `animate` | Building an animation from scratch with the right curve, duration and properties. |
| `animation-vocabulary` | Mapping what you see or feel to precise terms, so prompts get better results. |
| `apple-design` | Apple interface and motion principles, adapted for the web. |

```bash
npx skills@latest add emilkowalski/skills                      # whole set
npx skills add https://github.com/emilkowalski/skills --skill emil-design-eng   # one skill
```

For motion-heavy pages, store curves and durations as shared design tokens instead of hand-typing near-identical cubic-beziers.

<!-- ADD NEW SKILLS HERE (3.3 Motion and animation) -->

---

### 3.4 Polish and design review

Skills that run at the end of the process and look for problems.

| Skill | Repository | Use it for |
|---|---|---|
| `make-interfaces-feel-better` | jakubkrehel/make-interfaces-feel-better | Small details: concentric border radius, optical alignment, shadows, hit areas, image outlines, press feedback, typography. |
| `better-ui`, `better-typography` | jakubkrehel/skills | Related Jakub Krehel skills for UI polish and text. |
| `review-animations` | emilkowalski/skills | Strict review of animation code in a diff. Declines general reviews. |
| `improve-animations` | emilkowalski/skills | Audits all animation in a codebase and writes prioritized, self-contained plans. |
| `critique` and `audit` commands | pbakaus/impeccable | Scored UX review and an accessibility/performance/responsive report (P0 to P3). |

```bash
npx skills add jakubkrehel/make-interfaces-feel-better
npx skills add https://github.com/jakubkrehel/skills --skill better-ui
```

Usage: ask the agent to review a specific component or folder, for example "review `src/components/` with make-interfaces-feel-better". Narrow targets give better findings than a whole app.

<!-- ADD NEW SKILLS HERE (3.4 Polish and design review) -->

---

### 3.5 Immersive and award-style experiences

For portfolios, launches and sites where visual impact matters. Use sparingly: these add heavy motion and can clash with minimal design systems.

| Skill | Repository | Use it for |
|---|---|---|
| `build-awwwards-quality-sites` | MengTo/Skills | Raises the experience layer: motion stack, optional Three.js, asset sourcing and validation before handoff. It tells the agent to pick one compatible web-design skill and not to claim an award without evidence. |
| `awwwards` | juliobsz/skills | Alternative: builds an award-caliber site from one line (`/awwwards`). |
| `awwwards-inspiration` | INSANE0777/awwwards-skills | Design references from Awwwards via an MCP server (`npx -y awwwards-mcp`). |
| `storytelling-web` | wang1212/storytelling-web | Turns text into slide-style or scroll-driven narrative pages (single-file HTML). |

```bash
npx skills add MengTo/Skills --skill build-awwwards-quality-sites
npx skills add juliobsz/skills -g
npx skills add wang1212/storytelling-web
```

<!-- ADD NEW SKILLS HERE (3.5 Immersive and award-style experiences) -->

---

### 3.6 Design systems and themes

| Skill | Repository | Use it for |
|---|---|---|
| `theme-factory` | anthropics/skills | 10 preset themes (colors and fonts), or generate a theme from keywords or brand colors. |
| `brand-guidelines` | anthropics/skills | Applying Anthropic's visual identity to artifacts. |
| `design-engineering` | HKTITAN/design-engineering | Surface, component and layout principles (also listed in 3.1). |
| `/impeccable document`, `extract` | pbakaus/impeccable | Capture an existing visual system in `DESIGN.md` and extract repeated patterns. |

```bash
npx skills add anthropics/skills --skill theme-factory
```

<!-- ADD NEW SKILLS HERE (3.6 Design systems and themes) -->

---

### 3.7 Frontend engineering and testing

| Skill | Repository | Use it for |
|---|---|---|
| `webapp-testing` | anthropics/skills | Testing a local web app with Playwright: verify behavior, take screenshots, read browser logs. |
| `web-artifacts-builder` | anthropics/skills | Multi-component HTML artifacts with React, Tailwind and shadcn/ui. |
| `pick-ui-library` | emilkowalski/skills | Picking trusted UI libraries instead of hand-rolling components or installing abandoned packages. |

```bash
npx skills add anthropics/skills --skill webapp-testing
npx skills add anthropics/skills --skill web-artifacts-builder
```

<!-- ADD NEW SKILLS HERE (3.7 Frontend engineering and testing) -->

---

### 3.8 Game art, sprites and maps

#### Agent Sprite Forge

Repository: **0x0funky/agent-sprite-forge** (MIT). Turns one approved still image into a full game-ready sprite set (idle, walk, run, attack, jump, hurt), and generates layered maps. It is a set of **five skills that call each other's scripts**, so keep the folders together.

| Skill | Use it for |
|---|---|
| `generate2dsprite` | Master stills, characters, creatures, props, icons, FX and sprite sheets. |
| `video2dsprite` | A whole sprite set from one master still (one image-to-video clip per action), keyed, looped, retimed, finished and packaged. |
| `generate2dmap` | Top-down and side-scroll maps, tiles, prop kits, parallax and HD-2D plates, with collision and navigation checks. |
| `generate2dmedia` | Routes every image or video request: your API key first, then your signed-in Codex or Grok CLI, then code art. |
| `codeart2d` | Last-resort code-drawn pixel sprites, vector art, FX and tiles, always disclosed as code-drawn. |

**Pipeline:** master still → one clip per action → cut loops and one-shots → finish (HD default, pixel on request) → package and export.

**Exports:** animation.json with PNG atlas, WebM alpha, packed-alpha MP4; Godot SpriteFrames; Aseprite JSON; maps for Tiled, Godot and LDtk.

**Requirements**

| Need | Install |
|---|---|
| All skills | Python 3.10+ and `python -m pip install -r requirements.txt` (numpy, Pillow, scipy) |
| Video, sprite sets, packaging | ffmpeg 5.1+ with libvpx-vp9 and libx264 on `PATH` |
| Image and video generation (one of) | An API key for OpenAI, Gemini, xAI, BytePlus or fal.ai, or your own signed-in Codex CLI or Grok CLI |
| codeart2d SVG art (optional) | `python -m pip install -r requirements-codeart.txt` |

**Install: Claude Code (plugin)**

```bash
claude plugin marketplace add 0x0funky/agent-sprite-forge --sparse .claude-plugin skills
claude plugin install agent-sprite-forge@agent-sprite-forge
python -m pip install "numpy>=1.26" "Pillow>=10.1" "scipy>=1.11"
```

**Install: Codex**

```bash
git clone https://github.com/0x0funky/agent-sprite-forge.git
cd agent-sprite-forge
python -m pip install -r requirements.txt
python tools/install_skills.py --apply --host codex
```

**Install: Grok or a generic folder** (from the same clone)

```bash
python tools/install_skills.py --apply --host grok      # ~/.grok/skills
python tools/install_skills.py --apply --host agents    # ~/.agents/skills
python tools/install_skills.py --apply --dest <folder>  # any skills folder
python tools/install_skills.py --check --host codex     # report drift later
```

`--apply` backs up an existing copy and writes a sha256 manifest. Start a new agent session afterwards.

**Optional API keys** go in `~/.config/agent-sprite-forge/config.json` (Windows: `%APPDATA%\agent-sprite-forge\config.json`) or in environment variables such as `OPENAI_API_KEY`, `GEMINI_API_KEY`, `XAI_API_KEY`, `ARK_API_KEY`, `FAL_KEY`. Keep the file private (`chmod 600`). Without a key it uses your signed-in Codex or Grok CLI.

**Example prompts**

```
Use $generate2dsprite to make a master still of a fox ranger for a 2D platformer: orange fur, teal tunic, side view facing right. Show me three takes.
```

```
Use $video2dsprite to turn the approved fox master into idle, run, attack and hurt, pixel finish in 48x64 cells at 80 ms, and export it for Godot and Aseprite.
```

```
Use $generate2dmap to create a small top-down village with a pond, roads to three exits, collision and a walkable HTML preview, then export it to Tiled.
```

**Know before you use it**
- Paid API calls can cost money. Every call is logged in `<project>/.forge/ledger.jsonl`, and there is no spending cap unless you set one (`--budget-usd`, `--max-calls`).
- Image-to-video models can drift on fast limbs and jumps, so expect retakes on run and jump.
- Per the README, the Godot, LDtk and Aseprite exports were not yet verified in the editors themselves. Test imports before relying on them.
- Generated assets fall under each provider's terms, and the MIT license covers only the repo's code. Use original characters or IP you control for commercial work.

<!-- ADD NEW SKILLS HERE (3.8 Game art, sprites and maps) -->

---

### 3.9 Visual assets and media

| Skill | Repository | Use it for |
|---|---|---|
| `canvas-design` | anthropics/skills | Original posters and static visual pieces as PNG or PDF. |
| `algorithmic-art` | anthropics/skills | Generative art with p5.js, seeded randomness and tunable parameters. |
| `slack-gif-creator` | anthropics/skills | Animated GIFs for Slack. |
| `gpt-image-2` | ConardLi/garden-skills | Image generation and prompting. |
| `web-video-presentation` | ConardLi/garden-skills | Web video and presentations with 23 built-in themes. |
| `beautiful-article` | ConardLi/garden-skills | Turning any source into a styled article. |

```bash
npx skills add anthropics/skills --skill canvas-design
npx skills add conardli/garden-skills --skill web-video-presentation
```

<!-- ADD NEW SKILLS HERE (3.9 Visual assets and media) -->

---

### 3.10 Documents and files

Repository: **anthropics/skills**. These are source-available (not open source) and power Claude's file creation features.

| Skill | Use it for |
|---|---|
| `docx` | Word documents |
| `pdf` | Reading, merging, splitting and creating PDFs, plus form filling |
| `pptx` | PowerPoint decks |
| `xlsx` | Spreadsheets and CSV cleanup |

```bash
npx skills add anthropics/skills --skill docx
npx skills add anthropics/skills --skill pdf
```

Other writing and communication skills in the same repo: `doc-coauthoring`, `internal-comms`.

<!-- ADD NEW SKILLS HERE (3.10 Documents and files) -->

---

### 3.11 Meta: building skills and MCP servers

| Skill | Repository | Use it for |
|---|---|---|
| `skill-creator` | anthropics/skills | Creating, testing and improving your own skills. |
| `mcp-builder` | anthropics/skills | Building MCP servers (Python FastMCP or Node/TypeScript SDK). |
| `claude-api` | anthropics/skills | Building apps with the Claude API. |

A minimal skill is a folder with a `SKILL.md`:

```markdown
---
name: my-skill-name
description: What this skill does and when to use it
---

# My Skill Name

Instructions the agent follows when this skill is active.
```

The `anthropics/skills` repo also has a `template/` folder and the Agent Skills spec in `spec/`.

<!-- ADD NEW SKILLS HERE (3.11 Meta) -->

---

## 4. Recommended stacks

Each skill you install adds context. Start small and add layers only when the project needs them.

### Lean starter (any web project)

`frontend-design` + `impeccable` + `emil-design-eng`

### New landing page

1. `web-design-engineer`: references, style recipe and a declared design system.
2. `landing-page-design`: offer, structure and conversion copy.
3. `emil-design-eng` and `animate`: principles and motion.
4. `build-awwwards-quality-sites`: only if the project needs an extra experience layer.
5. `make-interfaces-feel-better`: final review.

### Improve an existing interface

1. `impeccable`: `critique` and `audit` first, then `polish`, `typeset`, `layout`.
2. `make-interfaces-feel-better`: fine details.
3. `review-animations` and `improve-animations`: if the UI has motion.

### Motion-focused work

`emil-design-eng` → `animation-vocabulary` (to phrase requests) → `animate` → `review-animations`, with `apple-design` as a reference.

### Game prototype

`agent-sprite-forge` (master still → sprite set → map) → export to Godot, Aseprite or the web → `webapp-testing` if the game runs in a browser.

### Documents and reports

`docx`, `pdf`, `pptx` or `xlsx` from anthropics/skills, matching the output format.

### Avoiding conflicts

- Pick one visual direction at the start and tell the agent. A minimal system and a heavy-motion layer can contradict each other.
- If the agent gets slow or inconsistent, install fewer skills per project instead of more.

---

## 5. Security and good practice

- A skill is instructions plus, sometimes, scripts the agent will run. **Read the `SKILL.md` and any script before installing** a third-party repo.
- Prefer repos with real usage and history. For important projects, pin a version (git submodule or a local copy).
- Skills with hooks (such as Impeccable) run commands automatically. Review what they add to your agent config.
- Skills that call paid APIs (such as Agent Sprite Forge) can spend money. Set budget caps and check the ledger.
- Never put API keys inside a project folder or commit them.
- Keep this file's tables in sync with what you actually have installed.

---

## 6. Adding a new skill

### Checklist

1. Open the repo and read its `README.md` and `SKILL.md`.
2. Find the exact skill name (the folder under `skills/` or the `name:` in the frontmatter).
3. Check requirements (Node, Python, ffmpeg, API keys) and license.
4. Test the install in a scratch project.
5. Add a row to the [index](#2-skill-index) and to the matching category in section 3. Create a new category if none fits.

### Entry template (copy into the right category)

```markdown
| `skill-name` | owner/repo | One line on what it is for. |
```

```bash
npx skills add owner/repo --skill skill-name
```

### Full-entry template (for skills with requirements or caveats)

````markdown
#### Skill or repo name

Repository: **owner/repo** (license)

| Skill | Use it for |
|---|---|
| `skill-name` | ... |

**Requirements:** ...

**Install**

```bash
...
```

**Know before you use it:** costs, limits, security notes.
````

### New category template

````markdown
### 3.X Category name

One sentence on when to use this category.

| Skill | Repository | Use it for |
|---|---|---|

```bash
# install commands
```

<!-- ADD NEW SKILLS HERE (3.X Category name) -->
````
