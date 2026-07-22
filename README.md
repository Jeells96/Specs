# SpecCheck — Submittal Compliance

A single-file web app for mechanical/plumbing contractors to run the submittal side of a job: parse a project spec's Division 22 (Plumbing) and 23 (HVAC) submittal requirements, track vendor submittal packages through their review lifecycle, and check each requirement against what was actually submitted.

## What it does

- **Parses spec requirements.** Upload one combined spec PDF or many per-section PDFs at once. SpecCheck finds each Division 22/23 section and extracts its Action, Informational, and Closeout submittal requirements into a checklist.
- **Tracks submittal packages.** Each section holds submittal *packages* — first-class records with a package number, stored PDF, revisions (Rev 0, Rev 1, …), and a review status: **Preparing → Submitted → Approved / Approved as Noted / Revise & Resubmit**, with submitted/returned dates. One package can cover multiple sections (vendors bundle).
- **Checks compliance (optional AI).** With a Google Gemini API key, each uploaded package is checked against the spec and every requirement is marked met / partial / missing with the supporting text. Scanned submittals (no text layer) are sent to Gemini as PDFs so they still get read.
- **Human override on everything.** Every requirement can be set manually to Met / Partial / Missing / **N/A** with a note — the UI distinguishes an AI verdict from one you confirmed. The AI is never the final word.
- **Scope & triage.** Mark sections out-of-scope so they drop out of the counts (without deleting them). Each section rolls up to a status (Not started / In progress / Ready to submit / Submitted / Approved / Needs resubmit), and the list filters and sorts by status, number, or due date. Sections can carry a due date.
- **Deliverables.** Export the **submittal register** as CSV, or open a printable register (Save as PDF) — the log a GC actually asks for.
- **Backup & restore.** Everything lives in your browser, so you can **back up a whole job** (checklist + PDFs) to a single `.speccheck.json` file and restore it on another machine.

## Using it

1. Open `index.html` in a browser (or serve the repo via GitHub Pages — the file must be named `index.html`).
2. Optional: paste a [Google Gemini API key](https://aistudio.google.com/apikey) in the header to enable automatic checking. The key is stored only in your browser and calls go straight from your browser to Google.
3. Create a job and upload the spec — drag in a combined PDF, all the per-section PDFs, or a `.zip` of them.
4. Add vendor submittals: drag PDFs, a **folder**, or a `.zip` onto the **Add Vendor Submittals** zone (or use "Choose a folder…"). Each PDF is matched to its section by the CSI number in its **folder name or filename** and turned into a package — so a folder like `23 07 19 - HVAC Piping Insulation/…/Submittal.pdf` lands on the right section even when the filename itself is generic. Unmatched files are reported. Everything uploaded shows in the **Uploaded submittals** list on the workspace, and each section card shows a 📎 count. You can also open a single section and create/upload a package by hand. Set statuses, dates, and overrides as the review progresses, and export the register when the GC asks.

## Storage & limits

- Data is stored locally: job metadata and checklist state in `localStorage`, PDFs in `IndexedDB`. Nothing is uploaded anywhere except the Gemini text checks. **Back up jobs** you care about — clearing browser data erases them.
- Scanned specs without an OCR text layer can't be parsed into a checklist (the app tells you). Scanned *submittals* are still checkable via Gemini's PDF reading.
- This is a serverless, single-user tool by design. Multi-user/shared state would require a backend.
