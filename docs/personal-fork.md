# Personal fork installation

This fork combines upstream's account priorities with the optional pin integration
from [PR 210](https://github.com/realiti4/claude-swap/pull/210).
The `pin` extra fixes the companion package at `cswap-pin==0.1.338`.

Install a tested commit, rather than a moving branch:

```bash
uv tool install --force "claude-swap[pin] @ git+https://github.com/caciolai/claude-swap@<tested-commit>"
```

Use this same installed `cswap` for pinning and automatic rotation. Stop the old
auto-switch loop before starting its replacement. Installing does not change
the account store or require registering the accounts again.

From a plain terminal outside Claude Code, pin the cloud identity:

```bash
cswap pin work-prefrontal
cswap pin --state
```

Then run the existing priority policy:

```bash
cswap auto --strategy priority-consume-first --threshold 95
```

Set both work accounts to priority 100 and personal to 0 using their registered
aliases. Keep Prefrontal enabled in rotation. The pin chooses the cloud identity;
the auto-switch strategy chooses inference credentials. See
[automatic switching](auto-switch.md) for thresholds and priority semantics.

Before each new Claude launch, ensure the proxy is available:

```bash
cswap pin --ensure
claude --dangerously-skip-permissions
```

The companion is a local TLS interception proxy and adds its CA to Claude's
configuration. See its [operation and recovery instructions](https://github.com/codeslake/cswap-pin).
Existing sessions retain their startup environment; launch a fresh session to
use a new pin. End-to-end Remote Control and Artifact ownership require a live
Claude session and are not established by the host's unit tests.

To return to upstream, first run `cswap pin --clear` using this build. Only then
reinstall the desired upstream release. Clearing removes pin configuration but
does not stop a proxy that existing sessions may still need.
