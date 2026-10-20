# Document Question-Answering Assistant

## Introduction
A Python-based application that allows users to upload a document and ask questions based on its content. It retrieves relevant passages to provide document-grounded answers.

## Summary
- Upload a document and enter questions about its content.
- Retrieve relevant text passages using **TF-IDF and cosine similarity**.
- Display answers based on the provided document.
- Return an appropriate message when relevant information is not found.


## 5. Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core application logic |
| Jupyter Notebook | Development environment and interactive interface |
| Scikit-learn | TF-IDF vectorization and cosine similarity |
| Pandas | Organizing question results and similarity scores |
| NumPy | Numerical operations |
| IPython Widgets | Document upload and question input controls |
| IPython Display | Rendering answers, tables, and source passages |
| Document parser | Extracting text from supported document formats |

The project uses local, open-source components and does not require a paid language-model API for TF-IDF-based retrieval.


## Workflow
1. Upload a document.
2. Extract and preprocess its text.
3. Split the text into smaller passages.
4. Convert passages into TF-IDF vectors.
5. Compare the question with the passages using cosine similarity.
6. Return the most relevant passage or an appropriate fallback message.


```text
             START
               |
               v
        Upload Document
               |
               v
         Extract Text
               |
               v
       Clean and Chunk Text
               |
               v
      Fit TF-IDF Vectorizer
               |
               v
       User Enters Question
               |
               v
      Transform Question
        Using TF-IDF
               |
               v
     Calculate Cosine Similarity
               |
               v
       Rank Document Chunks
               |
               v
       Check Relevance Score
               |
        +------+------+
        |             |
   Sufficient      Insufficient
     Evidence        Evidence
        |             |
        v             v
  Construct Answer   Show Fallback
        |
        v
  Display Answer and
  Supporting Passages
        |
        v
       END
```


## Key Features
- Document upload and text extraction
- Question answering based on document content
- Relevant passage retrieval
- Interactive notebook interface
- Fallback response for unsupported questions

## How to Run
1. Clone this repository.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Open the Jupyter Notebook in VS Code or Jupyter Notebook.
4. Run all cells, upload a document, and start asking questions.

## Note
This project is for educational purposes. Answer quality depends on the document content and the relevance of the retrieved passages.
