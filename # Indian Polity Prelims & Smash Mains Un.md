# Indian Polity Prelims & Smash Mains Unified Dataset (2027)

This dataset bundles all **14 unified Indian Polity chapters** in plain folders (no nested zips), connected by `master_index.json`, along with two standalone reference datasets (`Coi.json` and `Laxmikant.json`) at the root so your app can render **Prelims notes**, **Smash Mains notes**, **PYQs**, **high-resolution diagram screenshots (PNG)**, **single-page source PDFs**, **Constitution of India articles**, and **M. Laxmikanth reference chapters** side-by-side.

## 1. Folder Structure

```text
Indian_Polity_Prelims_Mains_Dataset/
├── master_index.json                       # Global entry point: chapter list + Article/Subtopic lookup
├── README.md                               # This integration & GitHub deployment guide
├── Coi.json                                # Standalone: The Constitution of India (bare text, 524 chunks)
├── Laxmikant.json                          # Standalone: Indian Polity by M. Laxmikanth (8th Ed., 95 topics)
└── chapters/
    ├── Ch01_Constitution_Making_Nature_Salient_Features/
    │   ├── Ch01_Constitution_Making_Nature_Salient_Features.json
    │   ├── subtopics/                      # Granular Article/Subtopic JSON files
    │   ├── diagrams/                       # High-resolution cropped PNG diagram/chart screenshots (scale 4.0)
    │   └── pages/
    │       ├── prelims/                    # Single-page Prelims source PDFs (prelims_page_XXX.pdf)
    │       └── mains/                      # Single-page Mains source PDFs (mains_page_YYY.pdf)
    ├── ...
    └── Ch14_Comparison_of_Constitutions/
```

## 2. Standalone Reference Files (Root Level)

These two files are included at the root of the dataset as standalone JSON datasets (separate from `chapters/` and not indexed as chapter folders):

1. **`Coi.json` — The Constitution of India**
   - **Edition / Date**: Sixth pocket size edition (incorporating amendments up to the Constitution (One Hundred and Sixth Amendment) Act, 2023 and 2026 updates), as on `2026-09-10`
   - **Publisher**: Government of India, Ministry of Law and Justice, Legislative Department
   - **Scope**: 402 pages, 524 chunks, 496 articles (467 active, 29 omitted), 12 schedules, 3 appendices
   - **Structure**:
     - `document_metadata`: Title, edition, publisher, date, and structural counts.
     - `chunks`: Array of 524 structured entries with `chunk_index`, `id`, `type`, `part`, `part_title`, `chapter`, `sub_heading`, `article_number`, `title`, `status`, `text`, `clauses`, `footnotes`, `page_start`, `page_end`, `word_count`, `char_count`, `approx_tokens`, and `embedding_text`.

2. **`Laxmikant.json` — Indian Polity (M. Laxmikanth)**
   - **Edition / Author**: 8th Edition (Kala Bhairava Edition) by M. Laxmikanth
   - **Publisher**: McGraw Hill Education (India) Private Limited
   - **Scope**: 1,200 pages across `front_matter`, 13 `parts` (95 `topics`), 10 `appendices`, and `back_matter`
   - **Structure**:
     - Top-level metadata: `book_title`, `edition`, `author`, `publisher`, `total_pages`, `running_header`.
     - `parts[]`: Each part contains `part_number`, `part_title`, `start_page`, `end_page`, `intro_pages`, and `topics[]` (each topic has `chapter_number`, `topic_title`, `start_page`, `end_page`, `pages`).

## 3. How to Connect Everything in Your App

### Step A: Load `master_index.json`
`master_index.json` has two main keys:
1. `topics`: Array of all 14 chapters with `chapter_json`, `prelims_pages_folder`, `mains_pages_folder`, `diagrams_folder`, and `subtopics`.
2. `article_and_subtopic_lookup`: Direct dictionary keyed by Article or theme (e.g., `"Article 21"`, `"Article 14"`, `"Article 368"`, `"Article 200"`, `"Panchayat"`).

Chapter entries and their nested subtopic entries include `prelims_notes_markdown_path` and `smash_mains_notes_markdown_path`. Direct lookup entries include those fields for the subtopic notes and `chapter_prelims_notes_markdown_path` / `chapter_smash_mains_notes_markdown_path` for the chapter notes. These master-index paths are relative to the repository root.

### Step B: Look Up Any Article or Topic (Example: `Article 21`)
1. Query `master_index.article_and_subtopic_lookup["Article 21"]`:
   - `chapter_json_path`: `"chapters/Ch05_Fundamental_Rights/Ch05_Fundamental_Rights.json"`
   - `subtopic_json_path`: `"chapters/Ch05_Fundamental_Rights/subtopics/Art_21_Right_to_Life_Personal_Liberty_Privacy_Digital_Rights_Education.json"`
2. Load `subtopic_json_path` (or find the matching item inside `chapter.subtopics_and_articles`):
   - `prelims_notes_markdown_path`: Relative path to the Prelims Markdown notes.
   - `smash_mains_notes_markdown_path`: Relative path to the Smash Mains Markdown notes.
   - `relevant_prelims_pyqs` & `relevant_mains_pyqs`: Tagged PYQs with `source_page_image` (pointing to `.pdf`).
   - `diagrams`: List of diagram objects (`diagram_image_path`, `title`, `source_page_image`).
   - `prelims_source_pages` & `mains_source_pages`: Exact relative paths to the single-page source PDFs.
3. Optionally cross-reference `Coi.json` (filter `chunks` by `article_number`, e.g., `"21"`) for the bare constitutional text and `Laxmikant.json` for textbook coverage.

The chapter JSON retains its topic and subtopic relationships. Notes are stored in adjacent `.prelims.md` and `.mains.md` files; the JSON path fields point to those files. Resolve each note path relative to the directory containing the JSON that supplied it. Chapter JSON subtopic entries point into `subtopics/`, while standalone subtopic JSON paths point to sibling Markdown files.

### Step C: Resolve Relative Diagram & Source PDF Paths
All diagram and page links inside a chapter JSON or subtopic JSON (such as `diagrams/pre_diag_art21_rights_table_p052.png` or `pages/prelims/prelims_page_052.pdf`) are **relative to that chapter folder** (`chapters/<topic_id>/`).

In your React/TypeScript or backend code:
```ts
const chapterBase = `chapters/${subtopic.parent_chapter_id}`;

// Resolve any Markdown diagram or source PDF link:
const resolveAssetUrl = (relativePath: string) => `${chapterBase}/${relativePath}`;

// Example: when user clicks a PYQ "View Source Page" button:
const pagePdfUrl = resolveAssetUrl(pyq.source_page_image);
// -> "chapters/Ch05_Fundamental_Rights/pages/prelims/prelims_page_047.pdf"
```

### Step D: Rendering Markdown with High-Res Diagrams & PDF Links
When rendering a Markdown notes file in a Markdown component (e.g. `react-markdown`), resolve links relative to the Markdown file's location so that:
- `![Diagram](diagrams/...)` renders the high-resolution PNG diagram screenshot inline.
- `[Prelims Source Page 52](pages/prelims/prelims_page_052.pdf)` opens the single-page source PDF in your PDF viewer/modal or a new browser tab.

Subtopic notes are stored in `subtopics/` and use `../diagrams/` and `../pages/` links to reach chapter assets. Chapter notes are stored beside the chapter JSON and link directly to `diagrams/` and `pages/`.

## 4. Pushing the Unzipped Folder Structure to GitHub

To commit the full folder structure directly to a GitHub repository (instead of uploading the `.zip` file):

1. **Extract** `Indian_Polity_Prelims_Mains_Complete.zip` into your local repository folder so `master_index.json`, `README.md`, `Coi.json`, `Laxmikant.json`, and `chapters/` sit at the repo root (or a subfolder).
2. **Push via Git CLI** (recommended for nested folders and 600+ files):
   ```bash
   git add .
   git commit -m "Add Indian Polity Prelims & Mains dataset with high-res diagrams and single-page PDFs"
   git push origin main
   ```
