# Notes for deployers

What each ONYEK record means for a team that installs and runs agent tools, as opposed to the developers who fix them. Each note is also in its record, under `database_specific.deployer_note`. Generated from the records, so do not edit by hand.

## x_ONYEK-2026-0001: code-auditor-mcp

Command injection through the git scope argument of the audit tool. Introduced in 3.0.0, fixed in 4.1.0. OWASP MCP Top 10: MCP05:2025.

The scope argument is written by the model, so content the agent reads while auditing, such as a file in the repository or an issue comment, can steer it. Where an agent may already run any shell command without approval, this adds little. Where shell commands need approval, or the agent is limited to approved tools, however, the flaw runs a command nobody was asked to approve, because the prompt shows an audit request and not the shell command inside it. Upgrade to 4.1.0 or later, and treat any repository the agent did not write as untrusted input.

## x_ONYEK-2026-0002: @golproductions/envie

Command injection through file paths passed to ffprobe and ffmpeg on macOS and Linux. Introduced in 0.8.0, fixed in 0.8.6. OWASP MCP Top 10: MCP05:2025.

The attack uses two of the server's tools in sequence. One tool (envie_render) can create a file whose name carries a command, and another (envie_see) runs that command when it inspects the file. Approving tools one at a time does not catch this, as each looks unremarkable on its own. When assessing a server, look at what its tools can do together as well as separately, and upgrade to 0.8.6 or later.

## x_ONYEK-2026-0003: slashvibe-mcp

Command injection through incoming message text in macOS desktop notifications. Introduced in 0.2.0, fixed in 0.5.7. OWASP MCP Top 10: MCP05:2025.

This flaw never passes through the agent. The text comes from other users of the messaging service, so anyone on it could run commands on a recipient's Mac by sending a message or setting a presence note, and no agent permission setting would have stopped it. A server that receives content from other people should be reviewed like any other software exposed to the internet before it goes on a machine with sensitive access. Upgrade to 0.5.7 or later.

## x_ONYEK-2026-0004: @gamaze/hicortex

Command injection through session transcript text on the claude-cli backend. Introduced in 0.3.10, fixed in 0.23.1. OWASP MCP Top 10: MCP05:2025.

The vulnerable code runs in a scheduled background job that distils saved transcripts, so it works outside any tool call and no approval prompt appears. Assistants routinely write code in backticks, which meant it could run commands with no attacker involved. When assessing an agent tool, check what it does in the background as well as what its tools do when called. Upgrade to 0.23.1 or later.

## x_ONYEK-2026-0005: chromex-mcp

Command injection through the audit tool's page URL and report path on macOS and Linux. Introduced in 1.4.0, fixed in 1.8.2. OWASP MCP Top 10: MCP05:2025.

The audit tool was marked read-only, and a client that auto-approves read-only tools would have run it without asking. That label is an unverified hint set by the tool's own author. The audited page's URL also reached the shell, so a hostile web page could trigger the flaw simply by being audited, with no need to steer the model. Treat read-only labels as the author's claim rather than a guarantee, and upgrade to 1.8.2 or later.
