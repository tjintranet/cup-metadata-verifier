# CUP Metadata Verifier

A lightweight browser-based tool for verifying Cambridge University Press book metadata against CUP print specification rules, before submission.

## Overview

Upload an Excel metadata file and the tool will instantly validate every record against the full set of CUP specification rules. Results are displayed per record with pass/fail status and detailed messages for any failures. Summary reports can be downloaded for review or sign-off.

No installation, no server, no build process required — the entire application is a single HTML file that runs locally in your browser.

---

## Installation

1. Download `cup-data-verifier.html`
2. Open it in a modern web browser

That's it. No other files are needed.

---

## Usage

### 1. Upload Your Excel File

Drag and drop your `.xlsx` or `.xls` file onto the upload area, or click **Browse Files** to select it. The tool reads the first sheet in the workbook.

Your Excel file should include the following columns (column order does not matter):

| Original Column Name | Description |
|---|---|
| `Master ISBN 13` | 13-digit ISBN |
| `full_title_for_export` | Book title |
| `pod_xml_TrimHeight` | Trim height in mm |
| `pod_xml_TrimWidth` | Trim width in mm |
| `pod_ExtentMod2` | Number of pages |
| `pod_xml_TJ_PaperStock` | Paper type |
| `pod_Colourspace` | Colour type (Mono/Colour) |
| `pod_xml_TJ_QualityOption` | Quality route (Standard/Premium) |
| `pod_ColourBook_fullBleed` | Full bleed (Y/N) |
| `pod_TJ_BindType_c` | Binding style (Cased/Limp) |
| `pod_CoverLaminate` | Cover lamination type |
| `pod_xml_Jacket` | Jacket (Y/N) |
| `pod_xml_TJ_JacketLamination` | Jacket lamination type |
| `pod_xml_TJ_FlapSize` | Flap size |
| `pod_xml_TJ_WibalinColour` | Wibalin colour |
| `pod_xml_SetISBN` | Set ISBN |

Only the columns listed above are processed. Any additional columns in the file are ignored.

### 2. Review Results

Once the file is loaded, results appear immediately. Each record is shown as a card displaying:

- **ISBN** and title
- **Passed / Failed** status badge
- A detailed breakdown of every validation check

Cards with a **green border** have passed all checks. Cards with a **red border** have one or more failures, with the specific issues listed.

A summary bar at the top shows:

- Total records processed
- Number currently shown
- Count of passed and failed records
- Breakdown by binding style, colour type, quality route, and jacket count

### 3. Filter and Export

Use the **Show failed only** toggle to focus on records that need attention.

Two report downloads are available:

- **Download Full Summary** — all records, pass and fail
- **Download Failed Summary** — only records with failures

Reports are plain text files, suitable for sharing or attaching to a sign-off workflow.

Individual record results can also be copied to the clipboard using the clipboard icon on each card.

### 4. Clear and Start Again

Click **Clear** to reset the tool and upload a new file.

---

## Data Transformations

The tool automatically applies the following transformations before validating, matching the values expected by the CUP specification.

### Paper Type Remapping

| Value in Excel | Mapped to |
|---|---|
| `Cream 80gsm` | `CUP MunkenPure 80 gsm` |
| `White 80gsm` | `Navigator 80 gsm` |
| `Matte Coated 90gsm` + Standard route | `Clairjet 90 gsm` |
| `Matte Coated 90gsm` + Premium route | `Magno Matt 90 gsm` |

### Lamination Remapping

| Value in Excel | Mapped to |
|---|---|
| `Matte` | `Matt` |

---

## Validation Rules

Each record is checked against the following eight rules.

### 1. Required Fields

The following fields must be present and non-empty: ISBN, Trim Height, Trim Width, Extent, Paper, Colour, Quality, Binding Style.

### 2. Binding Style

Must be one of:

- `Cased`
- `Limp`

### 3. Paper Type

Must be one of:

- `CUP MunkenPure 80 gsm`
- `Navigator 80 gsm`
- `Clairjet 90 gsm`
- `Magno Matt 90 gsm`

### 4. Colour

Must be one of:

- `Mono`
- `Colour`

### 5. Quality / Route

Must be one of:

- `Standard`
- `Premium`

### 6. Trim Size

The width × height combination must be one of the following approved sizes (mm):

`140×216`, `152×229`, `156×234`, `170×244`, `189×246`, `178×254`, `203×254`, `216×280`

### 7. Colour / Paper Compatibility

| Paper | Allowed Colour |
|---|---|
| CUP MunkenPure 80 gsm | Mono only |
| Navigator 80 gsm | Mono only |
| Clairjet 90 gsm | Colour only |
| Magno Matt 90 gsm | Mono or Colour |

### 8. Route / Paper Compatibility

| Paper | Allowed Route |
|---|---|
| CUP MunkenPure 80 gsm | Standard only |
| Navigator 80 gsm | Standard only |
| Clairjet 90 gsm | Standard only |
| Magno Matt 90 gsm | Premium only |

---

## Browser Compatibility

- Chrome 90+ (recommended)
- Firefox 88+
- Safari 14+
- Edge 90+

---

## Technical Details

All processing happens entirely within your browser. No data is sent to any server and no internet connection is required after the page has loaded.

**Libraries used (loaded from CDN on first open):**

- [SheetJS](https://sheetjs.com/) — Excel file parsing
- [Bootstrap 5](https://getbootstrap.com/) — UI framework
- [Bootstrap Icons](https://icons.getbootstrap.com/) — Icons

---

## Troubleshooting

**File won't load**
Ensure the file is `.xlsx` or `.xls` format and is not open in Excel at the same time.

**Records show as failed unexpectedly**
Check that values in the Excel file exactly match the expected formats — in particular paper type names and trim size values. Leading or trailing spaces can cause mismatches.

**Trim size fails but looks correct**
Trim sizes are compared as `width×height`. Ensure the trim width and height columns are not transposed.

**All records fail Required Fields**
The column headers in your Excel file may not match the expected names exactly. Check the column name list in the Usage section above.

---

**Version:** 1.0  
**Last Updated:** March 2026  
**Created by:** Colin for Cambridge University Press
