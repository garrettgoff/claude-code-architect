# Installation Guide

## Quick Start

The easiest way to test this plugin is to load it locally:

```bash
claude --plugin-dir /path/to/claude-code-architect
```

Once loaded, try:
```bash
/arch:init
```

## Installation Methods

### Method 1: Local Development (Recommended for testing)

```bash
git clone https://github.com/garrettgoff/claude-code-architect.git
cd claude-code-architect
claude --plugin-dir .
```

### Method 2: Git Installation (Project-specific)

Add to your project's `.claude/settings.json`:

```json
{
  "plugins": [
    {
      "type": "git",
      "url": "https://github.com/garrettgoff/claude-code-architect.git"
    }
  ]
}
```

Or use the command:
```bash
/plugin install git:https://github.com/garrettgoff/claude-code-architect.git
```

### Method 3: Global Installation

Add to your global Claude Code settings at `~/.claude/settings.json`:

```json
{
  "plugins": [
    {
      "type": "git",
      "url": "https://github.com/garrettgoff/claude-code-architect.git"
    }
  ]
}
```

## Verifying Installation

After installation, run:
```bash
/help
```

You should see the architecture commands listed:
- `/arch:init`
- `/arch:query`
- `/arch:workflow`

## Testing the Plugin

1. Navigate to a test project
2. Initialize architecture:
   ```bash
   /arch:init
   ```
3. Query the architecture:
   ```bash
   /arch:query Show me the structure
   ```

## Troubleshooting

### Plugin not found
- Verify the path is correct with `--plugin-dir`
- Ensure `.claude-plugin/plugin.json` exists
- Check Claude Code version is 1.0.33 or later

### Commands don't appear
- Restart Claude Code after installation
- Check that commands are in the `commands/` directory
- Verify files have `.md` extension

### Skills not working
- Skills should be in `skills/[skill-name]/SKILL.md`
- Check frontmatter has `name` and `description` fields
- Restart Claude Code to reload skills

## Updating

If using git installation:
```bash
/plugin update arch
```

If using local development:
```bash
cd claude-code-architect
git pull origin main
```
Then restart Claude Code.

## Uninstalling

Remove from your `settings.json` or stop using `--plugin-dir` flag.
