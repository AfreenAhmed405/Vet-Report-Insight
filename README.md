# Vet-Report-Insight

This project automates the extraction of structured clinical data from veterinary medical PDF reports using a local language model API. It processes multiple PDF files, extracts relevant biomarker values and clinical details based on a provided CSV list of items, and outputs the results into a consolidated CSV file.

---

## Features

- Extracts text from multiple veterinary PDF reports using PyMuPDF (`fitz`)
- Reads a CSV containing target items (biomarkers, clinical measurements) to extract
- Uses a local LLM API (e.g., Mistral or TinyLlama) via HTTP to extract JSON-formatted data through prompt engineering
- Supports extraction of numeric values ("results") and exact text snippets ("details")
- Handles missing items gracefully by returning `"N/A"`
- Merges and consolidates extracted data into a single CSV output

---

## Requirements

- Python 3.7+
- Libraries: `pandas`, `fitz` (PyMuPDF), `httpx`
- A running local language model API compatible with the prompt format (e.g., Mistral 7B instruct model or TinyLlama)

---

## Setup

1. Install dependencies:

```bash
pip install pandas pymupdf httpx
```
2. Place your veterinary PDF reports in the project folder. Update the `pdf_files` dictionary in the script with your filenames.
   
4. Prepare a CSV file (`table.csv`) listing the items to extract with columns:
  - `filename` (matching keys in `pdf_files`)
  - `items` (biomarkers or measurement names)
  - Optionally, a `details` column to specify if detailed text extraction is needed.

4. Ensure the local LLM API is running and accessible.

## Usage

1. Place your veterinary PDF reports in the project folder and update the `pdf_files` dictionary with their filenames.
2. Prepare the `table.csv` file with the required extraction items and optional details.
3. Ensure the local LLM API is running at `http://localhost:11434/api/generate`.
4. Run the Python script. It will:

   - Extract text from each PDF file.
   - Use the local LLM API to extract specified values and detailed text from the reports based on the CSV input.
   - Merge and compile extracted data into a single DataFrame.
   - Save the final extracted results into `output_table.csv`.

5. Review the `output_table.csv` for the extracted values and details.

---

## How It Works

- The script reads each PDF file, extracts its text, and matches it with the requested items listed in the CSV.

- For each PDF, it sends prompts to the local LLM API asking to extract specific values or details in JSON format.

- Results are parsed and combined into a structured table for further analysis or use.

---

## Notes

- The script strips extra whitespace in the CSV to improve matching accuracy.

- It supports both value extraction and detailed text extraction based on the CSV `details` column.

- The local LLM API should support JSON output parsing as per the prompt instructions.

- Large PDFs or many API calls may increase processing time and resource usage.

---

## Output

- Extracted values and details are saved in `output_table.csv` in a tabular format with columns:
  - `items`
  - `results` (extracted values)
  - `details` (if applicable)
