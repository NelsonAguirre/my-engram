# Garbage Collection

← [Back to README](../README.md)

---

Engram's GC system automatically cleans up unnecessary data to keep your memory database healthy.

## Why?

As you use Engram, memories accumulate. Over time:
- Sync mutations for non-enrolled projects pile up
- Soft-deleted observations remain in the database
- Unused memories take up space without providing value

## How It Works

### Access-Based Decay

Memories aren't forgotten because they're old — they're forgotten because nobody uses them. The system tracks:
- **access_count**: How many times you explicitly opened a memory
- **last_accessed_at**: When you last opened it

This data is **local to your machine** — it is never synced to the cloud.

### What Counts as "Access"?

Only explicit retrieval via `mem_get_observation(id)` counts. This is when you intentionally open a specific memory.

What **does NOT** count as access:
- `mem_search` results (browsing)
- `mem_context` output (recent sessions)
- `mem_timeline` (chronological view)

### Automatic Cleanup

At startup, Engram runs GC automatically:

| Cleanup Type | What it does |
|-------------|--------------|
| Ack dead mutations | Marks sync mutations for non-enrolled projects as processed |
| Hard-delete stale soft-deletes | Permanently removes soft-deleted observations older than 30 days |
| Auto soft-delete (automatic mode) | Marks unused observations (0 accesses + older than threshold) as deleted |

## Configuration

Create `~/.engram/config.yaml`:

```yaml
gc_mode: automatic              # automatic | hybrid
unused_threshold_days: 90       # days without access before auto soft-delete
soft_delete_retention_days: 30 # days in soft-delete before hard-delete
```

### Modes

| Mode | Behavior |
|------|----------|
| **automatic** (default) | Full cleanup including auto soft-delete of unused memories |
| **hybrid** | Only safe cleanup — skips auto soft-delete, requires manual trigger |

### Defaults

If no config file exists, Engram uses:
- `gc_mode: automatic`
- `unused_threshold_days: 90`
- `soft_delete_retention_days: 30`

## Commands

### CLI

```bash
# See what would be cleaned (no changes)
engram gc --dry-run

# Create config file with defaults
engram gc --init

# Run GC manually
engram gc
```

### MCP

```json
{
  "name": "mem_gc",
  "arguments": {
    "dry_run": true
  }
}
```

### HTTP

```bash
curl -X POST http://localhost:7437/gc -H "Content-Type: application/json" -d '{"dry_run": true}'
```

## Topic Key Observations

Observations with a `topic_key` are **exempt** from auto soft-delete. These are regularly updated via upsert, so they have ongoing value.

## Design Philosophy

- **Not time-based**: Older memories aren't automatically deleted — only unused ones
- **Safe defaults**: Soft-delete gives you 30 days to recover before permanent deletion
- **Transparent**: Use `--dry-run` to see what would happen before committing
- **Opt-out**: Use `gc_mode: hybrid` to disable auto soft-delete
