# Indian Polity Prelims & Smash Mains Curated App Dataset (2027 Edition)

## Decoupled JSON + Standalone Markdown (`.md`) Architecture
All Prelims and Smash Mains notes are stored in standalone, 100%-accurate `.md` files and linked from lightweight JSON index files via relative file paths (no inline Markdown inside JSONs):

- **Master Index:** `master_index.json` (links every chapter, subtopic, and article tag to its `.json`, `.md` notes, `.pdf` source pages, and `.png` diagrams)
- **Per-Chapter Folder (`chapters/<topic_id>/`):**
  - `<topic_id>.json`: Clean structured metadata, PYQ records, source page mappings, and relative links to `.md` files
  - `notes/<topic_id>_prelims_notes.md`: Full chapter Prelims notes
  - `notes/<topic_id>_smash_mains_notes.md`: Full chapter Smash Mains notes
  - `notes/<topic_id>_prelims_pyqs.md` & `notes/<topic_id>_mains_pyqs.md`: Formatted chapter PYQ banks
  - `notes/subtopics/<subtopic_id>_prelims_notes.md` & `<subtopic_id>_smash_mains_notes.md`: Granular subtopic/article Markdown notes
  - `notes/pages/prelims_page_XXX.md` & `mains_page_YYY.md`: Page-level Markdown notes
  - `subtopics/<subtopic_id>.json`: Lightweight subtopic JSON linking to the subtopic `.md` files, source PDFs, diagrams, and PYQs
  - `pages/prelims/*.pdf` & `pages/mains/*.pdf`: Single-page vector source PDFs
  - `diagrams/*.png`: High-resolution (`scale=4.0`) cropped diagrams

## Source JSON datasets

- [COI dataset](Coi.json)
- [Laxmikant dataset](Laxmikant.json)
