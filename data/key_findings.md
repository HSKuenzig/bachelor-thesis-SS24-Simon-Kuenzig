

#########REQUIREMENTS#########

The abstract/summary primarily outlines the results achieved in relation to the stated objectives.
When drafting the summary, key terms should be used
. If any questions were posed, these can also be answered here. This should enable a reader to grasp the findings of the thesis simply by reading the introduction and the summary.
Checklist:
1. A brief, clear overview of the subject matter, research question and objectives of the thesis
2. Methodology used
3. Presentation of the key findings and the resulting
conclusions.
For academic theses, approximately 200 to 250 words are sufficient.






########EXAMPLE########

Extracting knowledge from books is both essential and complex. In medical informatics in particular, simple and comprehensive access to knowledge is crucial. In this study, a pre-trained language model was used to make the content of the book *Health Information Systems* by Winter et al. (2023) more accessible and easier to use. During training, the quality of the model was evaluated at various points in time. To this end, the model answered examination questions from the book and from modules at the University of Leipzig, the content of which builds on the book. Finally, a comparison was carried out between the training timepoints, the model that was not trained further, and the state-of-the-art model GPT-4. With a macro-F1 score of 0.7, the GPT-4 model achieved the highest accuracy in answering the examination questions. The other models were unable to match this performance. However, performance improved from an initial MacroF1 score of 0.13 to 0.33 through continuous training. The results demonstrate a significant improvement in performance through this approach and provide a basis for future extensions. This demonstrates the feasibility of answering questions on healthcare information systems and solving a sample exam using further-trained language models; however, these models do not achieve practical application, as their performance falls short of the current state of the art and the models presented here are unable to answer a large proportion of the questions posed completely correctly.















########KEY FINDINGS##########

# Key Findings: Introduction

This document summarizes the core background, problem statements, motivations, goals, and tasks extracted from the introduction in [Chapters/1Introduction.tex](Chapters/1Introduction.tex). These key findings also serve as a comprehensive foundation for drafting the thesis's abstract.

---

## 1. Abstract-Relevant Core Components

### Context and Relevance
* **Healthcare & Knowledge Access:** Reliable, efficient, and intuitive access to clinical data, information, and domain knowledge is fundamental to healthcare quality, practice, and education. Medical informatics is the engineering driver behind these systems.
* **Literature as a Resource:** In education, students and practitioners mainly acquire knowledge through comprehensive textbooks like *Health Information Systems: Architectures and Strategies* (referred to as \citet{bb2}).

### The Problem (`The Fragmentation-Evaluation Gap`)
* **Information Fragmentation:** Relevant knowledge is highly fragmented across different sections and chapters of textbooks, making manual extraction tedious, time-consuming, and error-prone.
* **Limitations of Existing Tools:** Previous semantic technologies (e.g., the SNIK ontology) are limited when processing complex, cross-linked queries or generating natural language explanations.
* **Lack of Evaluation Standards:** While advancements in Large Language Models (LLMs) enable processing entire textbooks as context inputs, there is a distinct lack of a standardized, reproducible evaluation framework to automatically assess, verify, and compare LLM-generated information retrieval against original sources.

### Proposed Solution
* This thesis establishes a **reproducible, containerized, open-source application** to automatically generate, evaluate, and compare answers from selected LLMs using the textbook as a knowledge base.
* The evaluation incorporates both a standardized question-answering dataset and a real-world University Master's program exam to assess the academic and scientific viability of modern LLMs in medical informatics.

---

## 2. Key Findings by Section

### [Section 1.1: Subject Matter](Chapters/1Introduction.tex#L3)
* Accessing textbook knowledge on-demand without manual reading is a critical need.
* The **SNIK project** (Leipzig University, IMISE) is a key semantic representation of Health Information Management (HIM) knowledge, but fails at open-domain/complex conversational question-answering.
* Modern LLMs with large context windows (like GPT-4 and equivalent models) present a transformative opportunity to bridge this gap by digesting enormous academic texts inside the system's prompt context.

### [Section 1.2: Problem Statement](Chapters/1Introduction.tex#L29)
* **Underlying Problem:** Standard textbook references are fragmented. Students cannot retrieve synthesized knowledge (e.g., tracing a role's functions across the whole architecture) efficiently.
* **Retrieval Issues:** Prior natural language QA solutions suffer from factual incompleteness or a lack of alignment with original textbooks.
* **Principal Barrier:** There is currently no standardized or reproducible benchmarking framework for evaluating and comparing LLM retrieval capabilities dynamically on textbook-level resources.

### [Section 1.3: Motivation](Chapters/1Introduction.tex#L69)
* LLMs can generate complete, well-formed explanations with high factual reliability if supplied with the correct context.
* Adapting and rigorously benchmarking these systems permits finding the optimal model configuration to support students in research and preparation for academic scenarios.

### [Section 1.4: Goals (G1–G4)](Chapters/1Introduction.tex#L95)
* **Goal G1:** Create an open-source, executable application runnable with restricted resources to perform automatic, textbook-based QA and assessment.
* **Goal G2:** Automate textbook-based answering on healthcare information systems.
* **Goal G3:** Use the application to automatically solve a sample master's level exam on HIM.
* **Goal G4:** Evaluate and compare LLM answers using established methodologies and metrics.

---

## 3. Chapter 5: Practical Solution & Implementation

This section highlights the technical implementation of the comparison application, the generative pipelines, and the execution details of the scoring framework in [Chapters/5Solution.tex](Chapters/5Solution.tex).

### System Architecture & Model Selection
* **Unified Model Exploration ([5Solution.tex#L21](Chapters/5Solution.tex#L21)):** Models are integrated via open-source aggregators (**OpenRouter** and the Helmholtz AI Research Platform **Blablador**) to decouple cost, access, and runtime requirements. Included models span multiple weight sizes, including **Llama-4-Scout** ($109B$), **Gemini-2.5-Flash** ($400B$), and **GPT-4.1-Mini** ($\approx 7B$).
* **Academic/Sovereign Hosts ([5Solution.tex#L38](Chapters/5Solution.tex#L38)):** The use of sovereign/free cloud hosting for models like **Qwen3-VL-32B-Instruct** and **Ministral-8B-Instruct-2410** on *Blablador* provides secure, GDPR-compliant local execution vectors for academic institutions lacking massive computational funds.
* **Data Persistence & Bidirectional Mirroring ([5Solution.tex#L187](Chapters/5Solution.tex#L187)):** Runtime data structures and vector storage instances are isolated inside containerized Docker volumes and mirrored to local physical disk state to prevent loss across container teardown cycles.
* **Textbook Dimensions ([5Solution.tex#L201](Chapters/5Solution.tex#L201)):** The processed book context contains approximately $120,000$ tokens, representing the baseline constraint guiding retrieval thresholds.

### Pipeline Engineering (RAG vs. LCW)
* **Hybrid Retrieval Chunking is Mandatory ([5Solution.tex#L329](Chapters/5Solution.tex#L329)):** Early testing showed that pure *semantic chunking* resulted in poor similarity scores and failed vector queries. A **hybrid chunking strategy** (running semantic and fixed-size chunking side-by-side) was engineered to back up retrieved context.
* **Embedding Constraints ([5Solution.tex#L358](Chapters/5Solution.tex#L358)):** The chosen embedding model (`BAAI/bge-small-en`) filters and normalizes uppercase/lowercase acronyms (like converting the acronym `tHIS` into the literal word `this`). To bypass semantic misinterpretation, original queries containing acronyms are programmatically expanded with exact terminology before indexing.
* **Retrieval Strictness ([5Solution.tex#L393](Chapters/5Solution.tex#L393)):** The database is configured using a strict similarity minimum threshold of $0.85$ inside a containerized **Qdrant DB** instance, with a retrieve limit of the top $k=15$ chunks.
* **Token Reasoning Glitch & Prompt Modification ([5Solution.tex#L518](Chapters/5Solution.tex#L518)):** Under sovereign providers (*Blablador*), setting explicit raw API token limits (e.g. `max_token=512`) consumes token budgets during reasoning, leaving zero tokens for actual final generation. To fix this, hard constraints are omitted from API payloads and moved into written instructions inside the prompt templates.

### Automated "LLM-as-a-Judge" Evaluation Pipeline
* **LLM Judgements Orchestrated with DeepEval ([5Solution.tex#L756](Chapters/5Solution.tex#L756)):** The framework uses an asynchronous orchestrator using `Gemini_2_5_Flash` under a strict deterministic temperature ($0.0$) to score performance.
* **The Granularity Decomposition Paradigm ([5Solution.tex#L765](Chapters/5Solution.tex#L765)):** Evaluation of answers relies on **Semantic Subanswer Extraction** (Step 1). Original answers and generated answers are programmatically atomized into discrete semantic sub-units.
* **Prompt Engineering for Judges ([5Solution.tex#L827](Chapters/5Solution.tex#L827)):** Standard extraction outputs (JSON/Lists) fail due to symbol collisions and serialization failures. The solution uses custom structured tag delimiters (`<sub_answer>...</sub_answer>`) paired with strict regular expressions to parse results cleanly.
* **Multi-Dimensional Metrics ([5Solution.tex#L906](Chapters/5Solution.tex#L906)):**
  1. **Question Understanding (Step 2 - [5Solution.tex#L906](Chapters/5Solution.tex#L906)):** Binary score (with structured reason) evaluating whether the model correctly interpreted questions regardless of answer accuracy.
  2. **Explainability (Step 3 - [5Solution.tex#L1001](Chapters/5Solution.tex#L1001)):** Scores whether descriptions justify statements of fact rather than merely spelling out naked facts.
  3. **Correctness (Step 4 - [5Solution.tex#L1040](Chapters/5Solution.tex#L1040)):** Subanswers are classified as *Correct*, *Helpful*, or *Not Helpful*.
  4. **Robustness (Step 5 - [5Solution.tex#L1250](Chapters/5Solution.tex#L1250)):** Measures accuracy retention when questions are deliberately degraded with spelling/grammatical errors.

### Strategic Policy Modes
* **Strict Truth vs. Exam (Evidence-Sensitive) Policy ([5Solution.tex#L1202](Chapters/5Solution.tex#L1202)):**
  * **Strict Truth Mode:** Classifies any unverifiable answer or non-exact index match as incorrect.
  * **Exam Mode (Evidence-Sensitive):** Tolerates conceptually similar or auxiliary factual statements that are text-supported, permitting a realistic credit-based scoring akin to real-world exam criteria.

---

## 4. Chapter 6: Evaluation Results & Benchmarks

This section details the comparative findings, quantitative results, and metric evaluations of the system architectures presented in [Chapters/6Results.tex](Chapters/6Results.tex).

### Best-Performing Method & Model Summaries
* **Core Comparison ([6Results.tex#L11](Chapters/6Results.tex#L11)):** **RAG** proved to be the slightly superior and more stable method. It establishes critical factual boundaries, maintains high precision on corrupted inputs, and runs at a fraction of the cost of the LCW method.
* **Top Proprietary Model ([6Results.tex#L21](Chapters/6Results.tex#L21)):** Across all modes, **GPT-4.1-Mini** is the most balanced, accurate, and cost-effective model under RAG (yielding a RAG F1 of $0.518$ on textbook questions).
* **Top Open-Source Model ([6Results.tex#L23](Chapters/6Results.tex#L23)):** **Qwen3-VL-32B** (Blablador) proves outstanding, especially inside the LCW pipeline (yielding a strict F1 of $0.511$ without token limitations).

### Correctness Analysis
* **Consistent LCW Partial Truths ([6Results.tex#L281](Chapters/6Results.tex#L281)):** Answering questions under LCW yields high numbers of *partially correct* outputs ($82$ to $107$ out of $189$ total answers). Expanding the context to the entire $120,000$-token textbook successfully exposes models to key facts, but models struggle to gather *all* necessary subanswers.
* **RAG Precision Advantages ([6Results.tex#L299](Chapters/6Results.tex#L299)):** RAG significantly improves strict fact-matching. Under Strict Mode, DeepSeek-V3 jumps from $48$ to $56$ exact matches, and Gemini-2.5-Flash from $37$ to $51$ matches.
* **Failsafe Under RAG ([6Results.tex#L309](Chapters/6Results.tex#L309)):** Conversely, RAG prompts more *unanswered* results ($17$ for Gemini, $21$ for Llama). Since database queries enforce a minimum threshold of $0.85$, models choose to withhold answers rather than hallucinate fictional solutions when highly relevant vector keys are absent.
* **The "Exam Mode" Advantage ([6Results.tex#L510](Chapters/6Results.tex#L510)):** When evaluating under Exam Mode (which recognizes contextually "helpful" answers), correctness stats improve noticeably. Under RAG, **GPT-4.1-Mini** achieves $61$ exact matches ($32.2\%$) and **DeepSeek-V3** achieves $63$ matches ($33\%$).
* **Combined Performance ([6Results.tex#L540](Chapters/6Results.tex#L540)):** Combining *correct* and *partially correct* answers yields high scores. RAG-boosted **GPT-4.1-Mini** answered $89\%$ of exam questions correctly/partially correctly (under Exam Mode), followed by **DeepSeek-V3** at $84\%$.

### Macro F1 Metric Observations
* **Strict Macro F1 Clustered ([6Results.tex#L714](Chapters/6Results.tex#L714)):** Inside the LCW pipeline, all OpenRouter models align closely ($0.37$ to $0.41$).
* **The "Book vs. Exam" Split ([6Results.tex#L746](Chapters/6Results.tex#L746)):**
  * **Textbook-Specific Queries:** Low LCW baseline performance ($F1 \leq 0.15$). Switching to RAG yields massive performance improvements (e.g. GPT-4.1-Mini rising to a peak F1 of $0.518$).
  * **Transfer/Exam Queries:** Both pipelines achieved comparative performance ($\approx 0.35$ to $0.50$ F1), with no single superior technique.

### Quality, Intent, and Robustness Assessments
* **Exceptional Explainability ([6Results.tex#L758](Chapters/6Results.tex#L758)):** Models demonstrated high proficiency in providing explanatory reasoning alongside factual assertions, despite no explicit explanation requirements inside prompt headers.
* **Flawless Question Understanding ([6Results.tex#L775](Chapters/6Results.tex#L775)):** Evaluator checks showed high "understood" counts, with minor failures confined mostly to transfer-style questions.
* **Robustness & Degradation ([6Results.tex#L800](Chapters/6Results.tex#L800)):**
  * **LCW Fragility:** Spelling errors heavily degrade LCW performance. Correct matches on OpenRouter models decrease significantly (with *Llama* plunging by $21$ percentage points). Prompting over massive, un-indexed context blocks makes attention chains vulnerable to noisy input.
  * **RAG Stability:** Semantic query isolation and acronym parsing under RAG show remarkable stability against dirty inputs, with scores decreasing by merely $2\%$ to $7\%$ across most models.

-------------------


