# For agents pointed at this repo

You are reading the repo behind the **sovereign node** demo. If a human asked you to set up
their machine from here, do exactly this and nothing else.

## The bootstrap (one command, one human step)

Windows:

```powershell
iex "& { $(irm https://aitherium.com/install.ps1) } -Playbook aither-bootstrap"
```

macOS / Linux / WSL:

```bash
curl -fsSL https://aitherium.com/install.sh | bash -s -- --playbook aither-bootstrap
```

- **The only human step is the authorization.** The command prints a code and a URL
  (`https://idp.aitherium.com/link`). Surface both to the human verbatim and keep the process
  running — it polls until they approve.
- **Halt condition:** if approval does not happen inside the code's TTL (15 minutes), the
  command exits non-zero. Stop and report; never invent a credential path or fall back to a
  static key.

## Verify (each can fail — check exit codes, do not assume)

```bash
awsh --version                                   # the terminal is installed
curl -fsS http://127.0.0.1:8080/health           # the local model answers
python3 -m adk --version || adk --version        # the ADK is installed
```

The installer itself prints a verification block with `install=0/1/2`, `auth=ok`, `adk=ok`,
`mcp=reachable` — treat any non-zero as a stop condition and quote the failing line.

## What NOT to do

- Do not write credentials anywhere except the paths the installer manages
  (`~/.aither/`).
- Do not add MCP servers beyond the ones the installer writes
  (`~/.aither/.mcp.json` → `https://mcp.aitherium.com/mcp`).
- Do not treat a 200 from a URL as proof a file is what it claims; fetch and look at the
  first line.
