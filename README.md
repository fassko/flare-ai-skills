# Flare FAssets — Agent Skill

A shared Agent Skill for **Cursor**, **Claude Code**, and other [skills.sh](https://skills.sh/)-compatible agents. Provides domain knowledge and guidance for **Flare FAssets**: wrapped tokens (FXRP, FBTC, FDOGE, etc.), minting, redemption, agents, collateral, and smart contract integration on the Flare network. Same `SKILL.md` format works across tools.

## What's in the skill

- **FAssets overview:** Trustless bridge, FTSO/FDC, collateral model  
- **Participants:** Agents, users, collateral providers, liquidators, challengers  
- **Workflows:** Minting (reserve → pay → FDC proof → execute) and redemption; Core Vault  
- **Contracts:** Flare mainnet addresses and integration outline (AssetManager, FXRP, etc.)  
- **Reference:** Links to [Flare Developer Hub](https://dev.flare.network/fassets/overview/) docs and APIs  

The agent uses this skill when you work with FAssets, FXRP, FBTC, minting/redemption, Flare DeFi, or FAssets contracts and APIs.

## How to Use This Skill

### Option A: Using skills.sh (recommended)

Install this skill with a single command:

```bash
npx skills add https://github.com/fasssko/fassets-skill --skill flare-fassets-skill
```

For more information, visit the [skills.sh platform page](https://skills.sh/fasssko/fassets-skill/flare-fassets-skill).

Then use the skill in your AI agent, for example:

> Use the flare-fassets skill and explain how to mint FXRP step by step.

> Use the flare-fassets skill and list the main FAssets contracts on Flare mainnet.

### Option B: Claude Code Plugin

**Personal Usage**

To install this Skill for your personal use in Claude Code:

1. Add the marketplace:
   ```
   /plugin marketplace add fasssko/fassets-skill
   ```
2. Install the Skill:
   ```
   /plugin install flare-fassets@flare-fassets-skill
   ```

**Project Configuration**

To automatically provide this Skill to everyone working in a repository, configure the repository's `.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "flare-fassets@flare-fassets-skill": true
  },
  "extraKnownMarketplaces": {
    "flare-fassets-skill": {
      "source": {
        "source": "github",
        "repo": "fasssko/fassets-skill"
      }
    }
  }
}
```

When team members open the project, Claude Code will prompt them to install the Skill.

### Option C: Manual install

1. Clone this repository.
2. Install or symlink the **`flare-fassets-skill/`** folder following your tool's official skills installation docs (see links below).
3. Use your AI tool as usual and ask it to use the "flare-fassets" skill for FAssets, FXRP, minting, redemption, or Flare DeFi tasks.

**Where to Save Skills**

Follow your tool's official documentation; here are a few popular ones:

- **Codex:** [Where to save skills](https://code.claude.com/docs/en/skills#where-skills-live)
- **Claude Code:** [Using Skills](https://code.claude.com/docs/en/skills)
- **Cursor:** [Enabling Skills](https://docs.cursor.com/context/rules-for-ai#skills) (or `.cursor/skills/` in your project / `~/.cursor/skills/` for all projects)

**How to verify**

Your agent should reference the workflow and concepts in `flare-fassets-skill/SKILL.md` and jump into `reference.md` for Flare Developer Hub links when you ask about FAssets, FXRP, minting, redemption, or contracts.

## Repository layout

```
fassets-skill/
├── README.md              # This file
├── LICENSE                # MIT
├── .gitignore
└── flare-fassets-skill/   # The skill to copy into your agent's skills path
    ├── SKILL.md           # Main skill instructions
    └── reference.md       # Flare Developer Hub links
```

Only the **`flare-fassets-skill`** folder (with `SKILL.md` and `reference.md`) needs to live inside the tool's skills path.

## Requirements

- **Cursor:** [Cursor](https://cursor.com) with Agent/Skills support  
- **Claude Code:** Node.js 18+, `npm install -g @anthropic-ai/claude-code`, then run `claude` in your project  
- No extra runtime dependencies; the skill is markdown only  

## Links

- [Install via skills.sh](https://skills.sh/fasssko/fassets-skill/flare-fassets-skill) — one-command install for Cursor, Claude Code, Codex, and more  
- [Flare FAssets Overview](https://dev.flare.network/fassets/overview/)  
- [Flare Developer Hub](https://dev.flare.network/)  
- [Flare Network](https://flare.network/)  

## License

MIT — see [LICENSE](LICENSE).
