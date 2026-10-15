# RAG Pipeline

A small Retrieval-Augmented Generation (RAG) notebook that searches a text corpus about any given context and sends the most relevant passages to Gemini to answer a question.

## 📁 Project Files:

- `First_rag (1).ipynb` contains the data, chunking, embedding, indexing, retrieval, prompt, and generation steps.
- `requirements.txt` lists the Python packages used by the notebook.
- `.env.example` shows the required environment variable without containing a real key.
- `.env` stores the local Gemini API key and is excluded from Git by `.gitignore`.

## How It Works:

1. The notebook stores a Python tutorial as its source text.
2. `RecursiveCharacterTextSplitter` splits the text into chunks of 200 characters with 50 characters of overlap.
3. `HuggingFaceEmbeddings` uses `sentence-transformers/all-MiniLM-L6-v2` to create vectors for the chunks.
4. FAISS stores those vectors and retrieves the three closest chunks for a question.
5. A prompt combines the question with the retrieved context.
6. `ChatGoogleGenerativeAI` sends the prompt to `gemini-3.8-flash` and returns an answer based on the context.

## 📦 Requirements

- Python 3.11
- Conda (recommended on Windows for the CPU-only PyTorch and FAISS packages)
- A Gemini API key
- Internet access for package installation, the first embedding-model download, and Gemini API requests

The direct Python dependencies are `langchain`, `langchain-community`, `langchain-google-genai`, `langchain-text-splitters`, `faiss-cpu`, `python-dotenv`, `sentence-transformers`, and `ipykernel`. Their transitive dependencies are installed automatically.

## 🛠️ Installation

From the project directory, create and activate the Conda environment:

```powershell
conda create --name ragnev python=3.11 pip -y
conda activate ragnev
```

On Windows, install the CPU-only embedding and vector-search dependencies from Conda Forge, then install the remaining project requirements:

```powershell
conda install --override-channels --channel conda-forge faiss-cpu pytorch-cpu sentence-transformers -y
python -m pip install -r requirements.txt
```

## 🔐 API Key Setup
1. Copy `.env.example` to `.env` in the project directory.
2. Set the key in `.env`:

	```env
	GEMINI_API_KEY=your_gemini_api_key_here
	```

3. Keep the real key in `.env` only.

The notebook calls `load_dotenv()` and stops with a clear error if `GEMINI_API_KEY` is missing.

## ▶️ Run the Notebook

1. Open `First_rag (1).ipynb` in VS Code.
2. Select the `ragnev` Python kernel.
3. Run the notebook from the top so imports, the API key, documents, embeddings, and FAISS index are initialized in order.
4. Edit the `query` in the final cell and run it to ask another question.

The first run may take longer while the embedding model is downloaded and initialized. Subsequent questions reuse the in-memory FAISS index as long as the kernel remains running.

## 📚 What This Project Demonstrates

- Preparing source text for retrieval with chunking and overlap.
- Turning text into semantic vectors with an embedding model.
- Searching vectors with FAISS instead of sending the full corpus to the language model.
- Grounding a Gemini response in retrieved context through a prompt.
- Loading API credentials from an ignored `.env` file.
- Separating retrieval time from model-response time when diagnosing a slow RAG query.

Chunk size, overlap, and the number of retrieved passages (`k`) affect both answer quality and latency. For a larger corpus, consider persisting and reloading the FAISS index instead of rebuilding it after every kernel restart.

## ✅ Final Cell Output

The final cell was run with the query `what is oops?`. Its answer was:

> Based on the provided context, OOP stands for **object-oriented programming**. The context does not provide a full definition, but it identifies its core principles and concepts as:
>
> - Classes and Objects
> - Encapsulation
> - Inheritance
> - Polymorphism
> - Abstraction (abstract classes)
> - Iterators

### 🖼️ Screenshots

![RAG notebook final cell](<Final-Output/Screenshot 2026-10-15 122731.png>)

![RAG notebook output](<Final-Output/Screenshot 2026-10-15 123357.png>)
