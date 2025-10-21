# Vector Similarity and Information Retrieval
#cs #linguistics 
## Recurring Themes
- Documents represented as vectors
- Parts of documents: words, n-grams, chunks, abstract elements
- Measuring importance of each part
- Vector similarity applications:
  - Document-to-document similarity
  - Question-document matching  
  - Genre classification
  - Sentiment analysis

## Ad Hoc Information Retrieval (IR)
- **Given**: Collection of documents + query
- **Goal**: Find documents that best match the query
- **Assumptions**:
  - Documents = bag of terms
  - Queries = bag of terms
- **Terms**: words, bigrams, trigrams, noun groups

## Vector Representations
- Documents and queries represented as vectors
- Each dimension = a term (word)
- Each value = weight/score for that term

## TF-IDF: Common Weighting Scheme
- **TF (Term Frequency)**: Frequency of term in document
- **IDF (Inverse Document Frequency)**:
$$
\text{IDF}(t) = \log\left(\frac{\text{Number of Documents}}{\text{Number of Documents Containing } t}\right)
$$
- **TFIDF Formula**:
$$
\text{TFIDF}(t) = \text{TF}(t) \times \text{IDF}(t)
$$
- High TFIDF = term is frequent in document but rare in collection

## TF-IDF Normalization
- IDF normalized using log function
- TF normalization options:
  - Divide by document length: $\frac{\text{TF}}{\text{document length}}$
  - Apply log function: $\log(1 + \text{TF})$
- Puts long and short documents on equal footing

## TF-IDF Example: noodle vs. tablespoon
- **noodle**: 
  - TF = 3
  - IDF = $\log(10000/4) = \log(2500) = 7.82$
  - TFIDF = $3 \times 7.82 = 23.46$

- **tablespoon**:
  - TF = 4  
  - IDF = $\log(10000/1200) = \log(8.33) = 2.12$
  - TFIDF = $4 \times 2.12 = 8.48$

- **noodle** more characteristic of recipes than **tablespoon**

## Cosine Similarity
- Measures angle between vectors in high-dimensional space
- **Formula**:
$$
\text{Similarity}(A,B) = \frac{\sum_{i=1}^n a_i b_i}{\sqrt{\sum_{i=1}^n a_i^2} \cdot \sqrt{\sum_{i=1}^n b_i^2}}
$$
- Range: 0 to 1 (1 = identical vectors)
- High cosine = smaller angle = more similar

## Cosine Similarity Example
- Terms: potato chip, chicken, sesame seed, coconut milk, ground beef
- Q1: (0, 5, 0, 5, 0) - "chicken, coconut milk"
- D1: (0, 7, 0, 9, 0) - "Chicken and Coconut Soup Recipe"
- **Calculation**:
$$
\text{Similarity}(Q1,D1) = \frac{(0\times0) + (5\times7) + (0\times0) + (5\times9) + (0\times0)}{\sqrt{0^2 + 5^2 + 0^2 + 5^2 + 0^2} \cdot \sqrt{0^2 + 7^2 + 0^2 + 9^2 + 0^2}} = \frac{80}{\sqrt{50} \cdot \sqrt{130}} \approx 0.992
$$

## Alternative Similarity Measures
- **Jaccard Similarity**:
$$
J(A,B) = \frac{|A \cap B|}{|A \cup B|}
$$
- Example: "green eggs and ham" vs "blue eggs and chocolate"
  - Intersection = 2 words, Union = 8 words
  - $J = 2/8 = 0.25$

- **Euclidean Distance**:
$$
d(A,B) = \sqrt{\sum_{i=1}^n (a_i - b_i)^2}
$$
- Lower distance = more similar

- **Dot Product**:
$$
A \cdot B = \sum_{i=1}^n a_i b_i
$$

## Evaluation Metrics
- **Precision**:
$$
\text{Precision} = \frac{|\text{System Output} \cap \text{Answer Key}|}{|\text{System Output}|}
$$

- **Recall**:
$$
\text{Recall} = \frac{|\text{System Output} \cap \text{Answer Key}|}{|\text{Answer Key}|}
$$

- **F-measure**:
$$
F = \frac{2 \cdot \text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = \frac{2}{\frac{1}{\text{precision}} + \frac{1}{\text{recall}}}
$$

## Evaluation Example
- System Output: {1, 2, 4, 5, 7}
- Answer Key: {1, 2, 3, 4}
- Correct = {1, 2, 4}
- Precision = $3/5 = 0.6$
- Recall = $3/4 = 0.75$
- F-measure = $\frac{2 \cdot 0.6 \cdot 0.75}{0.6 + 0.75} = 0.67$

## Interannotator Agreement
- **Kappa Statistic**:
$$
\kappa = \frac{\text{Observed Agreement} - \text{Chance Agreement}}{1 - \text{Chance Agreement}}
$$

- **Example**:
  - Annotator 1: 70 prepositions, 30 auxiliaries
  - Annotator 2: 60 prepositions, 40 auxiliaries
  - Observed Agreement = $50/100 = 0.5$
  - Chance Agreement = $(0.7 \times 0.6) + (0.3 \times 0.4) = 0.54$
  - $\kappa = \frac{0.5 - 0.54}{1 - 0.54} = -0.09$

## Mean Average Precision (MAP)
- Used to evaluate ranked IR results
- Compute precision at multiple recall levels
- Average the precision scores
- **Example**:
  - Precision at 10% recall: $2/5 = 0.4$
  - Precision at 20% recall: $4/15 \approx 0.267$
  - Precision at 30% recall: $6/22 \approx 0.273$
  - MAP = average of all precision values

## Additional IR Factors
- Stop word removal (the, a, in, to, ...)
- Stemming (cats → cat, analyzed → analyz)
- Synonym identification and query expansion
- N-grams and chunking for term identification
- Dimensionality reduction (LDA)
- Deep learning for vector derivation
