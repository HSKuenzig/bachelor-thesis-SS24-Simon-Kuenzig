### Detailed Step-by-Step Strict Mode Performance Analysis (\cref{tab:total_strict_scores_answered})

An evaluation of the absolute performance scores in *Strict* mode reveals several critical insights regarding method architecture, parameter scaling, and local prompt configurations. 

#### 1. General Observations of the LCW Method
The Large Context Window (LCW) method exhibits highly consistent structural properties across all models:
* **High Semantic Coverage with High Partiality**: Across the board, LCW generates a substantial volume of *Partially Correct* (score 3) answers (ranging from $82$ to $99$ out of $189$). This demonstrates that injecting the full $120,000$-token textbook successfully exposes the models to the relevant facts, though they struggle to capture *every* required golden subanswer.
* **Elevated False Positives**: *Incorrect* counts (score 2) remain distinctively high in LCW (hovering between $44$ and $53$). Context leakage and the "needle-in-a-haystack" retrieval burden over the massive text block frequently lead to minor over-elaboration or contextual blurring that gets flagged as strict mismatches.
* **Extremely Low Non-Response Rate**: The *Not answered* (score 0) rate is remarkably low ($8$ to $10$ for OpenRouter, and a perfect $0$ for Qwen3-VL-32B with limits). Because the entire knowledge base is directly present in the input buffer, models almost never decline to answer due to "insufficient information."

#### 2. Structural Differences: LCW vs. RAG
The most significant trend between the two pipelines is RAG's ability to boost high-precision, localized factual recovery:
* **Precision Uplift**: For almost all models, switching from LCW to RAG yields a clear increase in absolute *Correct* (score 1) classifications. For example, *DeepSeek-V3* improves from $48$ to $56$, *GPT-4.1-Mini* climbs from $47$ to $58$, and *Gemini-2.5-Flash* advances from $37$ to $51$. Localizing the query to granular, relevant chunks prevents models from getting distracted by extraneous text.
* **The Non-Response Trade-off**: However, RAG forces a significant increase in *Not answered* (score 0) decisions. *Gemini-2.5-Flash* non-responses more than double (from $8$ in LCW to $17$ in RAG), and *Llama-4-Scout* jumps nearly threefold (from $8$ to $21$). This reflects the direct correlation between database retrieval thresholds ($0.85$ similarity cutoff) and halting: if the vector database fails to retrieve highly relevant points, models choose to withhold answers rather than generate hallucinated reasoning.

#### 3. Operational Impact of Limit Settings (Qwen Cohort)
Analyzing the *Qwen3-VL-32B* model across the different limit configurations reveals how prompt-level truncation constraints dictate model outputs:
* **LCW Limit vs. No Limit**: Enforcing token limits within the prompt (*yes*) results in slightly fewer absolute *Correct* answers ($45$ vs. $50$) but manages to eliminate unanswered questions entirely ($0$ vs. $9$) and elevates *Partially Correct* replies to $92$ (compared to $82$). This suggests token-restricted prompt instructions force the model to provide dense, abbreviated summaries which are highly contextually useful but may skip key subanswers.
* **RAG Limit vs. No Limit**: The difference is even more pronounced under RAG. Restricting output tokens (*yes*) yields only $36$ correct answers. Removing the limit (*no*) allows the model to output comprehensive detail, scoring $59$ *Correct* answers (the highest in the table). However, this lack of constraint simultaneously causes *Incorrect* answers to skyrocket to $61$, showing that long unrestricted outputs are highly likely to contain both verified textbook details and incorrect elaborations.

#### 4. Parameter Scale Comparison: OpenRouter vs. Blablador
Comparing the premium, highly parameterized OpenRouter models (*DeepSeek-V3* with 37B active parameters, *Llama-4-Scout* with 109B) to the localized, free, smaller *Qwen3-VL-32B* (32.8B dense) reveals a highly surprising finding:
* **No Definite Scale Advantage**: The larger commercial models do not exhibit a definitive superiority over the smaller academic model. 
* Under LCW (with limits), *Qwen3-VL-32B* scores $45$ *Correct* and $92$ *Partially Correct* answers, performing on par with or better than *Llama-4-Scout* ($43$ / $85$) and *Gemini-2.5-Flash* ($37$ / $99$). 
* Under RAG (unlimited context), *Qwen3-VL-32B* holds the highest *Correct* count in the entire strict evaluation ($59$), narrowly beating *GPT-4.1-Mini* ($58$). This implies that robust specialized instruction tuning of open-weights models like Qwen is highly competitive with pure parameter scale for open-book domain-expert QA tasks.
