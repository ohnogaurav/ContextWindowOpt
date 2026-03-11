
# 1. What is this paper about? (Main Idea)

**Topic:** Improving chatbot accuracy using better context selection.

Simple explanation:

* Chatbots like ChatGPT use **conversation history (context)** to answer questions.
* Usually the **entire conversation is sent to the AI**.
* But this creates problems:

  * Too much irrelevant information
  * Higher cost
  * Slower response
  * Wrong answers (hallucinations)

**Idea of the paper:**

Instead of sending **all past conversation**, send **only the most relevant parts**.

This improves:

* accuracy
* speed
* cost efficiency. 

---

# 2. Problem the paper tries to solve

Current chatbot systems have a limitation called **context window**.

Simple points:

* AI models can only process **limited tokens (text)** at once.
* When conversation becomes long:

  * important information gets lost
  * irrelevant messages distract the model

This problem is called:

**Context Overload**

Effects:

* wrong answers
* slow performance
* high API cost. 

---

# 3. Three methods compared in the paper

The research compares **three ways to choose conversation context**.

### 1️⃣ Full Context (Baseline)

Meaning:

* Send **entire conversation history** to the AI.

Example:

User conversation:

```
Q1
Q2
Q3
Q4
Q5
```

AI receives:

```
Q1 Q2 Q3 Q4 Q5
```

Problems:

* too much noise
* expensive
* slower.

---

### 2️⃣ Sliding Window

Meaning:

* Only send **last few messages**.

Example:

Conversation:

```
Q1 Q2 Q3 Q4 Q5
```

Sliding window (k = 3):

```
Q3 Q4 Q5
```

Pros:

* faster
* less tokens

Cons:

* old important info may be lost.

---

### 3️⃣ Relevance-Based Pruning (Proposed Method)

This is the **main idea of the paper**.

Meaning:

Instead of sending **all messages or last messages**,
send **only messages related to the current question**.

How?

Using:

* **embeddings**
* **cosine similarity**

Example:

Conversation:

```
1. talk about hotels
2. talk about flights
3. talk about restaurants
4. talk about beaches
```

User asks:

> "What was the first hotel name?"

System selects:

```
1. talk about hotels
```

and ignores the rest.

Result:

* clearer context
* better answers. 

---

# 4. System workflow (How the system works)

The system works in **5 steps**:

### Step 1 — Query Embedding

User question → converted into **vector embedding**

Example:

```
"What hotel did you suggest?"
```

becomes a vector number representation.

---

### Step 2 — History Embedding

Every conversation message is also converted into vectors.

Stored in a **vector database (FAISS)**.

---

### Step 3 — Similarity Calculation

The system calculates:

**cosine similarity**

between:

```
user question
vs
conversation messages
```

Then it selects **most relevant messages**.

---

### Step 4 — Prompt Construction

The system builds the final prompt:

```
system instructions
+ selected conversation context
+ user question
```

---

### Step 5 — LLM Response

This prompt is sent to the LLM (GPT-3.5 etc).

LLM generates the answer. 

---

# 5. Tools used in the project

Main technologies used:

Embedding model:

* **all-MiniLM-L6-v2**

Vector search:

* **FAISS**

LLM models:

* GPT-3.5-turbo
* GPT-4
* Llama-2

Evaluation datasets:

* Synthetic conversations
* **CoQA dataset**. 

---

# 6. How the system was evaluated

Two types of evaluation were used.

### 1️⃣ Quality Metrics

Measured using:

* **F1 score**
* **ROUGE-L**

Human reviewers also rated responses on:

* factual correctness
* relevance
* fluency.

---

### 2️⃣ Efficiency Metrics

Measured:

* number of tokens used
* response time
* cost per query.

---

# 7. Key Results of the research

The proposed method (**relevance pruning**) showed:

* **35–60% reduction in tokens**
* better relevance
* similar or better accuracy.

Meaning:

Chatbot becomes:

* cheaper
* faster
* more accurate. 

---

# 8. Example from the paper

Conversation had **14 previous messages**.

Question:

> "What was the first hotel name?"

Results:

| Method            | Answer            |
| ----------------- | ----------------- |
| Full context      | Correct but messy |
| Sliding window    | Wrong answer      |
| Relevance pruning | Correct and clear |

Because it selected **only hotel-related messages**. 

---

# 9. Final Conclusion

Main conclusion:

* Giving **too much context to LLM is not always good**.
* Selecting **relevant context** improves performance.

Benefits:

* fewer tokens
* lower cost
* faster responses
* better accuracy.

This method can help build **more efficient real-world chatbots**. 

---
