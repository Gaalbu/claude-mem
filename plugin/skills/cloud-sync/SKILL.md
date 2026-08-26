---
name: cloud-sync
description: Configure or inspect optional self-hosted cloud sync for claude-mem.
---

# Optional self-hosted cloud sync

Cloud sync is disabled unless `CLAUDE_MEM_CLOUD_SYNC_TOKEN`,
`CLAUDE_MEM_CLOUD_SYNC_USER_ID`, and `CLAUDE_MEM_CLOUD_SYNC_HUB_URL` are all
explicitly configured. Local SQLite and Chroma do not require this skill.

Only use a sync hub operated by the user or their organization. Never request,
infer, or transmit credentials to a third-party hosted service. Keep tokens in
`~/.claude-mem/settings.json` with owner-only permissions and never print them.

To inspect status, call the local worker's `/api/sync/status` endpoint. If sync
is not configured, report that it is intentionally off. When configured, make
the authenticated, read-only `GET /v1/sync/status` request to the configured
hub and inspect its reachability: `hub.reachable: true` is the only successful
result; `hub.reachable: false` means the sync is unavailable and must never be
reported as working. This
probe never appends or advances sync state. To enable sync, ask the user for
the three values from their self-hosted hub, write them to the local settings
file, restart the worker, and verify the local status endpoint and hub probe.
