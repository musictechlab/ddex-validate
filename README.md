# ddex-validate

A [Claude skill](https://docs.anthropic.com/en/docs/claude-code/skills) that validates DDEX ERN (Electronic Release Notification) XML files against official schemas — catching music metadata errors before they reach distributors.

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

## Contributing

Issues and PRs welcome. If you work with DDEX and encounter validation edge cases, please open an issue with a (sanitized) XML sample.

## License

MIT

---

Built by [Music Tech Lab](https://musictechlab.io) — tools for the music industry.
