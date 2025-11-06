# PDF Metadata Extraction using T5 Model

An AI-powered tool that automatically extracts structured metadata from PDF documents using Google's T5 (Text-to-Text Transfer Transformer) language model.

## Overview

This project leverages the T5-large model to intelligently analyze and extract metadata from various document types including legal contracts, research papers, judgments, articles, reports, and general documents. The extracted metadata is returned in a structured JSON format.

## Features

- **Intelligent Metadata Extraction**: Automatically identifies and extracts relevant metadata from document content
- **Multiple Document Type Support**: Works with legal contracts, research papers, judgments, articles, reports, and more
- **Comprehensive Metadata**: Extracts various metadata including:
  - Document type classification
  - Key entities (people, companies, organizations, locations)
  - Important dates (creation, publication, expiration dates)
  - Document structure (titles, chapters, clauses, sections)
  - References (laws, regulations, academic citations, case law)
  - Document summary or abstract
- **JSON Output**: Structured metadata output in JSON format for easy integration
- **Google Colab Support**: Designed to run seamlessly in Google Colab environment
- **Downloadable Results**: Automatically saves and downloads extracted metadata

## Requirements

### Dependencies

- Python 3.7+
- PyPDF2
- transformers
- torch
- google.colab (for Colab environment)

### Hardware Requirements

- **RAM**: Minimum 8GB (16GB+ recommended for t5-large model)
- **GPU**: Optional but highly recommended for faster processing
- **Storage**: ~3GB for model weights

## Installation

### For Google Colab

The notebook includes all necessary installation commands. Simply run the first cell:

```python
!pip install PyPDF2 transformers torch
```

### For Local Environment

```bash
pip install PyPDF2 transformers torch
```

## Usage

### In Google Colab

1. Open the `metadata_extraction.ipynb` notebook in Google Colab
2. Run the first cell to install dependencies
3. Run the second cell to load the T5 model and tokenizer
4. Run the third cell to define extraction functions
5. Run the fourth cell and upload your PDF document when prompted
6. The extracted metadata will be displayed and automatically downloaded as `extracted_metadata.txt`

### Step-by-Step Process

**Step 1: Install Libraries**
```python
!pip install PyPDF2 transformers torch
```

**Step 2: Import and Setup Model**
```python
from transformers import T5ForConditionalGeneration, T5Tokenizer

model_name = "t5-large"
tokenizer = T5Tokenizer.from_pretrained(model_name)
model = T5ForConditionalGeneration.from_pretrained(model_name)
```

**Step 3: Upload and Process PDF**
- Upload your PDF file when prompted
- The tool will automatically extract text and metadata
- Results will be displayed and saved to a downloadable file

## Model Configuration

The project uses the T5-large model with the following parameters:

- **Input Length**: 512 tokens (with truncation)
- **Output Length**: 50-500 tokens
- **Beam Search**: 4 beams
- **Length Penalty**: 2.0 (encourages comprehensive outputs)
- **Early Stopping**: Enabled

### Alternative Models

You can use smaller models for faster processing with slightly reduced quality:

- `t5-small`: Faster, requires less memory (~250MB)
- `t5-base`: Balanced performance (~900MB)
- `t5-large`: Best quality (~3GB) - **Default**

To change the model, simply modify:
```python
model_name = "t5-base"
```

## Technical Details

### PDF Reading

Uses PyPDF2 library to extract text from PDF files. Note that PyPDF2 may have limitations with:
- Complex layouts (tables, multi-column text)
- Scanned documents (OCR not included)
- Password-protected PDFs

### Prompt Engineering

The tool uses carefully crafted prompts to guide the T5 model in extracting structured metadata. The prompt instructs the model to:
- Identify document type
- Extract entities and dates
- Identify document structure
- Find references
- Generate summaries
- Output in JSON format

### Output Format

The extracted metadata is returned as a JSON-formatted string containing fields relevant to the document type.

## Limitations

1. **Token Limits**: Input is truncated to 512 tokens due to T5 model constraints
2. **PDF Complexity**: Complex PDF layouts may not be accurately parsed
3. **Processing Time**: Large documents with t5-large model may take several minutes
4. **Memory Requirements**: T5-large requires significant RAM/VRAM
5. **No OCR**: Scanned PDFs without text layer cannot be processed

## Known Issues

- **EOF Marker Error**: Some PDF files may cause `PdfReadError: EOF marker not found`. This occurs with corrupted or improperly formatted PDFs. Try re-saving the PDF or using a different source.

## Future Enhancements

- [ ] Support for OCR to handle scanned documents
- [ ] Batch processing for multiple documents
- [ ] Custom metadata field selection
- [ ] Support for other document formats (DOCX, TXT, HTML)
- [ ] Fine-tuned model for specific document types
- [ ] Web interface for easier access
- [ ] API endpoint for integration

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is open-source and available for educational and research purposes.

## Acknowledgments

- Google's T5 model from Hugging Face Transformers
- PyPDF2 library for PDF processing
- Google Colab for providing free GPU resources

## Contact

For questions, issues, or suggestions, please open an issue in the repository.

---

**Note**: This tool is designed for educational and research purposes. Always verify extracted metadata for critical applications.

