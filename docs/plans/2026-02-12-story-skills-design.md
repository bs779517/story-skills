# Story Skills Plugin - Design Document

## Overview

A Claude Code plugin providing a suite of cross-referenced skills for end-to-end story writing, powered by markdown. Each story element (characters, world, plot, chapters) is managed as structured markdown files with YAML frontmatter, enabling Claude to maintain consistency across the entire creative process.

## Plugin Structure

```
story-skills/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    ├── story-init/
    │   └── SKILL.md
    ├── character-management/
    │   ├── SKILL.md
    │   └── references/
    │       ├── character-template.md
    │       └── relationship-types.md
    ├── worldbuilding/
    │   ├── SKILL.md
    │   └── references/
    │       ├── location-template.md
    │       ├── system-template.md
    │       └── world-element-types.md
    ├── plot-structure/
    │   ├── SKILL.md
    │   └── references/
    │       ├── arc-template.md
    │       └── structure-models.md
    └── chapter-writing/
        ├── SKILL.md
        └── references/
            ├── chapter-template.md
            └── writing-guidelines.md
```

## Story Project Layout

What `story-init` creates for each new story:

```
my-story/
├── story.md              # Story bible - title, genre, synopsis, themes, POV, tense
├── characters/
│   ├── _index.md         # Character registry, family trees, relationship map
│   └── character-name.md # Individual character profiles
├── worldbuilding/
│   ├── _index.md         # World overview + links
│   ├── locations/
│   │   └── location-name.md
│   └── systems/
│       └── system-name.md  # Magic, politics, technology, religion, etc.
├── plot/
│   ├── _index.md         # Story arc overview, act structure, theme tracking
│   ├── arcs/
│   │   └── arc-name.md
│   └── timeline.md       # Chronological event timeline across all arcs
└── chapters/
    ├── _index.md         # Chapter registry, status tracking
    └── chapter-01.md
```

### Conventions

- `_index.md` files serve as registries and cross-reference hubs
- `story.md` is the top-level bible that all skills read for context
- YAML frontmatter on every file for structured metadata (status, tags, relationships)
- Character references use kebab-case filenames (e.g., `sera-voss`)
- Cross-links between files are maintained bidirectionally

## Skill Designs

### 1. story-init

**Triggers:** "start a new story", "initialize a story project", "create a story", "new book"

**Workflow:**
1. Ask for basic story info (title, genre, synopsis, setting era)
2. Create full folder structure
3. Populate `story.md` with story bible template
4. Create all `_index.md` files with empty registries
5. Suggest next steps

**`story.md` frontmatter:**
```yaml
---
title: "The Last Ember"
genre: fantasy
sub-genre: epic
setting-era: medieval
status: planning
themes:
  - redemption
  - power and corruption
pov: third-person-limited
tense: past
---
```

**No references/ needed** - skill is self-contained with inline templates.

### 2. character-management

**Triggers:** "create a character", "update character", "add a character", "family tree", "character relationships", "character timeline", "character arc"

**Creating a character:**
1. Ask for name and role (protagonist, antagonist, supporting, minor)
2. Build rich profile through conversation
3. Write `characters/character-name.md` with structured frontmatter + prose
4. Update `characters/_index.md` registry

**Character frontmatter:**
```yaml
---
name: "Sera Voss"
role: protagonist
age: 28
status: alive
aliases:
  - "The Ember Queen"
relationships:
  - character: kael-voss
    type: sibling
  - character: lord-maren
    type: antagonist
tags:
  - magic-user
  - royal-bloodline
arc: redemption
---
```

**Profile sections:** Appearance, Personality & Traits, Backstory, Motivations & Goals, Voice & Speech Patterns, Character Arc, Timeline (key life events)

**Updating:** Reads existing file, makes targeted edits. Relationship additions update BOTH character files.

**Family trees:** Maintained in `characters/_index.md` as structured markdown linking character files.

**References:**
- `character-template.md` - full blank character template
- `relationship-types.md` - relationship type reference

### 3. worldbuilding

**Triggers:** "create a location", "add a location", "magic system", "political system", "build the world", "add culture", "world history", "technology system"

**Locations** (`worldbuilding/locations/location-name.md`):
```yaml
---
name: "The Ashen Citadel"
type: city
region: Northern Reach
population: ~12,000
controlled-by: lord-maren
notable-characters:
  - sera-voss
  - kael-voss
tags:
  - fortified
  - magical-nexus
---
```
Sections: Description, History, Culture & Customs, Notable Features, Current State

**Systems** (`worldbuilding/systems/system-name.md`):
```yaml
---
name: "Ember Magic"
type: magic
prevalence: rare
---
```
Sections: Overview, Rules & Limitations, History, Practitioners, Impact on Society

**Cross-referencing:** When creating elements that mention existing characters or locations, links are added to both files. Reads `story.md` for genre/era tonal consistency.

**References:**
- `location-template.md`
- `system-template.md`
- `world-element-types.md` - catalogue of system types with prompts for what to flesh out

### 4. plot-structure

**Triggers:** "create a plot arc", "story structure", "add a plot point", "story timeline", "track foreshadowing", "pacing", "act structure", "story arc"

**Story arc overview** (`plot/_index.md`): Act structure, arc registry with status, theme tracking.

**Individual arcs** (`plot/arcs/arc-name.md`):
```yaml
---
name: "Sera's Reclamation"
type: main
status: in-progress
characters:
  - sera-voss
  - lord-maren
themes:
  - redemption
  - power-and-corruption
acts:
  - act-1
  - act-2
---
```
Sections: Setup, Rising Action, Climax, Resolution, Plot Points (with chapter refs), Foreshadowing (planted + payoff tracking)

**Timeline** (`plot/timeline.md`): Chronological event table linking arcs and chapters.

| When | Event | Arc | Chapter |
|------|-------|-----|---------|
| Prologue | Ashen Citadel falls | sera-reclamation | ch-01 |

**Cross-referencing:** Plot points link to characters and locations. Reads `story.md` themes. Foreshadowing tracker flags unresolved plants during chapter writing.

**References:**
- `arc-template.md`
- `structure-models.md` - common story structures (three-act, hero's journey, save the cat, kishotenketsu) with beat sheets

### 5. chapter-writing

**Triggers:** "write a chapter", "next chapter", "chapter outline", "draft chapter", "continue the story", "write a scene"

**Outline-first workflow:**
1. **Gather context** - Read `story.md`, `plot/_index.md`, `chapters/_index.md`, relevant arc files
2. **Ask what happens** - Or suggest next logical beats from plot arcs
3. **Build outline** - Beat-by-beat with POV, location, arc beats advanced
4. **Get approval** - User reviews/adjusts outline
5. **Write chapter** - Full prose pulling in character voice, location details, tonal consistency
6. **Post-write updates:**
   - Save to `chapters/chapter-NN.md`
   - Update `chapters/_index.md`
   - Update `plot/timeline.md` with new events
   - Flag foreshadowing planted or paid off
   - Note character state changes

**Chapter frontmatter:**
```yaml
---
title: "The Ember Wakes"
number: 7
pov: sera-voss
locations:
  - ashen-citadel
characters:
  - sera-voss
  - kael-voss
arcs-advanced:
  - sera-reclamation
status: draft
word-count: 3200
---
```

**Cross-referencing in action:**
- Reads POV character profile for voice, personality, arc state
- Reads location files for setting descriptions
- Reads arc files for beats to hit
- Checks timeline for chronological consistency
- Reviews previous chapter for continuity

**References:**
- `chapter-template.md`
- `writing-guidelines.md` - prose style, show-don't-tell, pacing, dialogue formatting

## Cross-Referencing Strategy

All skills share these conventions:
- Read `story.md` for top-level context (genre, themes, tense, POV)
- Character references use kebab-case filenames as identifiers
- When creating/updating any element that references another, update both files
- `_index.md` files are the authoritative registries for each domain
- YAML frontmatter enables structured queries (e.g., find all characters tagged `magic-user`)

## Build Order

Skills should be implemented in this order due to dependencies:
1. `story-init` (no dependencies, creates the foundation)
2. `character-management` (depends on story-init structure)
3. `worldbuilding` (depends on story-init, cross-refs characters)
4. `plot-structure` (depends on story-init, cross-refs characters + world)
5. `chapter-writing` (depends on all of the above)
