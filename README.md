# Document Question-Answering Assistant

## 1. Project Overview

The **Document Question-Answering Assistant** is a Python-based application that enables users to upload a document and ask questions about its content through an interactive Jupyter Notebook interface.

The application uses Natural Language Processing (NLP) and information retrieval to identify relevant passages from the uploaded document. It then constructs a concise answer from the retrieved information and displays the supporting source passages for verification.

The main objective is to provide **document-grounded question answering**, where responses are based on the supplied document rather than general knowledge.

**Project:** CodeNova Internship — Task 4  
**Category:** Retrieval and Grounded Question Answering  
**Language:** Python  
**Interface:** Jupyter Notebook with interactive widgets  
**Retrieval Algorithm:** TF-IDF with cosine similarity  
**Processing:** Local document processing

## 2. Problem Statement

Reading lengthy documents to find specific information can be time-consuming. Traditional keyword searches may also return too much text without directly answering the user's question.

This project addresses the problem by retrieving relevant document passages in response to a natural-language question and presenting the information in a more useful format.

The system is designed to:

- Accept a document provided by the user.
- Extract and divide its text into passages.
- Identify passages relevant to a question.
- Construct a concise answer from the retrieved information.
- Display the source passages used to support the answer.
- Reject questions that cannot be answered from the available document content.

## 3. Main Algorithm

The core of the application is a **TF-IDF-based information retrieval algorithm using cosine similarity**.

TF-IDF measures the importance of terms in a passage relative to the collection of passages. Cosine similarity compares the numerical representation of a question with the representations of the document passages.

The algorithm consists of the following stages.

### Step 1: Document Ingestion

The user uploads a document through the notebook interface.

The application extracts the document's textual content using the configured document-reading method. The extracted text becomes the input for the subsequent processing stages.

### Step 2: Text Preprocessing and Chunking

The extracted text is cleaned and divided into smaller passages, also called chunks.

Chunking allows the system to retrieve specific sections rather than treating the entire document as a single unit.

For example, a document discussing artificial intelligence, cloud computing, and cybersecurity can be divided into passages covering those topics separately.

The resulting passages are represented as:

\[
D = \{d_1,d_2,\ldots,d_n\}
\]

where \(d_i\) represents an individual document passage and \(n\) is the total number of passages.

### Step 3: TF-IDF Vectorization

Each passage is converted into a numerical vector using Term Frequency–Inverse Document Frequency.

TF-IDF combines two measurements:

**Term Frequency (TF):** Measures how frequently a term occurs within a passage.

**Inverse Document Frequency (IDF):** Reduces the importance of terms that appear across many passages and gives more weight to terms that help distinguish passages.

A simplified TF-IDF expression is:

\[
\operatorname{TFIDF}(t,d)
=
\operatorname{TF}(t,d)
\times
\log\left(\frac{N}{\operatorname{DF}(t)}\right)
\]

where:

- \(t\) is a term.
- \(d\) is a passage.
- \(N\) is the number of passages.
- \(\operatorname{DF}(t)\) is the number of passages containing the term.

The exact calculation depends on the vectorizer configuration. Scikit-learn's default implementation also applies its configured normalization and smoothing.

This process converts the document passages into numerical vectors that can be compared mathematically.

### Step 4: Question Vectorization

When the user submits a question, the question is transformed using the **same fitted TF-IDF vectorizer** used for the document passages.

This is essential because the question and passages must use the same vocabulary and feature space to calculate meaningful similarity scores.

Let the question vector be \(q\).

### Step 5: Similarity Calculation

The system calculates the cosine similarity between the question vector and each passage vector.

The formula is:

\[
\operatorname{Similarity}(q,d_i)
=
\frac{q\cdot d_i}
{\|q\|\|d_i\|}
\]

where:

- \(q\) is the question vector.
- \(d_i\) is the vector representing passage \(i\).
- \(q\cdot d_i\) is their dot product.
- The denominator is the product of their vector magnitudes.

A higher similarity score indicates a stronger match between the terms in the question and those in the passage.

The passages are ranked in descending order of similarity, and the most relevant passages are selected for answer construction.

**Important:** Cosine similarity measures textual similarity, not whether a passage is factually correct or fully answers the question.

### Step 6: Relevance Filtering

Selecting the highest-scoring passage alone is not sufficient. Even an irrelevant passage can receive the highest score if every other passage is less similar.

Therefore, the system needs a relevance threshold or another explicit relevance check to decide whether the retrieved content sufficiently supports an answer.

If no passage meets the configured relevance requirement, the application returns:

`Information not found in provided document.`

The threshold should be validated against representative questions and documents rather than treated as a universal value.

### Step 7: Answer Construction

The selected passages are used to construct a response to the user's question.

This stage is important because **retrieval and answer generation are different operations**:

- Retrieval identifies the passages that are likely to contain the answer.
- Answer construction extracts or summarizes the information needed to answer the question.

Simply concatenating the retrieved passages can produce an answer that repeats large portions of the document without addressing the question directly.

The application should therefore keep the answer separate from the supporting evidence, remove unnecessary surrounding text where appropriate, and avoid including information that does not help answer the question.

The exact quality of this stage depends on the answer-construction logic implemented in the notebook. TF-IDF and cosine similarity alone retrieve relevant text; they do not independently generate a natural-language summary.

### Step 8: Source Passage Display

After constructing the answer, the application displays the supporting passages and their similarity scores.

This makes the result easier to verify because the user can compare the response with the original document content.

The final output contains:

1. The submitted question.
2. The concise answer.
3. The supporting source passages.
4. Their retrieval similarity scores, where available.
5. A fallback message when sufficient evidence is unavailable.

## 4. Algorithm Flow

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

## 6. Application Interface

The notebook provides an interactive interface with two main sections.

### Document Processing

The user selects a document and clicks **Process Document**. The application extracts the text, creates passages, and prepares them for retrieval.

The interface displays processing information such as the document name, word count, and number of passages, where implemented.

### Question Answering

After processing the document, the user enters a question and clicks **Ask Question**.

The application displays the question, its answer, and the supporting source passages. This allows the user to examine both the result and the evidence behind it.

## 7. Example

Suppose the uploaded document contains the following passage:

"Artificial intelligence is a field of computer science that develops systems capable of performing tasks that normally require human intelligence. These tasks include recognizing patterns, understanding language, solving problems, and making predictions."

**Question:** What is artificial intelligence?

**Expected concise answer:**

Artificial intelligence is a field of computer science that develops systems capable of performing tasks that normally require human intelligence, such as recognizing patterns, understanding language, solving problems, and making predictions.

**Supporting evidence:** The passage defining artificial intelligence in the uploaded document.

The response should communicate the relevant information without unnecessarily reproducing unrelated sections of the document.

For a question such as "What is the capital of Japan?", if the document does not contain that information, the application should display the configured fallback message instead of answering from external knowledge.

## 8. Evaluation and Testing

The application can be evaluated using a set of questions whose answers are known to be present or absent from the test documents.

| Evaluation area | What to verify |
|---|---|
| Document processing | Text is extracted and passages are created correctly |
| Retrieval relevance | Retrieved passages contain information related to the question |
| Answer relevance | The response directly addresses the question |
| Answer conciseness | Unnecessary sections of the document are not repeated |
| Source transparency | Displayed passages support the answer |
| Unsupported questions | Questions without sufficient evidence trigger the fallback |
| Consistency | Repeated questions produce consistent retrieval results |

For a more rigorous evaluation, retrieval Precision@k and Recall@k can measure how effectively the system finds relevant passages. Answer relevance and factual support can be assessed against manually prepared reference answers.

No benchmark scores are reported here because a validated evaluation dataset and measured results have not been provided.

## 9. Limitations

- **Lexical matching:** TF-IDF primarily captures term overlap and does not fully understand semantic meaning.
- **Question phrasing:** A question may retrieve weak passages if its vocabulary differs significantly from the document.
- **Threshold sensitivity:** A poorly chosen relevance threshold may reject useful passages or accept irrelevant ones.
- **Answer construction:** Retrieval alone cannot guarantee a concise, complete natural-language answer.
- **Context fragmentation:** Splitting text into passages may separate related information.
- **Document quality:** Incorrect text extraction can reduce retrieval accuracy.
- **Evidence limitations:** Similarity scores do not establish factual correctness.

## 10. Future Improvements

- Replace or supplement TF-IDF with free, locally running sentence-transformer embeddings for semantic retrieval.
- Introduce a reranking stage to improve the ordering of retrieved passages.
- Improve answer construction through extractive summarization or a locally hosted open-source language model.
- Use overlapping chunks to preserve context between passages.
- Add support for scanned documents through OCR.
- Build a labelled question-and-answer test set to measure retrieval and answer quality.
- Add source highlighting and document-page references where supported by the parser.

## 11. Running the Project

### Prerequisites

- Python installed on the system.
- Jupyter Notebook or VS Code with the Jupyter extension.
- The libraries used by the notebook.

### Installation

Create and activate a virtual environment in PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the dependencies:

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Launch Jupyter Notebook:

```powershell
python -m notebook
```

Alternatively, open the notebook directly in VS Code and select the Python interpreter from `.venv`.

Run the application cell, upload a document, process it, and begin asking questions.

## 12. Conclusion

The Document Question-Answering Assistant demonstrates a retrieval-based approach to answering questions from user-provided documents.

Its main algorithm combines TF-IDF vectorization with cosine similarity to rank document passages according to their relevance to a question. Relevance filtering, answer construction, and source passage display build on this retrieval stage to provide a more useful and transparent question-answering experience.

The project also demonstrates an important limitation of information retrieval: finding a passage related to a question is not the same as producing a concise, correct answer. Separating retrieval from answer construction and providing an explicit fallback are essential for improving the reliability of document-grounded responses.

**Disclaimer:** The system's answers depend on document quality, retrieval settings, relevance filtering, and answer-construction logic. Supporting passages should be checked when accuracy is important.
