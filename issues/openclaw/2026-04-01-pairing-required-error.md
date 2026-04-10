## What Happened (Context for the Future)

### Problem: "pairing required" Error

When OpenClaw starts on a new machine or after a reinstall, the gateway generates a fresh pairing token. Your existing session (stored on the old machine or in the old config) doesn't know about this new token, so every command fails with:

```
Exec approval registration failed: Error: gateway closed (1008): pairing required
Gateway target: ws://127.0.0.1:18789
Source: local loopback
Config: /home/claw/.openclaw/openclaw.json
Bind: loopback
```

This is a security feature — the gateway is correctly rejecting an unauthenticated client.

**Fix:** Run `openclaw devices list` to see pending pairing requests, then `openclaw devices approve [request ID]` to approve the session.

---

### Problem: "chat exec approvals are not enabled on Telegram"

Once pairing is fixed, exec commands work but require approval from the Web UI or terminal — Telegram doesn't support them natively.

**Fix:** Enable Telegram as an exec approval client:

```bash
openclaw config set channels.telegram.execApprovals.enabled true
openclaw gateway restart
```

This sends approve/deny buttons directly to your Telegram DMs instead of requiring the Web UI.

---

### Problem: "Error: Config validation failed: Unrecognized key: exec"

Early attempts to fix the approval issue tried `openclaw config set exec.ask off` — but the key `exec.ask` **does not exist** in the OpenClaw config schema. The correct approach is `channels.telegram.execApprovals.enabled` (see above).

This was discovered on Fedora KDE, OpenClaw version ~2026.4.1.

---

### Problem: Security Mode Blocks Dangerous Commands

Even with approvals sorted, `tools.exec.security` defaults to `allowlist` mode, which blocks potentially dangerous commands like `rm -rf`.

**Fix:**

```bash
openclaw config set tools.exec.security full
openclaw gateway restart
```

---

## Commands Reference

### Pairing & Gateway

```bash
# List pending device approvals
openclaw devices list

# Approve a pending device
openclaw devices approve [request ID]

# Restart gateway (required after config changes)
openclaw gateway restart
```

### Config Setup (in order)

```bash
# 1. After pairing is accepted — enable full exec security
openclaw config set tools.exec.security full
openclaw gateway restart

# 2. Enable Telegram exec approvals (get buttons in Telegram DMs)
openclaw config set channels.telegram.execApprovals.enabled true
openclaw gateway restart
```

---

## Environment Details (for version awareness)

| Item | Detail |
|------|--------|
| OS | Fedora KDE (previously Kali Linux) |
| OpenClaw Version | ~2026.4.1 |
| Config File | `/home/claw/.openclaw/openclaw.json` |
| Gateway Port | 18789 |
| Gateway Bind | loopback |
| Session Storage | `/home/claw/.openclaw/agents/` |

---

## Notes

- `openclaw devices list` shows pending pairing requests — approve with `openclaw devices approve [request ID]`
- `tools.exec.security full` is needed for destructive commands like `rm -rf`
- `channels.telegram.execApprovals.enabled true` routes exec approvals to Telegram instead of Web UI
- Always run `openclaw gateway restart` after config changes
- Config keys change between versions — if something doesn't work, check the docs at `/usr/local/lib/node_modules/openclaw/docs/gateway/configuration-reference.md` or `openclaw config --help`
