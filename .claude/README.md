# Claude Configuration

This directory contains configuration and customization for Claude Code and AI tools in this repository.

## Files

### `settings.json`
Main configuration file for Claude Code. Controls behavior, model selection, and features.

**Key settings:**
- `model`: AI model to use (`sonnet`, `opus`, `haiku`)
- `autoSummarize`: Automatically summarize long conversations
- `ignorePatterns`: Files/directories to exclude from operations
- `hooks`: Custom shell commands triggered by events
- `customInstructions`: Project-specific instructions for Claude

### `.claudeignore` (in repo root)
Similar to `.gitignore`, specifies files/patterns that Claude should not read or modify.

## Directory Structure

```
.claude/
├── settings.json          # Main configuration
├── README.md             # This file
└── prompts/              # Custom prompts (if needed)

skills/                   # AI skills and expertise (root level)
├── housing-planning-uk.md
├── threejs.md
└── (additional skills...)
```

## Customization

### Adding Custom Instructions
Edit the `customInstructions` field in `settings.json` to provide project-specific context:

```json
{
  "customInstructions": "This is a React/TypeScript project using Next.js. Always use functional components with hooks. Follow the existing code style in src/."
}
```

### Setting Up Hooks
Hooks execute shell commands in response to events:

```json
{
  "hooks": {
    "beforeWrite": "npm run lint",
    "afterWrite": "npm run format"
  }
}
```

### Creating Custom Prompts
Add `.md` files to the `.claude/prompts/` directory for Claude Code-specific prompts.

### Adding Skills
Skills (specialized AI expertise) should be added to the `/skills/` directory at the repository root. See `/skills/README.md` for details.

## Best Practices

1. **Version control**: Commit `.claude/settings.json` and `.claudeignore` to share team settings
2. **Sensitive data**: Never put API keys or secrets in settings files
3. **Ignore patterns**: Keep `.claudeignore` updated as your project grows
4. **Custom instructions**: Use for project-specific conventions and patterns
5. **Model selection**: Use `haiku` for simple tasks, `sonnet` for most work, `opus` for complex tasks

## Documentation

For more information, visit:
- [Claude Code Documentation](https://docs.anthropic.com/claude/docs/claude-code)
- [Settings Reference](https://github.com/anthropics/claude-code)
