# Kitami LINE Bot - Source Register

## 1. Purpose

This register documents the official source documents used for the Kitami household-waste LINE Bot prototype. It assigns each source a clear Japanese title, English title, standardized filename, role, priority, and verification status.

## 2. Source authority order

When two sources appear inconsistent, use the following order:

1. **Japanese item dictionary** for item-specific classification and disposal instructions.
2. **Japanese household sorting guide** for category rules, bag colors, size limits, preparation, and safety requirements.
3. **Official area calendar** for collection weekdays and dates.
4. **English sorting guide** for resident-facing English explanations.
5. If an answer remains uncertain, do not guess. Refer the user to Kitami City Waste Management Division.

## 3. Official source documents

### Source 1: Japanese collection calendar

- Original filename: `content_20260311_134119.pdf`
- Japanese label: **令和8年度 北見自治区ごみカレンダー（対象地域版）**
- English label: **FY2026 Kitami Autonomous Region Waste Collection Calendar - Selected Area**
- Recommended filename: `01_kitami_collection_calendar_2026_ja.pdf`
- Language: Japanese
- Coverage: Selected prototype collection area containing multiple neighborhoods
- Validity: April 1, 2026 to March 31, 2027
- Role: Japanese calendar reference and cross-check for the selected service area
- Derived output: Supports `collection_schedule.json`
- Verification role: Secondary calendar verification
- Notes: The visual calendar is authoritative because plain-text extraction is distorted by its graphical layout.

### Source 2: Japanese household sorting guide

- Original filename: `content_20260331_132719.pdf`
- Japanese label: **北見自治区のごみの分け方・出し方【家庭用】2026年改訂版**
- English label: **Kitami Autonomous Region Household Waste Sorting and Disposal Guide - 2026 Revised Edition**
- Recommended filename: `02_kitami_household_waste_guide_2026_ja.pdf`
- Language: Japanese
- Coverage: Kitami Autonomous Region household waste
- Edition: 2026 revised edition
- Role: Authoritative general rules for categories, bags, preparation, size limits, special handling, and safety
- Derived output: `general_rules.md`
- Verification status: Visually inspected and structured for the prototype

### Source 3: Japanese item dictionary

- Original filename: `content_20260611_094048.pdf`
- Japanese label: **令和8年度 ごみの区分け辞典（北見自治区）**
- English label: **FY2026 Waste Separation Dictionary - Kitami Autonomous Region**
- Recommended filename: `03_kitami_waste_dictionary_2026_ja.pdf`
- Language: Japanese
- Coverage: Kitami Autonomous Region household items
- Length: 40 pages
- Role: Primary authoritative source for item-level waste categories, conditions, exceptions, and disposal instructions
- Derived output: `waste_dictionary.json`
- Verification status: All 1,128 table records extracted; JSON structure validated; representative records spot-checked

### Source 4: English collection calendar

- Original filename: `content_20260611_110932.pdf`
- Japanese label: **令和8年度 北見自治区ごみカレンダー（対象地域・英語版）**
- English label: **FY2026 Kitami Autonomous Region Waste Collection Calendar - Selected Area, English Edition**
- Recommended filename: `04_kitami_collection_calendar_2026_en.pdf`
- Language: English
- Coverage: Same selected prototype collection area as Source 1
- Validity: April 1, 2026 to March 31, 2027
- Role: Main readable calendar source used to structure collection recurrence and exact dates
- Derived output: `collection_schedule.json`
- Verification status: Visually inspected; recurrence rules converted to exact dates; dates validated programmatically

### Source 5: English household sorting guide

- Original filename: `content_20260709_111803.pdf`
- Japanese label: **北見市 ごみの分け方・出し方（北見自治区・英語版）**
- English label: **Kitami City: How to Separate Trash - Kitami Autonomous Region, English Edition**
- Recommended filename: `05_kitami_household_waste_guide_2026_en.pdf`
- Language: English
- Coverage: Kitami Autonomous Region household waste
- Edition date: April 2026
- Role: Resident-facing English explanations and terminology
- Derived output: `english_guide.md`
- Verification status: All 16 pages visually inspected and reorganized into a readable Markdown reference
- Authority limitation: The Japanese dictionary and Japanese rules take priority if the English wording is incomplete or ambiguous.

## 4. Derived knowledge-base files

### `general_rules.md`

- Japanese label: **北見自治区 家庭ごみ基本分別ルール**
- English label: **Kitami Household Waste General Rules**
- Primary source: Source 2
- Function: Category definitions, bag rules, preparation, thresholds, safety logic, and bot decision order

### `waste_dictionary.json`

- Japanese label: **北見自治区 品目別ごみ分別データ**
- English label: **Kitami Item-Level Waste Separation Data**
- Primary source: Source 3
- Function: Searchable item-level classification and disposal instructions

### `collection_schedule.json`

- Japanese label: **北見自治区 対象地域ごみ収集日程データ**
- English label: **Kitami Selected-Area Waste Collection Schedule Data**
- Primary source: Source 4
- Verification source: Source 1
- Function: Neighborhood coverage, recurrence rules, and exact collection dates

### `english_guide.md`

- Japanese label: **北見自治区 ごみ分別英語案内データ**
- English label: **Kitami Waste Sorting English Guidance**
- Primary source: Source 5
- Function: Clear English explanations for LINE Bot responses

### `source_register.md`

- Japanese label: **北見LINE Bot 情報源管理台帳**
- English label: **Kitami LINE Bot Source Register**
- Function: Source identity, authority, provenance, file mapping, and verification record

## 5. Recommended project structure

```text
kitami-linebot/
├── source_documents/
│   ├── 01_kitami_collection_calendar_2026_ja.pdf
│   ├── 02_kitami_household_waste_guide_2026_ja.pdf
│   ├── 03_kitami_waste_dictionary_2026_ja.pdf
│   ├── 04_kitami_collection_calendar_2026_en.pdf
│   └── 05_kitami_household_waste_guide_2026_en.pdf
└── knowledge_base/
    ├── general_rules.md
    ├── waste_dictionary.json
    ├── collection_schedule.json
    ├── english_guide.md
    └── source_register.md
```

## 6. File-management rule

- Keep the original PDF content unchanged.
- Rename only the local copies using the standardized filenames above.
- Do not edit or overwrite official source PDFs.
- Keep the original filename recorded in this register for traceability.
- When Kitami City publishes a new edition, store it as a new file and update the validity and edition fields rather than silently replacing the old source.

## 7. Prototype provenance rule

Every bot answer should ultimately be traceable to:

- one item record in `waste_dictionary.json`, when available;
- the applicable general rule in `general_rules.md`;
- the next valid date in `collection_schedule.json`; and
- `english_guide.md` only for clear English phrasing.

If the required official evidence is missing, the bot must state that the result is uncertain rather than inventing a disposal instruction.
