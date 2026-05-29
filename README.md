# ddex-validate

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Built by MusicTech Lab](https://musictechlab.io/oss/build-by-musictechlab.io.svg)](https://musictechlab.io)

> **Proof of Concept** — This skill is an experiment in using AI to streamline daily routines when working with music industry data. Instead of switching to web validators or remembering CLI flags, you validate DDEX files from the same conversation where you're already working. It's a small example of a bigger idea: embedding domain expertise directly into AI workflows so that repetitive data checks happen automatically, not manually.

A [Claude skill](https://docs.anthropic.com/en/docs/claude-code/skills) that validates DDEX ERN (Electronic Release Notification) XML files against official schemas — catching music metadata errors before they reach distributors.

## Why this exists

Music labels and distributors deal with DDEX XML files daily. Every release submission requires valid metadata — ISRCs, UPCs, territory codes, contributor roles, deal terms. Getting it wrong means rejected deliveries and delayed releases.

The validation itself is well-defined and mechanical: parse XML, check against a schema, verify field formats. That makes it a perfect candidate for an AI skill — let the machine handle the tedious checks while you focus on the music.

This proof of concept explores how Claude skills can turn domain knowledge (DDEX standards, distributor requirements, common pitfalls) into reusable, on-demand expertise that activates right when you need it.

## What it does

Paste or upload a DDEX ERN XML file and get instant validation with actionable error messages:

- **Schema validation** against official DDEX XSD schemas (ERN 3.8.2, 4.1.1+)
- **Music industry checks** — ISRC format, UPC/EAN digits, territory codes, required fields
- **Fix suggestions** — every error comes with a specific, copy-pasteable fix
- **Structural summary** — release count, recordings, territories, sender/recipient info

## Quick start

### Option 1: Claude Code (CLI)

```bash
# Clone into your skills directory
git clone https://github.com/musictechlab/ddex-validate.git ~/.claude/skills/ddex-validate
```

Then in Claude Code:
```
> Validate this DDEX file: /path/to/release.xml
```

### Option 2: Claude.ai (Web)

1. Download this repo as a ZIP
2. Go to **Settings > Capabilities > Skills**
3. Upload the ZIP
4. Toggle on the **ddex-validate** skill

### Option 3: Manual install

Copy the skill folder to your Claude Code skills directory:
```bash
cp -r ddex-validate/ ~/.claude/skills/ddex-validate/
```

## Usage

The skill activates automatically when you mention DDEX, ERN, release metadata validation, or upload an XML file. You can also trigger it explicitly:

```
Validate this DDEX ERN XML against the 3.8.2 schema:
<paste XML here>
```

```
Check my release metadata for errors: /path/to/ern-release.xml
```

```
Is this DDEX file valid for distribution?
```

### Example output

**Valid file:**
```
DDEX ERN 382 — Valid

Summary:
- Releases: 1
- Sound Recordings: 1
- Images: 1
- Territory: Worldwide
- Message Type: TestMessage
- Sender: Sample Music Corp
- Recipient(s): Music Distribution Corp
```

**File with errors:**
```
DDEX ERN 382 — 2 error(s) found

| # | Line | Error                              | Suggested Fix                          |
|---|------|------------------------------------|----------------------------------------|
| 1 | 36   | Element 'ISRC': '' is not valid    | Add 12-char ISRC: CCXXXYYNNNNN         |
| 2 | 45   | Missing element 'TerritoryCode'    | Add <TerritoryCode>Worldwide</…>       |
```

## Supported versions

| ERN Version | Schema URL | Status |
|-------------|-----------|--------|
| 3.8.2 | `service.ddex.net/xml/ern/382/` | Fully supported |
| 4.1.1 | `service.ddex.net/xml/ern/411/` | Fully supported |
| 4.3 | `service.ddex.net/xml/ern/43/` | Supported |

The skill auto-detects the version from the XML namespace. If detection fails, it asks which version to use.

## Skill structure

```
ddex-validate/
├── SKILL.md                        # Core instructions (YAML frontmatter + Markdown)
├── references/
│   └── ern-structure.md            # ERN element hierarchy and field formats
├── assets/
│   └── ern382-sample.xml           # Example ERN 3.8.2 file for testing
└── README.md                       # This file (for humans on GitHub)
```

## What is DDEX?

[DDEX](https://ddex.net) (Digital Data Exchange) is the music industry standard for exchanging metadata between labels, distributors, and digital service providers (DSPs) like Spotify, Apple Music, and Amazon Music.

**ERN** (Electronic Release Notification) is the most common DDEX message type — it describes a music release including tracks, artwork, contributors, territory rights, and commercial terms.

Getting ERN metadata wrong means your release gets rejected by the distributor or DSP, delaying your release date. This skill catches those errors before submission.

## Where this is heading

This is a proof of concept — the first step in exploring how AI skills can fit into music data workflows. Some ideas we're considering:

- **Batch validation** — process a directory of ERN files before a bulk delivery
- **Auto-fix mode** — instead of just reporting errors, rewrite the XML with fixes applied
- **DSRF support** — extend to Digital Sales Report Format (flat file) validation
- **MCP integration** — connect to distributor APIs to validate _and_ submit in one workflow
- **Royalty data checks** — apply the same pattern to streaming reports and financial data

If you work with music metadata and have ideas for what an AI-powered validation workflow should look like, we'd love to hear from you.

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting a PR.

## Security

To report a vulnerability, please see [SECURITY.md](SECURITY.md).

## License

MIT

---

<div align="center">
  MusicTech Lab - Rockstars Developers dedicated to the Music Industry<br>
  <a href="https://musictechlab.io">Website</a>
  <span> | </span>
  <a href="https://linkedin.com/company/musictechlab">LinkedIn</a>
  <span> | </span>
  <a href="https://musictechlab.io/contact">Let's talk</a><br>
  Crafted by <a href="https://musictechlab.io">musictechlab.io</a>
</div>
