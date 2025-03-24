# Topic Modeling Approach for `future_videos.pdf`

This project analyzes `future_videos.pdf` to identify themes for improving business data analytics videos using topic modeling. Two methods were employed: **Latent Dirichlet Allocation (LDA)** and **Correlation Explanation (CorEx)**, applied to 76 sections of the document.

## Approach Overview
The analysis followed these steps:
1. **Preprocessing**
   - Extracted text using `pdfplumber`.
   - Split into 76 sections based on numbered items and headers (e.g., "Improvements").
   - Tokenized, lowercased, removed punctuation/numbers, and lemmatized using `spacy`.
   - Applied stopwords (`nltk` + custom: "business," "data," "video," etc.) and filtered tokens (> 3 characters).
   - Result: 432 unique terms in dictionary, 76-section corpus.

2. **LDA Modeling**
   - **Method**: Unsupervised probabilistic model via `gensim`.
   - **Vectorization**: Created a bag-of-words corpus with `Dictionary` (no_below=2, no_above=0.5).
   - **Tuning**: Tested 2-15 topics, optimized for `c_v` coherence with `passes=30`, `alpha/eta=auto`.
   - **Result**: 3 topics, coherence = 0.4323.

3. **CorEx Modeling**
   - **Method**: Semi-supervised, information-theoretic model via `corextopic`.
   - **Vectorization**: Built a sparse document-word matrix (`scipy.sparse.csr_matrix`).
   - **Anchoring**: Used LDA-derived and document-specific anchors (e.g., "study," "predictive," "ethical").
   - **Tuning**: Adjusted anchor strength (2 → 5), topics (3 → 4 → 3), `max_iter=200`.
   - **Result**: 3 topics, TC = 40.0580.

## Key Parameters
- **LDA**: `random_state=42`, `passes=30`, coherence optimized to 0.4323.
- **CorEx**: `seed=42`, `anchor_strength=5`, TC = 40.0580 with 3 topics.

## Themes Identified
Both models converged on three themes:
1. **Practical Applications & Tools**: Hands-on, real-world focus (e.g., tutorials, case studies).
2. **Emerging Technologies & Trends**: Predictive analytics, machine learning, cloud.
3. **Ethics & Analytical Skills**: Ethics, privacy, visualization, soft skills.

## Why This Approach?
- **LDA**: Provided a baseline with high coherence, capturing natural patterns.
- **CorEx**: Refined themes with anchors, ensuring alignment with document goals (e.g., "Practical Demonstrations").
- **Combined**: Balanced unsupervised discovery (LDA) with guided relevance (CorEx) for actionable insights.

## Limitations
- Small corpus (76 sections) limited TC ceiling (~40).
- Preprocessing filtered niche terms (e.g., "quantum," "bias")—future runs could relax stopwords.

## Usage
Results inform video content in `findings.md`, offering clear, data-driven themes for development.