# ONYEK

A small, curated database of security advisories for npm packages in the AI agent and Model Context Protocol (MCP) ecosystem. Maintained by Michael K Onyekwere.

## What this is

Most vulnerability databases cover general-purpose packages well and MCP servers and agent tooling barely at all. This one focuses on that gap: command injection, unsafe code execution, credential exposure and similar flaws in the tools that autonomous agents run.

## How a record is made

1. A candidate finding is located in the package's published source, in context.
2. It is reviewed, AI-assisted, starting from the default that it is **not** a vulnerability. A finding is confirmed only when attacker-influenced input can be shown to reach a dangerous sink.
3. Every confirmation is independently re-audited before it is recorded, and the underlying shell or evaluation behaviour is demonstrated with a harmless primitive, never a working exploit.
4. Affected version ranges are measured against every published release, not inferred.
5. Findings that are not yet fixed are disclosed privately to the maintainer first. Only fixed and coordinated findings are published here.

The review is AI-assisted with human accountability; it is not claimed to be hand-audited line by line by a person. Records carry no working attack strings.

## Format

Each file under `records/` is a single advisory in [OSV schema](https://ossf.github.io/osv-schema/) format, validated against the published JSON Schema. Records currently carry the `x_ONYEK` experimental prefix; the `x_` is dropped once the `ONYEK` prefix is allocated by osv.dev.

## Notes for deployers

Each record also carries two fields under `database_specific`, written for the teams that decide which agent tools to install:

- `owasp_mcp_top10`: the [OWASP MCP Top 10](https://owasp.org/www-project-mcp-top-10/) category the record is evidence for.
- `deployer_note`: what the flaw means for a team running the tool, and what would have limited it.

[DEPLOYER-NOTES.md](DEPLOYER-NOTES.md) collects the notes in one place.

## Credit and licence

Records credit Michael K Onyekwere as the analyst. Reuse: CC-BY-4.0 (proposed), so the data can propagate through downstream tooling while the source is attributed.
