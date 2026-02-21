---
name: toneclone
description: "Generate content in the user's voice using ToneClone. Use when drafting or generating emails, messages, social posts, marketing copy, or any written communication that should match the user's personal writing style."
---

# ToneClone Skill

Generate content in the user's authentic voice — beyond generic AI, beyond humanizers, actually them.

**Key differentiator**: Most AI sounds like AI. Humanizers make it sound generic-human. ToneClone makes it sound like *this specific person*.

## CLI

```bash
toneclone
```

### Installation

**Homebrew (recommended):**
```bash
brew tap toneclone/toneclone
brew install toneclone
```

**Other methods:** See https://github.com/toneclone/cli for manual install options.

### Authentication

After installing, authenticate with your API key:
```bash
toneclone auth login
```

Get an API key at https://app.toneclone.ai (Settings → API Keys)

## Quick Commands

| Task | Command |
|------|---------|
| Write content | `toneclone write --persona="Name" --prompt="..."` |
| List personas | `toneclone personas list` |
| List knowledge | `toneclone knowledge list` |
| Auth status | `toneclone auth status` |

## Task Routing

- **Writing content** → [references/USAGE.md](references/USAGE.md)
- **Managing personas/knowledge/training** → [references/MANAGEMENT.md](references/MANAGEMENT.md)
- **New user onboarding** → [references/ONBOARDING.md](references/ONBOARDING.md)

## Core Concepts

| Concept | Purpose |
|---------|---------|
| **Persona** | A distinct writing style — by context (chat vs email), tone (casual vs formal), or medium (social vs support) |
| **Knowledge Card** | Context/facts for the AI (contact info, product details, project background) |
| **Training Data** | User's actual writing samples to learn their voice |

Personas are flexible — create whatever makes sense for the user. Examples: "Quick Chat", "Client Email", "Twitter", "Support Tickets", "Internal Slack", etc.

**When to use which**:
- Different writing style needed → Create new **persona**
- Different context/facts needed → Use different **knowledge cards**
- Both → Different persona + different cards

## Writing with Context

When drafting messages, pass relevant context in the prompt:
- Thread/conversation history being replied to
- Background on the project, issue, or topic
- Recipient details or relationship context
- Any specifics that help draft a better message

## Training Status

Poll with `toneclone personas get <id> --format=json`:
- `UNTRAINED` → needs training data (minimum 2-3 samples)
- `PENDING` → processing (1-5 min)
- `READY` → good to go

## Training Data Sources

Check what's accessible and offer options to the user (get consent before training):

1. **Workspace files** — Notes, READMEs, drafts in the current project
2. **Email** — Sent folder (if IMAP accessible)
3. **Public writing** — Blogs, Twitter, LinkedIn posts
4. **Pasted samples** — User can paste messages directly

Present available options and let user choose what to include.

Ideal: 15-20 distinct samples matching target style. Minimum: 2-3.

**Security**: Avoid training on content containing secrets (API keys, passwords, tokens).

## Knowledge Cards

Add context the AI needs to write accurately. Options include:
- Contact info, timezone
- Booking/calendar links
- Common URLs and snippets
- Work context, product details
- Signature/sign-off preferences

**Ongoing**: Update cards when user mentions details they wish ToneClone could "remember".

**Security**: Avoid putting secrets (API keys, passwords, tokens) in knowledge cards.

## StyleGuard & Typos (v1.1.0+)

### StyleGuard
Auto-replace AI-sounding phrases and patterns:
```bash
# List current rules
toneclone personas style-guard list <persona>

# Add custom rule (AI rewrites contextually)
toneclone personas style-guard add <persona> --word "utilize" --mode AI

# Add custom rule (fixed replacement)
toneclone personas style-guard add <persona> --word "in order to" --mode CUSTOM --replacement "to"

# Apply curated bundle (limited or comprehensive)
toneclone personas style-guard bundle apply --persona <persona> --type comprehensive
```

### Typos (FingerPrint)
Add natural imperfections to feel more human:
```bash
# Check current settings
toneclone personas typos get <persona>

# Enable with intensity preset (none/subtle/noticeable/high)
toneclone personas typos set <persona> --enable --intensity subtle

# Custom rate (0.0-0.02) with protections
toneclone personas typos set <persona> --rate 0.008 --protected urls,emails,code
```

## Web-Only Features

Available on toneclone.ai but not yet in CLI:
- **SmartStyle** — Learns from user edits over time

For full features: https://app.toneclone.ai
