# Journal Entry Monitor — List Report

Read-only **Fiori Elements List Report + Object Page** for SAP universal journal line items.

Field names, types and navigations come from the real OData V2 metadata `API_JOURNALENTRYITEMBASIC_SRV` (SAP Journal Entry Item – Basic). The app does not invent document or posting-date fields that are absent from that API. Time is modelled as `LedgerFiscalYear` / `FiscalPeriod` / `FiscalYearPeriod`. The technical key is `ID`.

The companion **Analytical List Page** lives in [mindtek-journalentry-monitor-alp](https://github.com/igormuntoreanu/mindtek-journalentry-monitor-alp).

## Why a new tab from the MindTek website

The marketing site is **OpenUI5**. This monitor is **SAPUI5 Fiori Elements** (`sap.suite.ui.generic.template`). Embedding both in one shell mixes frameworks and squeezes a full Fiori filter bar and table into a marketing layout. Opening the app in a new tab matches Fiori launchpad behaviour.

## Run locally

```bash
npm install
npm start
```

Opens [http://localhost:8081/index.html](http://localhost:8081/index.html).

The MindTek Work page (other repo, port 8080) launches this URL in a new tab.

## Mock server

`webapp/localService/mockserver.js` uses `sap/ui/core/util/MockServer` with:

- `webapp/localService/metadata.xml` — copy of `claude/API_JOURNALENTRYITEMBASIC_SRV.edmx`
- `webapp/localService/mockdata/*.json` — company codes, cost/profit centers, G/L accounts, 420 journal items

Regenerate mock data:

```bash
npm run generate-mockdata
```

The OData model runs in **client** operation mode so filters and sorting work against the mock payload.

## Annotations

`webapp/annotations/annotations.xml` is local UI vocabulary (`LineItem`, `SelectionFields`, `HeaderInfo`, `Facets`, `DataPoint`, `Criticality`, `ValueList`). The SAP EDMX has no `UI.*` terms.

## Notes

- Read-only: no create, update, delete, no draft.
- Credit postings are stored as **negative** amounts so Fiori criticality can render them in red.
- A few rows use `JrnlEntryItemObsoleteReason = W` (reversed) and some balance-sheet items have no cost center.
- Excel export is the standard Smart Table toolbar action.
- **Export PDF** is a custom header action (`pdfmake`, current selection or first 100 rows).
