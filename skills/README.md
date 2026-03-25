# AI Skills

This directory contains specialized AI skills and expertise domains. Each skill is organized in its own folder with a README.md file and any supporting resources.

## Directory Structure

```
skills/
├── README.md                          # This file
├── buffett-financial-analysis/       # Warren Buffett Financial Analysis
│   └── SKILL.md
├── housing-planning-uk/              # UK Housing & Planning Law
│   └── README.md
├── ig-trading-api/                   # IG Markets Trading API
│   └── README.md
├── suno-api/                         # Suno AI Music Generation API
│   └── SKILL.md
└── threejs/                          # Three.js 3D Graphics
    └── README.md
```

## Available Skills

### [buffett-financial-analysis/](./buffett-financial-analysis/)
Warren Buffett's framework for identifying companies with a durable competitive advantage, based on "Warren Buffett and the Interpretation of Financial Statements" by Mary Buffett and David Clark.

**Covers:**
- Income statement analysis (gross margins, SGA, R&D, net earnings, EPS trends)
- Balance sheet analysis (cash, debt, retained earnings, ROE, debt/equity)
- Cash flow analysis (capex ratio, buybacks, free cash flow)
- Equity bond valuation framework
- Summary scorecard and red flags checklist

**Use for:** Analyzing company financials, evaluating investment opportunities, assessing durable competitive advantages and moats

### [housing-planning-uk/](./housing-planning-uk/)
Expert knowledge of UK Housing and Planning law, specifically the Housing and Planning Act 2016.

**Covers:**
- New homes and starter homes legislation
- Rogue landlords and property agents regulations
- Banning orders and rent repayment orders
- Social housing provisions
- Planning in England
- Compulsory purchase procedures
- Right to buy implementation

**Use for:** Property development, landlord/tenant issues, planning applications, housing law queries

### [ig-trading-api/](./ig-trading-api/)
Expert knowledge of IG Markets Trading API for automated trading and financial applications.

**Covers:**
- REST API for trading, positions, orders, and account management
- Streaming API with Lightstreamer for real-time market data
- Authentication methods (session tokens and OAuth)
- Historical and live price data
- Watchlists and client sentiment
- Python and JavaScript code examples

**Use for:** Automated trading systems, trading bots, market data analysis, portfolio management applications, financial integrations

### [suno-api/](./suno-api/)
Expert knowledge of the Suno AI API for music generation, lyrics creation, audio processing, and video production.

**Covers:**
- Music generation (custom lyrics, description mode, instrumental)
- Audio manipulation (extend, cover, mashup, replace section, add vocals/instrumental)
- Lyrics generation and timestamped lyrics
- Audio processing (vocal separation, stem splitting, WAV conversion, MIDI export)
- Media creation (music videos, cover art)
- Personas, style boost, sound effects, file upload API
- Async callback patterns and polling

**Use for:** AI music generation, song creation, audio remixing, vocal separation, sound effects, music production integrations

### [threejs/](./threejs/)
Expert knowledge of Three.js, the JavaScript 3D graphics library for WebGL.

**Covers:**
- Core concepts (Scene, Camera, Renderer, Materials, Lights)
- All API categories (Cameras, Lights, Materials, Geometries, Objects, Loaders, Animation)
- Common patterns (raycasting, model loading, post-processing)
- Performance optimization
- Best practices for production

**Use for:** 3D web graphics, WebGL development, interactive visualizations, games, VR/AR

## Creating New Skills

To create a new skill:

1. Create a new folder in the `skills/` directory with a descriptive kebab-case name (e.g., `react-native/`)
2. Create a `README.md` file inside the folder as the main skill documentation
3. Structure the skill README with:
   - Clear title and description
   - Core concepts and fundamentals
   - API/feature reference
   - Common patterns and examples
   - Best practices
   - Resources and documentation links
4. (Optional) Add supporting files like examples, diagrams, or reference materials in the same folder
5. Commit and push to the repository

**Example:**
```bash
mkdir skills/my-new-skill
touch skills/my-new-skill/README.md
# Edit README.md with skill content
git add skills/my-new-skill/
git commit -m "Add my-new-skill"
```

### Skill Template

```markdown
# [Technology/Domain] Expert

You are an expert in [technology/domain name].

## Core Concepts

[Fundamental concepts that form the foundation]

## [Major Category 1]

[Details, API reference, examples]

## [Major Category 2]

[Details, API reference, examples]

## Common Patterns

[Frequently used patterns and code examples]

## Best Practices

[Recommended approaches and guidelines]

## Resources

[Official docs, tutorials, references]
```

## Using Skills

When interacting with Claude in this repository, Claude will have access to all skills in this directory. You can ask questions related to any skill domain, and Claude will provide expert guidance based on the skill documentation.

## Skill Guidelines

- **Focus**: Each skill should cover one specific technology or domain
- **Depth**: Provide comprehensive coverage of the subject
- **Examples**: Include practical code examples where applicable
- **Current**: Keep skills updated with latest versions and best practices
- **Structure**: Use clear markdown structure with headers and sections
- **Size**: Aim for comprehensive but focused content (typically 200-500 lines)
