# Indian Polity Unified Prelims & Smash Mains App Dataset (2027)

## 1. Directory & Archive Architecture
```text
Indian_Polity_Prelims_Mains_App_Dataset_Master.zip
├── master_index.json                       # Global manifest & instant Article/Subtopic lookup map
├── README_APP_INTEGRATION.md               # Schema & React/TypeScript integration guide
└── chapters/
    ├── Ch01_Constitution_Making_Nature_Salient_Features/
    │   ├── Ch01_Constitution_Making_Nature_Salient_Features.json
    │   ├── relevant_pages.zip              # Zip of relevant Prelims & Mains source pages + diagrams
    │   ├── subtopics/                      # Granular Article/Subtopic JSON files
    │   ├── diagrams/                       # Cropped PNG screenshots of diagrams/flowcharts/maps
    │   └── pages/
    │       ├── prelims/                    # Rendered Prelims source page images (prelims_page_XXX.jpg)
    │       └── mains/                      # Rendered Mains source page images (mains_page_YYY.jpg)
    ├── ...
    └── Ch05_Fundamental_Rights/
        ├── Ch05_Fundamental_Rights.json
        ├── relevant_pages.zip
        ├── subtopics/
        │   └── Art_21_Right_to_Life_Personal_Liberty_Privacy_Digital_Rights_Education.json
        ├── diagrams/
        │   ├── pre_diag_art21_rights_table_p052.png
        │   ├── pre_diag_art21_inferred_rights_p053.png
        │   └── mains_diag_privacy_data_protection_art21_p050.png
        └── pages/
            ├── prelims/ (prelims_page_046.jpg ... prelims_page_057.jpg)
            └── mains/   (mains_page_034.jpg ... mains_page_074.jpg)
```

## 2. Simultaneous Prelims + Mains + Source Page Exploration (Example: Article 21)
When a user selects **Article 21** (or any Article/subtopic in `subtopics_and_articles` or `master_index.json -> article_and_subtopic_lookup["Article 21"]`):
1. **Prelims Notes (`prelims_notes_markdown`)**: Clean Markdown with tables comparing *Procedure Established by Law* vs *Due Process of Law*, *A.K. Gopalan* vs *Maneka Gandhi* vs *Puttaswamy*, Inferred Fundamental Rights under Art 21, and Art 21A (Right to Education), with embedded Markdown links to diagram screenshots (`![Diagram](diagrams/pre_diag_art21_rights_table_p052.png)`) and source pages (`[Prelims Source Page 52](pages/prelims/prelims_page_052.jpg)`).
2. **Smash Mains Notes (`smash_mains_notes_markdown`)**: Clean Markdown covering Right to Privacy & Puttaswamy 3-pronged test, DPDP Act, Digital Rights, Personality Rights, Phone Tapping, Narco-Analysis (`Selvi` case), and Passive Euthanasia, with embedded diagram links (`![Diagram](diagrams/mains_diag_privacy_data_protection_art21_p050.png)`) and source page links (`[Smash Mains Source Page 50](pages/mains/mains_page_050.jpg)`).
3. **Relevant Prelims & Mains PYQs (`relevant_prelims_pyqs` & `relevant_mains_pyqs`)**: Every question is tagged with `related_articles` (e.g., `["Article 21"]`) and includes `source_page_image` (e.g., `"pages/prelims/prelims_page_047.jpg"` or `"pages/mains/mains_page_034.jpg"`) so clicking "View Source Page" on any question opens the exact PDF page where that question appears.
