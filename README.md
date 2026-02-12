<div align="center">

# ✨ Story Skills

**End-to-end story writing powered by markdown and AI agents.**

Manage characters, build worlds, structure plots, and write chapters — all as structured markdown files with YAML frontmatter.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent_Skills-SKILL.md-blue)](https://agentskills.io)
[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-blueviolet)](https://docs.anthropic.com/en/docs/claude-code)

</div>

---

> Works with **Claude Code**, **GitHub Copilot**, **Cursor**, **Windsurf**, **Gemini CLI**, **OpenAI Codex**, **OpenCode**, and any agent that supports the `SKILL.md` standard.

## 🚀 Quick Start

```shell
# Claude Code — install in two commands
/plugin marketplace add danjdewhurst/story-skills
/plugin install story-skills@story-skills
```

Then just say **"Start a new story"** and the agent takes it from there.

## 📦 Installation

<details>
<summary><strong>Claude Code</strong></summary>

```shell
# Add the marketplace
/plugin marketplace add danjdewhurst/story-skills

# Install the plugin
/plugin install story-skills@story-skills
```

</details>

<details>
<summary><strong>GitHub Copilot (VS Code)</strong></summary>

[VS Code with Copilot](https://code.visualstudio.com/docs/copilot/customization/agent-skills) discovers skills from multiple directories:

```shell
git clone https://github.com/danjdewhurst/story-skills.git

# Copy skills to your project (any of these work)
cp -r story-skills/skills/* .github/skills/
cp -r story-skills/skills/* .agents/skills/

# Or install globally
cp -r story-skills/skills/* ~/.copilot/skills/
```

Skills auto-activate when your request matches a skill's description or can be invoked manually.

</details>

<details>
<summary><strong>Cursor</strong></summary>

[Cursor](https://www.cursor.com) supports the `SKILL.md` standard:

```shell
git clone https://github.com/danjdewhurst/story-skills.git

cp -r story-skills/skills/* .agents/skills/
```

</details>

<details>
<summary><strong>Windsurf</strong></summary>

[Windsurf](https://windsurf.com) discovers skills from workspace and global directories:

```shell
git clone https://github.com/danjdewhurst/story-skills.git

# Copy skills to your project
cp -r story-skills/skills/* .windsurf/skills/

# Or install globally
cp -r story-skills/skills/* ~/.codeium/windsurf/skills/
```

Cascade auto-invokes skills when your request matches a skill's description, or use `@skill-name` to invoke manually.

</details>

<details>
<summary><strong>Gemini CLI</strong></summary>

[Gemini CLI](https://github.com/google-gemini/gemini-cli) supports the same `SKILL.md` format via the [Agent Skills](https://agentskills.io) standard:

```shell
# Install all skills globally
gemini skills install https://github.com/danjdewhurst/story-skills.git

# Or install a specific skill
gemini skills install https://github.com/danjdewhurst/story-skills.git --path skills/story-init
gemini skills install https://github.com/danjdewhurst/story-skills.git --path skills/character-management
gemini skills install https://github.com/danjdewhurst/story-skills.git --path skills/worldbuilding
gemini skills install https://github.com/danjdewhurst/story-skills.git --path skills/plot-structure
gemini skills install https://github.com/danjdewhurst/story-skills.git --path skills/chapter-writing

# Or link locally after cloning
git clone https://github.com/danjdewhurst/story-skills.git
gemini skills link story-skills/skills
```

Gemini will auto-discover the skills and activate them when your request matches a skill's description.

</details>

<details>
<summary><strong>Codex (OpenAI)</strong></summary>

[Codex](https://github.com/openai/codex) uses the same `SKILL.md` format via the [Agent Skills](https://agentskills.io) standard:

```shell
git clone https://github.com/danjdewhurst/story-skills.git

# Install to your user skills (available across all projects)
cp -r story-skills/skills/* ~/.agents/skills/

# Or install to a specific repo
cp -r story-skills/skills/* .agents/skills/
```

Codex detects new skills automatically.

</details>

<details>
<summary><strong>OpenCode</strong></summary>

The skills use the same `SKILL.md` format that [OpenCode](https://opencode.ai) supports natively:

```shell
git clone https://github.com/danjdewhurst/story-skills.git

# Copy skills to your project
cp -r story-skills/skills/* .opencode/skills/

# Or install globally
cp -r story-skills/skills/* ~/.config/opencode/skills/
```

OpenCode also searches `.claude/skills/` paths, so project-level Claude skills are automatically discovered.

</details>

<details>
<summary><strong>Other platforms</strong></summary>

These skills follow the open [Agent Skills](https://agentskills.io) standard (`SKILL.md` with YAML frontmatter). They work with any compatible agent — just copy the skill folders to your agent's skills directory.

For non-agent use:

- **Claude.ai / ChatGPT Projects** — add the SKILL.md and reference files as project knowledge
- **Any LLM API** — include skill content in system prompts
- **Manual use** — the templates, workflows, and story structure are model-agnostic

</details>

## 🛠️ Skills

| Skill | What it does | Try saying |
|-------|-------------|------------|
| **story-init** | Scaffolds a complete story project with folder structure, story bible, and registries | *"Start a new story"* |
| **character-management** | Creates rich character profiles with relationships, traits, arcs, and family trees | *"Create a character"* |
| **worldbuilding** | Builds locations and world systems — magic, politics, technology, religion, and more | *"Design a magic system"* |
| **plot-structure** | Plans story arcs using structures like three-act, hero's journey, save the cat, kishotenketsu | *"Create a plot arc"* |
| **chapter-writing** | Writes chapters using an outline-first workflow, pulling context from all story files for consistency | *"Write the next chapter"* |

## 📁 Project Structure

Running **story-init** creates this layout:

```
my-story/
├── story.md                  # Story bible — title, genre, themes, POV, tense
├── characters/
│   └── _index.md             # Character registry
├── worldbuilding/
│   ├── _index.md             # World overview
│   ├── locations/
│   └── systems/
├── plot/
│   ├── _index.md             # Arc overview
│   ├── arcs/
│   └── timeline.md
└── chapters/
    └── _index.md             # Chapter registry
```

## ⚙️ How It Works

Every story element is a markdown file with YAML frontmatter. Skills cross-reference each other to keep your story consistent:

- **`story.md`** is the top-level bible read by all skills
- Characters, locations, and arcs use **kebab-case identifiers** (e.g., `sera-voss`)
- **`_index.md`** files serve as registries for each domain
- Relationships and references are maintained **bidirectionally**

## 📖 Example

> Explore [`examples/the-last-ember/`](examples/the-last-ember/) — a complete fantasy story with three characters, two locations, a magic system, a plot arc with foreshadowing tracking, and a first chapter with full prose.

## 📄 License

[MIT](LICENSE)
