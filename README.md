# Kontoflux.io Finance Skills

Agent skills for German finance work with your **own** bank accounts, built for the
[Kontoflux.io MCP server](https://kontoflux.io/mcp/). Install them in Claude Code,
Codex or Claude and your agent can match incoming payments to invoices, prepare the
month-end close for your tax advisor, forecast cash, prepare the VAT return
(UStVA) and more, using live data from your connected accounts.

The skills are written in German, follow the open [Agent Skills](https://agentskills.io/)
format and are read-only: no skill triggers payments, cancels contracts or sends
messages.

> **Note:** These skills support your own work. They do not replace professional
> bookkeeping, tax or legal advice and are provided as is. See the
> [disclaimer](#disclaimer).

## Skills

| Skill | What it does |
|---|---|
| [kontoflux-bankdaten](skills/kontoflux-bankdaten/SKILL.md) | Foundation: choose accounts, page through all transactions, date boundaries, signed amounts, transfers between own accounts, safe handling of payment references |
| [kontoflux-zahlungseingang](skills/kontoflux-zahlungseingang/SKILL.md) | Match incoming payments to invoices and e-invoices (XRechnung, ZUGFeRD), incl. partial payments, Skonto and collective payments |
| [kontoflux-offene-posten](skills/kontoflux-offene-posten/SKILL.md) | Find overdue invoices and draft payment reminders and dunning letters (BGB default interest) |
| [kontoflux-monatsabschluss](skills/kontoflux-monatsabschluss/SKILL.md) | Match bookings and receipts, list missing receipts, prepare the handover to the tax advisor (e.g. DATEV Unternehmen online) |
| [kontoflux-liquiditaet](skills/kontoflux-liquiditaet/SKILL.md) | 13-week cash forecast with German tax and social-security payment dates and scenarios |
| [kontoflux-ustva](skills/kontoflux-ustva/SKILL.md) | Prepare the German VAT return (Umsatzsteuer-Voranmeldung), especially for cash-basis taxation |
| [kontoflux-abos-lastschriften](skills/kontoflux-abos-lastschriften/SKILL.md) | Detect subscriptions and SEPA direct debits, price increases, double debits and returns |
| [kontoflux-finanzbericht](skills/kontoflux-finanzbericht/SKILL.md) | Monthly or quarterly report with explained variances |

## Install

### Claude Code

```bash
claude plugin marketplace add kontoflux-io/finance-skills
claude plugin install kontoflux-finance@kontoflux
```

Or inside a session: `/plugin marketplace add kontoflux-io/finance-skills`, then
`/plugin install kontoflux-finance@kontoflux`. Update later with
`claude plugin marketplace update kontoflux`.

### Codex

```bash
codex plugin marketplace add kontoflux-io/finance-skills
codex plugin add kontoflux-finance@kontoflux
```

Update later with `codex plugin marketplace upgrade`.

### Claude (web and desktop)

Download the ZIP of a skill from the [latest release](https://github.com/kontoflux-io/finance-skills/releases/latest)
and upload it in Claude's settings under Skills. Always add `kontoflux-bankdaten.zip`;
the other skills build on it.

### Other agents

Copy the folders from [`skills/`](skills/) into your agent's skills directory, for
example `.agents/skills/` or `~/.claude/skills/`. See
[agentskills.io/clients](https://agentskills.io/clients) for supported clients.

## Connect your bank accounts

The skills need the Kontoflux.io MCP server in the same agent:

1. Connect your bank account in [Kontoflux.io](https://kontoflux.io).
2. Create an MCP integration and select the accounts your agent may read.
3. Open "Client verbinden" and run the setup command for your client. The
   [MCP guide](https://docs.kontoflux.io/integrationen/mcp-server/) covers Claude Code,
   Codex, Claude, ChatGPT, Cursor and more.

Then ask, for example:

- "Welche Rechnungen aus `offen.csv` sind inzwischen bezahlt?"
- "Bereite den Monatsabschluss für September vor."
- "Reicht unser Kontostand in den nächsten 13 Wochen?"
- "Welche Lastschriften sind in den letzten sechs Monaten teurer geworden?"

## Good to know

- Bank data reflects the last synchronisation, not real time. Every skill states
  the data timestamp.
- Kontoflux.io categories are general spending groups, not SKR03/SKR04 accounts.
- Where your data is processed after retrieval depends on the agent and model you
  use.

## Disclaimer

These skills are tools that support your own work. They do not replace professional
bookkeeping, accounting, tax or legal advice, and they do not automatically take over
the services of your tax advisor, accountant or bank. Tax and legal content (such as
the VAT return, default interest or direct debit refund periods) is general
information. Results can be incomplete or wrong: check them before you rely on them,
submit anything to the tax office or make decisions.

The skills are provided "as is", without warranty of any kind, express or implied,
including accuracy, completeness and fitness for a particular purpose. To the extent
permitted by law, neither the creator nor Kontoflux.io is liable for any damage
arising from the use of these skills or from decisions based on their results.

### Haftungsausschluss

Diese Skills unterstützen dich bei deiner eigenen Arbeit. Sie ersetzen keine
professionelle Buchhaltung, Steuerberatung oder Rechtsberatung und übernehmen nicht
automatisch die Leistungen deiner Steuerberatung, Buchhaltung oder Bank. Steuerliche
und rechtliche Inhalte (etwa zur Umsatzsteuer-Voranmeldung, zu Verzugszinsen oder zu
Lastschrift-Fristen) sind allgemeine Informationen. Ergebnisse können unvollständig
oder fehlerhaft sein: Prüfe sie, bevor du dich darauf verlässt, etwas beim Finanzamt
einreichst oder Entscheidungen triffst.

Die Skills werden ohne Gewähr bereitgestellt, ohne ausdrückliche oder stillschweigende
Zusicherung, insbesondere hinsichtlich Richtigkeit, Vollständigkeit oder Eignung für
einen bestimmten Zweck. Soweit gesetzlich zulässig, haften weder der Ersteller noch
Kontoflux.io für Schäden, die aus der Nutzung dieser Skills oder aus Entscheidungen
auf Grundlage ihrer Ergebnisse entstehen.

## Credits

Written for Kontoflux.io. Topics and structure were inspired by public collections
such as [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins),
[openaccountants/openaccountants](https://github.com/openaccountants/openaccountants) and
[Klotzkette/claude-fuer-deutsches-recht](https://github.com/Klotzkette/claude-fuer-deutsches-recht).
No content was copied from them.

## Maintainers

- Bump `version` in both `plugin.json` and `.claude-plugin/plugin.json` for every
  release; clients cache plugins by version.
- Push a tag `vX.Y.Z` to build the per-skill ZIPs for the release.
- Validate before pushing: `claude plugin validate .`
