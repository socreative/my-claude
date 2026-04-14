# My Claude — Plugin Marketplace

A Claude Code plugin marketplace with specialized AI skills for trading, finance, music, 3D graphics, and more.

## Installation

```bash
/plugin marketplace add socreative/my-claude
```

Then install individual plugins:

```bash
/plugin install buffett-financial-analysis@socreative-skills
```

## Available Plugins

| Plugin | Description |
|--------|-------------|
| [buffett-financial-analysis](plugins/buffett-financial-analysis/) | Warren Buffett's framework for evaluating durable competitive advantages from financial statements |
| [bulkowski-chart-patterns](plugins/bulkowski-chart-patterns/) | Chart pattern identification and statistics-based trading tactics from Bulkowski's Encyclopedia of Chart Patterns |
| [housing-planning-uk](plugins/housing-planning-uk/) | UK Housing and Planning Act 2016 — property law, landlord regulations, planning applications |
| [ig-trading-api](plugins/ig-trading-api/) | IG Markets Trading API for automated trading, market data, and portfolio management |
| [suno-api](plugins/suno-api/) | Suno AI API for music generation, lyrics, audio processing, and video production |
| [technical-analysis-murphy](plugins/technical-analysis-murphy/) | Technical analysis framework from John J. Murphy — price charts, indicators, trend analysis |
| [threejs](plugins/threejs/) | Three.js 3D graphics library for WebGL development |
| [uk-corporation-tax](plugins/uk-corporation-tax/) | UK Corporation Tax Act 2010 — rates, reliefs, group relief, loss rules, banking surcharge, oil ring fence |
| [uk-ct600-filing](plugins/uk-ct600-filing/) | Practical CT600 filing guide for small/micro companies — Alphatax Cloud, expense categorisation, tax computation, capital allowances |

## Plugin Structure

```
plugins/<name>/
├── .claude-plugin/
│   └── plugin.json       # Plugin manifest (name, description, version)
└── skills/
    └── <name>/
        └── SKILL.md      # Skill definition with YAML frontmatter
```

## Creating a New Plugin

1. Create the directory structure: `plugins/<name>/.claude-plugin/` and `plugins/<name>/skills/<name>/`
2. Add a `plugin.json` with name, description, and version
3. Add a `SKILL.md` with YAML frontmatter (`name`, `description`) and skill content
4. Add the plugin to `.claude-plugin/marketplace.json`
