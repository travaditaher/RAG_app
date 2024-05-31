# Document Question Answering with Streamlit

This repository contains a Streamlit application that allows users to upload PDF, TXT, or MD files, input a question, and receive a response based on the content of the uploaded documents. The application uses a pre-trained model from Hugging Face to generate embeddings for text chunks and identify the most relevant chunks to answer the user's question.

## Features

- **File Upload**: Upload multiple PDF, TXT, or MD files.
- **Text Parsing**: Extract text from uploaded files and clean it.
- **Chunking**: Split extracted text into manageable chunks.
- **Embedding Generation**: Generate embeddings for text chunks using the BAAI/bge-base-en-v1.5 model.
- **Similarity Matching**: Find the top 3 most relevant text chunks to the user's question.
- **Question Answering**: Generate a response based on the relevant text chunks using the llama3 8b chat model.

## Requirements

- Python 3.8+
- Streamlit
- Transformers
- PyPDF2
- LangChain
- NumPy
- Scikit-learn
- PyTorch
- Ollama

## How to use:

**Upload your documents**: Use the sidebar to upload multiple PDF, TXT, or MD files.

**Ask a question**: Enter your question in the provided input box and press Enter.

**View the response**: The app will display the response generated based on the content of the uploaded documents.

## Detailed Workflow

1. **File Upload**:
   - Users upload multiple PDF, TXT, or MD files through the Streamlit sidebar.
   - Uploaded files are read and processed in memory.

2. **Text Parsing**:
   - PDF files are parsed using `PdfReader` from `PyPDF`.
   - TXT and MD files are read as text.
   - Extracted text is cleaned by removing hyphenation and unnecessary newline characters.

3. **Chunking**:
   - Text is split into chunks using the `RecursiveCharacterTextSplitter` from `langchain`.
   - Chunks are created with a specified size and overlap to ensure context is preserved.

4. **Embedding Generation**:
   - For each text chunk, embeddings are generated using the `BAAI/bge-base-en-v1.5` model from Hugging Face.
   - Mean pooling is applied to obtain a fixed-size embedding for each chunk.
   - Embeddings are saved to disk to avoid redundant computations in future runs.

5. **Similarity Matching**:
   - The user's question is converted into an embedding using the same model.
   - Cosine similarity is computed between the question embedding and each text chunk embedding.
   - The top 3 most relevant chunks are identified based on similarity scores.

6. **Question Answering**:
   - A custom prompt is constructed, incorporating the top 3 relevant chunks.
   - The `ollama.chat` method with model 'llama3' is used to generate a response based on the prompt and the user's question.
   - The response is displayed to the user in the Streamlit interface.
