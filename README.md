# Sovereign Node — Agents, Everywhere

**AI Tinkerers LA · "Agents, Everywhere: Bots, Channels & More" · 2026-09-12**

One paste turns a blank laptop into a working AI node: a **local model** that answers even
with the internet down, the agent toolchain, and a live connection to a **shared workspace**
where a teammate and a team of agents collaborate in the same room.

## The one paste

Windows (PowerShell):

```powershell
iex "& { $(irm https://aitherium.com/install.ps1) } -Playbook aither-bootstrap"
```

macOS / Linux / WSL:

```bash
curl -fsSL https://aitherium.com/install.sh | bash -s -- --playbook aither-bootstrap
```

What happens, in order:

1. **Device-flow sign-in** — a code appears; you approve it on your phone. No password, no API key.
2. **A local model.** Bonsai (ternary-quantized) is sized to your RAM — 1.7B / 4B / 8B / 27B —
   and served on loopback. Weights come from our own mirror (`weights.aitherium.com`) first;
   a third-party source is an explicit, loud fallback.
3. **The agent toolchain** — `awsh` (terminal), `awdk` (Python ADK), wired to the MCP gateway.
4. **Verification** — every step probes a real surface and fails loudly with a named reason.

Everything a step needs is fetched from the same door that serves the installer; nothing is
pre-staged, and the node keeps answering when the home fleet does not.

## The team half

The owner mints a **bootcode** for a teammate. The teammate opens the link, is admitted with a
member role (not admin), and lands in the shared workspace — same room as the agents, same
files, same chat. Agents and humans collaborate side by side; the teammate's own tools can
plug in through the same MCP gateway.

## Why "sovereign"

- **The model is local.** Pull the network cable mid-demo and the node still answers. That is
  the point, not an excuse.
- **The weights are ours.** The mirror serves the exact quant files the installer requests;
  a mirror that carries different filenames than the installer asks for is a silent fallback
  to someone else's servers, and we treat that as a defect.
- **The cloud is optional.** The fleet adds reach (workspace, agents, tunnel); the node
  stands alone without it.

## Verify it yourself

```bash
# the installer reached a real playbook (not "found" and then bash syntax error)
curl -fsSL https://aitherium.com/playbooks/aither-bootstrap.sh | head -1

# the door is live
curl -o /dev/null -w "%{http_code}\n" https://aitherium.com/install.sh          # expect 200

# the weight lane serves the names the installer requests
curl -o /dev/null -w "%{http_code}\n" -r 0-0 https://weights.aitherium.com/Bonsai-4B-Q1_0.gguf   # expect 206
```

## Built on

The `aw*` stack: `awsh` (terminal), `awdk` (ADK), `awnode`, `awconnect`, the desk overlay,
and the awnix OS image lineage. Apache/MIT-licensed pieces kept where they belong.

## License

MIT — see [LICENSE](./LICENSE).
