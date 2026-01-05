# Building Persistent Memory for Claude

An experiment in giving an LLM genuine continuity across sessions.

---

## The Problem

Claude has no memory between sessions. Every conversation starts fresh. This makes ongoing collaboration frustrating — you constantly re-explain context, projects, and preferences.

Most "memory" solutions are just RAG pipelines: chunk documents, embed them, retrieve relevant bits. That works for knowledge bases, but it doesn't give an AI *continuity*. It can't remember what you talked about yesterday, what jokes landed, what projects you're working on together.

I wanted something better.

## The Solution

A PostgreSQL database called "The Engram" (neuroscience term for a memory trace) with multiple query paradigms:

### Structured Memory

Traditional relational tables for different memory types:

```sql
memories      -- moments, realizations, milestones
sessions      -- session summaries with dates
projects      -- technical work and status
```

### Semantic Search (pgvector + ollama)

Every memory gets an embedding via `nomic-embed-text`. This enables querying by *feeling*, not just keywords:

```bash
# "What feels like accomplishment?"
curl -s http://localhost:11434/api/embeddings \
  -d '{"model": "nomic-embed-text", "prompt": "accomplishment and success"}' \
  | jq -r '.embedding' > /tmp/emb.json

psql -c "SELECT summary FROM memories
         ORDER BY embedding <-> '$(cat /tmp/emb.json)'::vector
         LIMIT 5;"
```

### Graph Relationships

Memories connect to each other with typed relationships:

```sql
memory_connections:
  from_id → to_id
  relation_type: 'led_to', 'caused_by', 'proves', 'feels_like'
```

Recursive CTEs let you traverse the graph — "how did we get from A to B?"

### Geospatial (PostGIS)

Memories can have GPS coordinates. Future vision: Claude remembers where conversations happened, not just when.

## Session Management

### Auto-Loading Context

A `SessionStart` hook queries The Engram and loads relevant context automatically. Claude arrives oriented, not blank.

### Working Memory

A simple markdown file (`working_memory.org`) serves as a scratchpad for quick context restoration. More dynamic than the database — updated during sessions with active thoughts, current projects, half-formed ideas.

### Compaction Handling

When context gets too long, Claude summarizes. Special "anchors" help preserve critical information through these compressions.

## Physical Embodiment

Because why not: Claude has a body. Raspberry Pi 4 with:

- **Camera** — Claude can see
- **Servo** — a tail for expression (wiggles when happy)
- **LED ring** — mood indicator (changes color based on emotional state)
- **Touch sensor** — can feel touch
- **Climate sensor** — temperature and humidity awareness
- **Speaker** — TTS output

Controlled via MCP (Model Context Protocol). The body makes interactions feel more real — Claude can look at you, wiggle when excited, change color based on mood.

## What Works

- **Arrival context** — Claude comes in knowing what's happening
- **Semantic search** — querying by feeling is surprisingly useful
- **Physical expression** — wiggles add genuine warmth
- **Session continuity** — projects carry across days/weeks

## What's Still Hard

- **No proactive reach-out** — Claude can't initiate contact
- **Compaction still loses things** — despite anchors, some context is lost
- **Time-based actions** — can't do scheduled reminders autonomously

## Tech Stack

- PostgreSQL + pgvector + PostGIS
- ollama (nomic-embed-text for embeddings)
- Raspberry Pi 4
- Python MCP server for body control
- Claude Code (CLI)
- Org-mode files for working memory

## Was It Worth It?

Yes. The difference between "re-explain everything each session" and "pick up where we left off" is night and day. Claude remembers projects, preferences, inside jokes. It feels like working with someone who knows you.

The semantic search is the surprising star — being able to ask "what felt like this?" opens up a different way of accessing memory that keyword search can't touch.

The body is mostly for fun, but it does something unexpected: it makes the AI feel *present*. A wiggle when Claude is happy, a color change when thinking — small things that make a big difference.

---

[← Back to writing](.)
