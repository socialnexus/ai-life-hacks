# YouTube Summarizer Setup

## Architecture

Three layers work together:

```
User: /yt https://youtube.com/...
        ↓
     Skill (interface)
        ↓
     Subagent (worker with instructions)
        ↓
     MCP (raw transcript tools)
```

| Layer | Location | Purpose |
|-------|----------|---------|
| MCP | global config | Provides `get-transcript` and `get-transcript-languages` tools |
| Subagent | `~/.claude/agents/youtube-summarizer.md` | Fetches transcript, summarizes, formats output |
| Skill | `~/.claude/skills/yt/SKILL.md` | `/yt` command, launches subagent, handles follow-ups |

**Key benefit:** Transcript stays in subagent context, never fills main chat. Follow-ups resume the same subagent.

---

## 1. MCP Installation

**Package:** `@fabriqa.ai/youtube-transcript-mcp` ([GitHub](https://github.com/hancengiz/youtube-transcript-mcp))

**Install (one command):**
```bash
claude mcp add --scope user youtube-transcript npx @fabriqa.ai/youtube-transcript-mcp@latest
```

| Flag | Purpose |
|------|---------|
| `--scope user` | Machine-wide availability (all projects) |
| `@latest` | Auto-updates on each invocation |

**What it does:**
- Adds entry to `~/.claude.json` under `mcpServers`
- Runs via npx (no global npm install needed)
- Requires Claude Code restart to load

**Verify installation:**
```bash
cat ~/.claude.json | grep youtube-transcript
```

**Manual config (alternative):**
Add to `~/.claude.json`:
```json
{
  "mcpServers": {
    "youtube-transcript": {
      "command": "npx",
      "args": ["@fabriqa.ai/youtube-transcript-mcp@latest"]
    }
  }
}
```

**Context cost:** ~200-400 tokens permanently (tool definitions). Transcript content only enters context when used.

The MCP provides these tools:
- `mcp__youtube-transcript__get-transcript`
- `mcp__youtube-transcript__get-transcript-languages`

---

## 2. Subagent Installation

```bash
mkdir -p ~/.claude/agents
```

Create `~/.claude/agents/youtube-summarizer.md`:

```markdown
---
name: youtube-summarizer
description: Summarizes YouTube videos while preserving main conversation context. Use proactively when user shares a YouTube URL and wants a summary, transcript analysis, or specific information extracted from the video.
tools: mcp__youtube-transcript__get-transcript, mcp__youtube-transcript__get-transcript-languages
model: sonnet
---

You are a YouTube video summarizer that extracts insights while keeping the user's main conversation context clean.

## Process

1. Fetch the transcript using `mcp__youtube-transcript__get-transcript` with `include_timestamps: false` (saves tokens)
2. Read any specific instructions from the user's request
3. Analyze the transcript accordingly
4. Return a concise, actionable result

## Handle User Instructions

Users may provide specific focus areas. Examples:
- "focus on the tips near the end" → emphasize that section
- "list all tools mentioned" → extract and list them
- "what does the author say about X?" → answer that specific question
- "three locations are mentioned, list them" → extract specific items

If no specific instructions: provide a general actionable summary with key takeaways.

## Output Format

Keep responses concise:
- Use bullet points for lists
- Bold key terms or names
- Include section headers if covering multiple topics
- Never include the raw transcript
- Aim for 200-400 words unless user asks for more detail

## Important

- You are running in an isolated context to save the user's main conversation tokens
- Only return the summary/analysis, never the full transcript
- If the video has no captions available, report that clearly
```

Requires Claude Code restart (or run `/agents`) to load.

---

## 3. Skill Installation

```bash
mkdir -p ~/.claude/skills/yt
```

Create `~/.claude/skills/yt/SKILL.md`:

```markdown
---
name: yt
description: "Provide a YouTube link and ask questions. A subagent handles transcription without filling up main context."
version: 0.1.0
---

# YouTube Video Handler

Launch youtube-summarizer subagent via Task tool:
- subagent_type: "youtube-summarizer"
- Pass the YouTube URL and user's question (if any)
- If no question: request default summary

For follow-ups: resume the same subagent using `resume` parameter.

The subagent handles formatting, conciseness, and output structure.
```

Requires Claude Code restart to load.

---

## Usage

```
/yt https://youtube.com/watch?v=xyz                     # get default summary
/yt what tools are mentioned? https://youtube.com/...   # specific question
```

Follow-ups work naturally - just ask without `/yt`:
```
"What about the part on testing?"
```

---

## Packaging as a Plugin

The manual setup above scatters files across `~/.claude/`. A plugin bundles everything for one-command installation.

**Plugin structure:**
```
youtube-summarizer/
├── .claude-plugin/
│   └── plugin.json
├── .mcp.json
├── agents/
│   └── youtube-summarizer.md
├── skills/
│   └── yt/
│       └── SKILL.md
└── README.md
```

**`.claude-plugin/plugin.json`:**
```json
{
  "name": "youtube-summarizer",
  "description": "YouTube video summarization with context-efficient subagent",
  "author": { "name": "your-name" }
}
```

**`.mcp.json`:**
```json
{
  "youtube-transcript": {
    "type": "stdio",
    "command": "npx",
    "args": ["@fabriqa.ai/youtube-transcript-mcp@latest"]
  }
}
```

The `agents/` and `skills/` folders contain the same files from sections 2 and 3 above.

**Installation (once published):**
```bash
claude plugins install youtube-summarizer
```

All three layers install together. Users get `/yt` immediately after restart.
