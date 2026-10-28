# Insurance Policy RAG Chatbot

A document-grounded chatbot that answers questions about a private medical-insurance policy by retrieving relevant passages from a policy PDF and giving those passages to a language model.

> **Educational project.** This repository demonstrates retrieval-augmented generation (RAG). It is not insurance, legal, or medical advice. Always confirm benefit eligibility, exclusions, limits, and approved facilities with the insurer and the policy documents that apply to the member's own plan.

## Overview

This project turns an insurance policy PDF into a searchable question-answering assistant. Instead of asking a model to answer from general knowledge, the workflow retrieves relevant excerpts from `Hospital-Plans-Rules_01Mar26_VHIMR2.pdf` and instructs the model to answer only from that retrieved context.

The notebook supports three interchangeable model-provider modes:

- **Local**: Sentence Transformers for embeddings and Qwen 2.5 1.5B Instruct for answer generation.
- **OpenAI**: OpenAI embeddings and a chat model, using an API key stored securely outside the notebook.
- **Gemini**: Google Gemini embeddings and a Gemini chat model, using an API key stored securely outside the notebook.

## What It Demonstrates

- Loading and extracting text from a PDF with `pypdf`.
- Splitting long policy text into manageable chunks.
- Converting chunks into vector embeddings.
- Finding the most relevant chunks with cosine similarity.
- Building a retrieval context with source chunk IDs and relevance scores.
- Generating responses constrained to the retrieved policy context.
- Returning a simple backend-friendly response through `chatbot_api()` for future frontend integration.

## Workflow

```text
Insurance policy PDF
        |
        v
Text extraction with PyPDF
        |
        v
Chunking
        |
        v
Embeddings + similarity search
        |
        v
Top matching policy passages
        |
        v
LLM answer grounded in retrieved context
```

## Notebook

The main implementation is in:

- `IP_Chatbot_24198170.ipynb`

Key stages in the notebook:

1. Install dependencies.
2. Select `local`, `openai`, or `gemini` as the provider.
3. Upload or point to the policy PDF.
4. Extract and chunk document text.
5. Create chunk embeddings.
6. Retrieve the top matching chunks for a question.
7. Build a context-only prompt and generate a response.
8. Test the chatbot and inspect retrieved source chunks.

## Getting Started

### Option 1: Google Colab

1. Open `IP_Chatbot_24198170.ipynb` in Google Colab.
2. Upload `Hospital-Plans-Rules_01Mar26_VHIMR2.pdf` when prompted, or ensure the filename matches `DOCUMENT_PATH` in the notebook.
3. Run the installation cell.
4. Choose a provider:

```python
PROVIDER = "local"  # or "openai" or "gemini"
```

5. Run the cells in order.
6. Ask a question through `answer_question("Your question here")`.

### Option 2: Local Python Environment

```bash
git clone https://github.com/nizamshaikh12/insurance-policy-rag-chatbot.git
cd insurance-policy-rag-chatbot
python -m venv .venv
```

Activate the environment, then install the dependencies:

```bash
pip install -r requirements.txt
jupyter notebook
```

Open `IP_Chatbot_24198170.ipynb`, update `DOCUMENT_PATH` if necessary, and run the notebook sequentially.

## Provider Setup

### Local mode

The default local setup uses:

- Embedding model: `sentence-transformers/all-MiniLM-L6-v2`
- Chat model: `Qwen/Qwen2.5-1.5B-Instruct`

It requires no API key, but downloading and running the models can take time and may need substantial Colab or local-memory resources.

### OpenAI or Gemini mode

Do not hard-code credentials in the notebook or commit them to GitHub. Set `OPENAI_API_KEY` or `GEMINI_API_KEY` as an environment variable, or use Colab Secrets with notebook access enabled.

## Example Queries

- `Which hospitals are covered?`
- `What are the exclusions in this policy?`
- `What does the policy say about emergency treatment?`
- `How are private and semi-private accommodation defined?`

The strongest answers come from questions that can be answered by the supplied policy text. When the needed information is absent from the retrieved context, the chatbot is instructed to say that it could not find the answer in the document.

## Screenshot

`CHATBOT-Answer.jpg` shows an example response with retrieved source chunks displayed below the answer.

## Technology Stack

- Python
- Jupyter Notebook / Google Colab
- PyPDF
- NumPy
- Sentence Transformers
- Hugging Face Transformers and Accelerate
- OpenAI API (optional)
- Google Generative AI / Gemini (optional)
- ipywidgets

## Responsible Use

- This project uses a policy document as a retrieval source; it does not determine coverage or claims decisions.
- Policy wording, tables of benefits, approved-facility directories, and plan-specific conditions can change. Verify information against the current documents and the relevant policy plan.
- Keep API keys in environment variables or notebook-secret storage; never publish them in source code, notebooks, screenshots, or commits.
- Before publishing a project based on third-party documents, ensure that you have permission to share them and that no personal or sensitive information is included.

## Author

**Nizam Shaikh**  
FinTech professional and MSc Financial Technology student at National College of Ireland.
