# PDF Metadata Extraction using OpenAI GPT-4o-mini

An AI-powered tool that automatically extracts structured metadata from PDF documents using OpenAI's GPT-4o-mini model with advanced prompt engineering techniques.

## Overview

This project leverages OpenAI's GPT-4o-mini API to intelligently analyze and extract metadata from various document types including research papers, legal contracts, conference papers, articles, reports, judgments, and general documents. The extracted metadata is returned in a structured JSON format with guaranteed valid JSON output.

## Features

- **Advanced Prompt Engineering**: Comprehensive prompt design with clear instructions and examples
- **Multiple Document Type Support**: Works with research papers, legal contracts, conference papers, judgments, articles, reports, and more
- **Comprehensive Metadata Extraction**: Automatically extracts:
  - Document type classification
  - Authors and parties involved
  - Titles and key topics
  - Publication/creation/judgment dates
  - Locations and jurisdictions
  - Journal names, DOI, ISBN, volume, issue
  - Key sections and structural elements
  - References (citations, case law, laws)
  - Keywords and summaries
- **Guaranteed Valid JSON Output**: Uses OpenAI's JSON mode for reliable structured data
- **Google Colab Integration**: Seamless file upload and download functionality
- **Detailed Documentation**: Extensively commented code for learning and customization
- **Cost Control**: Configurable text truncation to manage API costs
- **Deterministic Output**: Uses temperature=0 for consistent results

## Requirements

### Dependencies

- Python 3.7+
- openai
- PyPDF2
- google.colab (for Colab environment)

### API Requirements

- **OpenAI API Key**: Required to use GPT-4o-mini model
- **API Credits**: Pay-per-use pricing based on tokens processed

### Hardware Requirements

- **RAM**: 4GB+ (no local model loading required)
- **GPU**: Not required (cloud-based API)
- **Internet**: Required for API calls

## Installation

### For Google Colab

The notebook includes all necessary installation commands:

```python
!pip install PyPDF2 transformers torch
!pip install openai
```

### For Local Environment

```bash
pip install PyPDF2 openai
```

## Usage

### In Google Colab

1. Open the `metadata_extraction .ipynb` notebook in Google Colab
2. Run Cell 1 to install PyPDF2, transformers, and torch
3. Run Cell 2 to install OpenAI library
4. Run Cell 4 to import libraries and enter your OpenAI API key when prompted
5. Run Cell 6 to define the `read_pdf()` function
6. Run Cell 8 to define the `create_prompt()` function
7. Run Cell 10 to execute the main extraction process:
   - Upload your PDF file when prompted
   - The tool will automatically extract text and metadata
   - Results will be displayed in console
   - Metadata will be saved and downloaded as `extracted_metadata.json`

### Step-by-Step Process

**Step 1: Install Required Libraries**

```python
!pip install PyPDF2 transformers torch
!pip install openai
```

**Step 2: Setup OpenAI Client**

```python
from openai import OpenAI
from getpass import getpass

api_key = getpass("Enter your OpenAI API Key: ")
client = OpenAI(api_key=api_key)
MODEL_NAME = "gpt-4o-mini"
```

**Step 3: Upload and Process PDF**

The notebook will prompt you to upload a PDF file, then automatically:
- Extract text using PyPDF2
- Send it to GPT-4o-mini with structured prompts
- Parse and format the JSON response
- Display results and download the metadata file

## Model Configuration

The project uses OpenAI's **GPT-4o-mini** model with the following parameters:

- **Model**: `gpt-4o-mini` (cost-effective, high-quality)
- **Temperature**: `0` (deterministic, consistent outputs)
- **Max Tokens**: `1500` (sufficient for detailed metadata)
- **Response Format**: `json_object` (guaranteed valid JSON)
- **Character Limit**: `15,000 characters` (configurable for cost control)

### Alternative Models

You can switch to other OpenAI models by changing the `MODEL_NAME` variable:

- `gpt-4o-mini`: Fast, cost-effective, excellent quality - **Default**
- `gpt-4o`: Higher accuracy, more expensive
- `gpt-3.5-turbo`: Lower cost, good for simple documents

```python
MODEL_NAME = "gpt-4o"
```

## Technical Details

### PDF Reading

Uses PyPDF2 library to extract text from PDF files page by page. Limitations include:
- Complex layouts (tables, multi-column text) may not parse perfectly
- Scanned documents without text layer require OCR preprocessing
- Password-protected PDFs are not supported

### Prompt Engineering

The tool uses advanced prompt engineering with:

1. **Clear Role Definition**: System message establishes the AI as a metadata extraction expert
2. **Structured Instructions**: Detailed explanation of what metadata means
3. **Categorized Guidelines**: Who, What, When, Where, Structure, References framework
4. **Concrete Examples**: Full example with research paper input and expected JSON output
5. **Critical Instructions**: Explicit JSON-only output requirement
6. **Context Window**: Up to 15,000 characters of document content

### JSON Output Guarantee

The implementation uses multiple layers to ensure valid JSON:

1. **OpenAI JSON Mode**: `response_format={"type": "json_object"}`
2. **System Message**: Explicit instruction to return JSON only
3. **User Prompt**: Critical instruction emphasizing JSON-only output
4. **Python Parsing**: `json.loads()` and `json.dumps()` with error handling
5. **Fallback**: Returns raw response if JSON parsing fails for debugging

### Error Handling

Comprehensive error handling includes:
- Try-except blocks for API calls
- JSON parsing error catching
- Safe navigation with optional chaining concepts
- Detailed error messages with exception details
- Graceful degradation with error JSON output

## Cost Estimation

OpenAI GPT-4o-mini pricing (as of 2024):
- **Input**: ~$0.15 per 1M tokens
- **Output**: ~$0.60 per 1M tokens

For a typical research paper (15,000 characters ≈ 3,750 tokens input + 500 tokens output):
- **Cost per document**: ~$0.001 (approximately $1 per 1000 documents)

Use the `max_chars` parameter to control costs for very large documents.

## Limitations

1. **API Dependency**: Requires internet connection and active OpenAI API key
2. **Character Limits**: Input truncated to 15,000 characters by default (configurable)
3. **PDF Complexity**: Complex layouts may not be accurately parsed by PyPDF2
4. **No OCR**: Scanned PDFs without text layer cannot be processed
5. **API Costs**: Usage incurs charges based on OpenAI pricing
6. **Rate Limits**: Subject to OpenAI API rate limits based on your account tier

## Output Example

Sample output for a conference paper:

```json
{
  "document_type": "Conference Paper",
  "title": "Contour-based Repositioning of lower limbs of the GHBMC Human Body FE Model",
  "authors": [
    "Aditya Chhabra",
    "Sachiv Paruchuri",
    "Dhruv Kaushik",
    "Kshitij Mishra",
    "Anoop Chawla",
    "Sudipto Mukherjee",
    "Rajesh Malhotra"
  ],
  "conference_name": "IRCOBI Conference 2017",
  "reference_number": "IRC-17-67",
  "funding": "European Union Seventh Framework Programme grant agreement n°605544 (PIPER project)",
  "keywords": [
    "Contour-based Repositioning",
    "Human Body Model",
    "Injury Prediction",
    "Posture-specific Models"
  ],
  "summary": "Presents a contour-based repositioning technique for lower limbs of the GHBMC Human Body Model, improving posture-specific human body models for injury prediction"
}
```

## Future Enhancements

- [ ] Support for OCR to handle scanned documents
- [ ] Batch processing for multiple documents
- [ ] Custom metadata field selection
- [ ] Support for other document formats (DOCX, TXT, HTML)
- [ ] Web interface for easier access
- [ ] Async processing for large batches
- [ ] Support for other LLM providers (Anthropic, Azure OpenAI)
- [ ] Token usage tracking and cost reporting
- [ ] Metadata validation and quality scoring

## Security Notes

- API keys are entered securely using `getpass()` (hidden input)
- Never commit API keys to version control
- Consider using environment variables for production
- Review OpenAI's data usage policies for sensitive documents

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is open-source and available for educational and research purposes.

## Acknowledgments

- OpenAI for the GPT-4o-mini API
- PyPDF2 library for PDF processing
- Google Colab for providing free computing resources

---

**Note**: This tool is designed for educational and research purposes. Always verify extracted metadata for critical applications. Ensure compliance with OpenAI's usage policies and your organization's data handling requirements.

