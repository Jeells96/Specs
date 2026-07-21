# SpecCheck — Submittal Compliance

A single-file web app for mechanical/plumbing contractors: upload a project spec PDF, and SpecCheck extracts every Division 22 (Plumbing) and Division 23 (HVAC) section's submittal requirements into a checklist. Then upload vendor submittals per section, and it uses Google Gemini to check each requirement as **met**, **partial**, or **missing**.

## Using it

1. Open `index.html` in a browser (or serve the repo via GitHub Pages — the file must be named `index.html`).
2. Optional: paste a [Google Gemini API key](https://aistudio.google.com/apikey) in the header to enable automatic submittal checking. The key is stored only in your browser's localStorage and calls go straight from your browser to Google.
3. Create a job, upload the project spec — either one combined PDF or multiple per-section PDFs at once (drag-drop supports multi-select) — then open any section and upload vendor submittals. Scanned vendor submittals without a text layer are sent to Gemini as PDFs so they can still be checked.

All data (jobs, checklists, PDFs) is stored locally in your browser — spec PDFs in IndexedDB, checklist state in localStorage. Nothing is uploaded to any server other than the Gemini API text checks.

## Notes and limits

- Scanned specs without an OCR text layer can't be parsed (the app will tell you).
- Section page links open the stored spec PDF at the exact page where the section starts.
- Uploading a new spec to an existing job rebuilds its checklist (you'll be asked to confirm).
