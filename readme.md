# Chat with PDFs using AWS Bedrock

This repository demonstrates a **Streamlit** application that allows you to:

- **Load and split PDF documents** from a local `data/` folder  
- **Generate embeddings** using the **Amazon Titan** embeddings model via **AWS Bedrock**  
- **Store** those embeddings in a **FAISS** vector store for similarity search  
- **Query** an **AWS Bedrock** language model (like Claude, AI21, Llama, etc.) to answer questions about the documents  

This is essentially a **“Chat PDF”** style project powered by AWS Bedrock.

---

## 1. Project Structure & Flow

1. **Data Ingestion**  
   - We use `PyPDFDirectoryLoader` (from `langchain_community.document_loaders`) to read all PDF files in the `data/` directory.  
   - The text is split into chunks using `RecursiveCharacterTextSplitter` to make it easier for LLMs to handle.

2. **Embeddings & Vector Store**  
   - We use **Amazon Titan** (`amazon.titan-embed-text-v1`) to generate embeddings for each text chunk.  
   - These embeddings are stored in a **FAISS** index, which is saved locally so you don’t have to recreate it every time.

3. **Streamlit Web Application**  
   - A simple UI built with [Streamlit](https://streamlit.io/):
     - **Vectors Update**: Reads the PDFs, embeds them, and creates/updates the FAISS index.  
     - **Claude Output** / **Llama2 Output**: Uses AWS Bedrock to fetch the answer to your query from the relevant text chunks.

4. **AWS Bedrock Model Details**  
   - The code includes two sample functions:
     - `get_claude_llm()`: references the model `ai21.j2-mid-v1` (despite the name “Claude”).  
     - `get_llama2_llm()`: references the model `meta.llama3-3-70b-instruct-v1:0`.  
   - You can adjust these to other Bedrock-supported models (e.g., `anthropic.claude-2`, `amazon.titan-text`, etc.).  
   - **Important**: Some larger models (like Llama 70B or Llama 3) require an **inference profile ARN**. If you encounter a `ValidationException` saying on-demand throughput isn’t supported, either:
     - Provide an `inference_profile_arn="your-arn-here"` to the `Bedrock` constructor, **OR**
     - Use a different model that supports on-demand usage.

---

## 2. Installation & Setup

### a. Clone the Repository

```bash
git clone https://github.com/Suru10/Bedrock_PDF_Chat
cd ChatPDF-Bedrock

### b. Create a Python Virtual Environment (Recommended)

It is highly recommended to create and activate a virtual environment for this project to avoid dependency conflicts. In your terminal or command prompt, run:

```bash
python3 -m venv venv
source venv/bin/activate
# On Windows:
# venv\Scripts\activate
```
Upgrade your pip, and install libraries

```bash
pip install --upgrade pip
pip install -r requirements.txt
```
Add AWS Credentials
Open .env with a text editor and add your AWS credentials and region:
```bash
AWS_ACCESS_KEY_ID=YOUR_AWS_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=YOUR_AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION=us-east-1
```
