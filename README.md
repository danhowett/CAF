# NCSC CAF v4.0 Assessment Toolkit

A pair of self-contained, offline HTML tools for carrying out and presenting a self-assessment against the UK National Cyber Security Centre's [Cyber Assessment Framework (CAF) version 4.0](https://www.ncsc.gov.uk/collection/cyber-assessment-framework).

- **Assessment Tool** — score all 41 contributing outcomes, record evidence and justifications, and generate a full print-ready PDF report.
- **Executive Briefing** — a boardroom slide deck generated from the tool's export, for presenting the outcome to senior stakeholders.

Both are single HTML files. There is no build step, no server, no dependencies and no network calls — open them in a browser and everything runs locally. Nothing you enter ever leaves your machine.

> **Disclaimer.** These tools support *self-assessment* and do not constitute a regulatory determination. Target levels for each outcome are set by your relevant regulator or Competent Authority. Always confirm your requirements against the official CAF and your sector profile.

---

## Contents

- [Quick start](#quick-start)
- [The Assessment Tool](#the-assessment-tool)
- [The Executive Briefing](#the-executive-briefing)
- [How the two fit together](#how-the-two-fit-together)
- [Scoring model](#scoring-model)
- [Data, persistence and privacy](#data-persistence-and-privacy)
- [Export formats](#export-formats)
- [Browser support](#browser-support)
- [Repository layout](#repository-layout)
- [Licence and attribution](#licence-and-attribution)

---

## Quick start

1. Download the two HTML files (see [Repository layout](#repository-layout)).
2. Double-click **`NCSC-CAF-v4-Assessment-Tool.html`** to open it in your browser.
3. Click **Load Sample** in the sidebar to explore a worked example, or **Assessment Setup** to start your own.
4. Work through the four objectives, rating each contributing outcome and recording evidence.
5. Click **Generate Report** for the PDF, and **JSON** to export a data file.
6. To brief stakeholders, open **`CAF-v4-Executive-Slideshow.html`** and drop your exported JSON onto it.

No installation. To use it from a web server instead, serve the files as static assets — nothing else is required.

---

## The Assessment Tool

`NCSC-CAF-v4-Assessment-Tool.html`

A complete workspace for a CAF v4.0 self-assessment.

### Framework coverage

- All **4 objectives**, **14 principles** and **41 contributing outcomes** of CAF v4.0 (published 6 August 2025), verified against the official framework.
- Correct handling of the **nine binary outcomes** that have no "Partially Achieved" level (A1.a, A1.b, A1.c, A2.c, A3.a, D1.b, D1.c, D2.a, D2.b) — these are rated Achieved / Not Achieved only.
- New-in-v4.0 outcomes are flagged.

### Assessing

- Rate each outcome **Achieved**, **Partially Achieved**, **Not Achieved** or **Not Applicable**.
- **Not Applicable requires a written justification** — the tool prompts for a defensible position (exemption, justification, compensating controls) and flags any N/A left unjustified.
- Record free-text **evidence and commentary** against every outcome.
- A live **validation banner** tracks completeness: outcomes not yet addressed, and N/A claims missing justification, are surfaced until resolved.

### Assessment setup

Captures the context that a reviewer or Competent Authority expects, all optional and editable at any time:

- Organisation, sector, Competent Authority, essential function and in-scope systems
- CAF Profile
- Audit team: assessment lead, QC / quality reviewer, team members, client / business owner, Senior Responsible Owner
- Assessment date and period, evidence location, method / notes, version reference, and classification

### Reporting

**Generate Report** produces a branded, paginated, print-ready document (use your browser's *Save as PDF*). It includes:

1. **Cover** — headline maturity, risk posture and key assessment metadata.
2. **About this assessment** — purpose, scope, audience, who assessed it, and a full assessment-details table.
3. **Executive summary** — rating breakdown, maturity by objective, and a 14-principle radar chart with a decoded key.
4. **Risk matrix** — rated outcomes plotted by impact against likelihood, plus the highest residual risks.
5. **Improvement plan** — gaps ranked by priority, with the scoring method explained.
6. **Detailed recommendations (4a)** — a full paragraph per priority gap, generated from the ratings.
7. **Not Applicable register** — every N/A declaration with its justification.
8. **Contributing outcome detail** — all 41 outcomes with ratings and recorded evidence.

### Sample data

**Load Sample** populates a realistic, fictional water-sector OT assessment so you can see every feature — including a mix of all rating states, justified N/A entries and one deliberately left blank to demonstrate validation.

---

## The Executive Briefing

`CAF-v4-Executive-Slideshow.html`

A dark, full-screen slide deck for presenting the assessment outcome to a board or senior stakeholders. It is generated entirely from the tool's JSON export.

### Using it

1. Open the file in a browser.
2. Drag your exported JSON onto the window, or click **Choose assessment file**. The JSON the tool auto-saves to disk works directly.
3. Navigate the slides.

### Controls

| Key | Action |
| --- | --- |
| `→` / `Space` / `Page Down` | Next slide |
| `←` / `Page Up` | Previous slide |
| `Home` / `End` | First / last slide |
| `F` | Toggle full-screen |

On-screen arrows and a progress rail are also provided.

### Slides

1. **Title** — headline posture, organisation, classification and scope.
2. **Outcome scoring** — the rating breakdown and submission readiness.
3. **Maturity by objective** — the four objectives as scored bars.
4. **Security principle profile** — the 14-principle radar with a decoded key.
5. **Risk matrix** — impact vs likelihood, with the highest residual risks.
6. **Improvement priorities** — category counts and the top gaps to brief.
7. **Next steps** — recommended actions, adapted to the assessment's state.

The briefing is view-only and always renders these seven slides; it does not edit the underlying assessment.

---

## How the two fit together

```
┌─────────────────────────────┐        JSON export        ┌──────────────────────────┐
│  Assessment Tool            │  ─────────────────────▶   │  Executive Briefing      │
│  score · evidence · report  │   (schemaVersion 1)       │  board-ready slide deck  │
└─────────────────────────────┘                           └──────────────────────────┘
        │                                                           ▲
        │  auto-saves the same JSON to disk / localStorage          │
        └───────────────────────────────────────────────────────────┘
                        the auto-saved file feeds the briefing directly
```

The tool's JSON export is the single interchange format. The same file:

- **re-imports** into the tool to restore an assessment,
- **feeds** the Executive Briefing, and
- is what the tool **auto-saves** to disk.

So the file the tool saves as you work can be dropped straight onto the briefing.

---

## Scoring model

Maturity is measured **only over rated outcomes** (Achieved / Partially Achieved / Not Achieved). Outcomes marked **Not Applicable are excluded** from the calculation rather than counted as failures; outcomes **Not Addressed** are excluded but flagged as outstanding.

Each rated outcome scores 3 (Achieved), 2 (Partially Achieved) or 1 (Not Achieved). An objective's percentage is its points over its maximum possible points across rated outcomes.

**Improvement priority** ranks the gaps:

```
Priority = ( Impact × Likelihood × Gap severity ) ÷ Effort
```

- **Impact (1–5)** — how badly a failure of the outcome would affect the essential function.
- **Likelihood (1–5)** — current exposure to exploitation or failure.
- **Gap severity** — 2 for Not Achieved, 1 for Partially Achieved.
- **Effort (1–5)** — relative cost and complexity to remediate.

Higher scores are addressed first. Items are also tagged **Quick win**, **Critical**, **Strategic** or **Foundational**. The impact / likelihood / effort weightings are the toolkit's own heuristic to aid prioritisation — they are not part of the CAF and should be reviewed against your own risk assessment.

---

## Data, persistence and privacy

Everything is stored locally. There are no accounts, no telemetry and no network requests.

The Assessment Tool saves your work in three layers:

1. **Automatic in-browser save** — every change is written to the browser's `localStorage` (debounced). On reopening, the tool offers to restore your last session. This survives refreshes, tab closes and reboots.
2. **Auto-save to a file** *(Chromium browsers)* — click **Auto-save to file…** and choose a location once; the tool then writes back to that file automatically as you work, using the File System Access API.
3. **Manual export** — the **JSON** button downloads a full backup at any time.

> `localStorage` is per-browser and per-machine, and is cleared by private/incognito mode on close. For portability between machines, or as a durable backup, use the JSON export or the disk auto-save.

---

## Export formats

### JSON (`schemaVersion: 1`)

The complete, re-importable state and the interchange format for the briefing. Top-level structure:

```jsonc
{
  "schemaVersion": 1,
  "framework": { "name": "...", "version": "4.0", "published": "2025-08-06",
                 "objectives": 4, "principles": 14, "contributingOutcomes": 41 },
  "assessment": {
    "organisation": "...", "leadAssessor": "...", "date": "...",
    "essentialFunction": "...", "cafProfile": "...", "inScopeSystems": "...",
    "sector": "...", "competentAuthority": "...", "qcReviewer": "...",
    "teamMembers": "...", "clientOwner": "...", "seniorResponsibleOwner": "...",
    "assessmentPeriod": "...", "evidenceLocation": "...", "method": "...",
    "version": "...", "classification": "...", "exportedAt": "<ISO 8601>"
  },
  "completeness": {
    "addressed": 40, "outstanding": ["D2.b"],
    "unjustifiedNotApplicable": [], "readyForSubmission": false
  },
  "summary": {
    "overallPercent": 64, "rated": 39,
    "achieved": 9, "partiallyAchieved": 17, "notAchieved": 12,
    "notApplicable": 2, "notAddressed": 1,
    "byObjective": [ { "id": "A", "title": "...", "percent": 67,
                       "rated": 9, "notApplicable": 0, "notAddressed": 0 } ],
    "improvementPlan": [ { "ref": "B2.c", "objective": "B", "name": "...",
                           "rating": "NOT", "impact": 5, "likelihood": 5,
                           "effort": 3, "priority": 16.7, "category": "critical",
                           "action": "..." } ]
  },
  "contributingOutcomes": [
    {
      "ref": "A1.a", "objective": "A", "principle": "A1",
      "principleTitle": "Governance", "name": "Board Direction",
      "newInV4": false, "partiallyAchievedAvailable": false,
      "rating": "ACH", "ratingLabel": "Achieved",
      "impact": 5, "likelihood": 3, "effort": 2, "riskScore": null,
      "justification": null, "evidence": "...",
      "justificationRequired": false, "justificationMissing": false
    }
    // ... 41 outcomes
  ]
}
```

Rating codes: `ACH`, `PAR`, `NOT`, `NAP` (Not Applicable), `UNSET` (Not Addressed).

### CSV

A flat, one-row-per-outcome extract for auditors and spreadsheet analysis, with a metadata header block, an objective summary, and per-outcome ratings, justifications and evidence. RFC 4180 quoting and a UTF-8 BOM so it opens cleanly in Excel.

### PDF

Produced via the browser's *Save as PDF* from the **Generate Report** view (see [Reporting](#reporting)).

---

## Browser support

Built with standard HTML, CSS and vanilla JavaScript — no frameworks.

| Feature | Chrome / Edge | Firefox | Safari |
| --- | --- | --- | --- |
| Assessment, scoring, reporting | ✓ | ✓ | ✓ |
| In-browser auto-save & restore | ✓ | ✓ | ✓ |
| Auto-save to a chosen file | ✓ | — *(use JSON export)* | — *(use JSON export)* |
| PDF export (Save as PDF) | ✓ | ✓ | ✓ |
| Executive Briefing | ✓ | ✓ | ✓ |

**Printing the report:** choose **Save as PDF** as the destination, set **Margins: Default**, turn **Background graphics ON** and turn **Headers and footers OFF** (this removes the browser's date and file-path stamps). Chrome and Edge give the most faithful result; the full-bleed cover may show a margin in other browsers.

---

## Repository layout

```
.
├── NCSC-CAF-v4-Assessment-Tool.html   # the assessment workspace + report
├── CAF-v4-Executive-Slideshow.html    # the briefing deck (loads exported JSON)
├── CAF-v4-SAMPLE-export.json          # a sample export to try the briefing with
└── README.md
```

> This repository tracks a single current version of each file. If you keep dated or numbered builds, record changes here or in a `CHANGELOG`.

---

## Licence and attribution

The CAF itself is Crown copyright: **CAF content © Crown copyright 2025, reproduced under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).** The Open Government Licence version (v3.0) is unrelated to the CAF framework version (v4.0).

Add your own licence for the toolkit code in a `LICENSE` file and reference it here (for example, MIT for a permissive open-source release). Until then, no code licence is granted by default.

The official framework is published by the NCSC: <https://www.ncsc.gov.uk/collection/cyber-assessment-framework>
