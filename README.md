# NLP Assignment - HW2  
**Name:** Tejaswini Kolluru  
**ID:** 700773943  

---

## Q1. Bayes Rule Applied to Text  
**Tasks:**  
1. Explain in your own words what each term means: P(c), P(d∣c), and P(c∣d).  
2. Why can the denominator P(d) be ignored when comparing classes?  

### Answer 1  
#### 1. Explanation of Terms  
- **P(c)**: Prior probability of class `c`. It reflects how likely a class is before seeing the document.  
  *Example*: If 70% of all emails are not spam, then P(Not Spam) = 0.7.  

- **P(d | c)**: Likelihood, i.e., the probability of observing document `d` given class `c`.  
  *Example*: Probability of seeing words like “discount” and “buy” in spam emails.  

- **P(c | d)**: Posterior probability of class `c` given document `d`. This is what we ultimately want to compute in classification.  

#### 2. Why P(d) Can Be Ignored  
Bayes’ Rule:  

\[
P(c|d) = \frac{P(c) \cdot P(d|c)}{P(d)}
\]  

- **P(d)** is the probability of the document itself, independent of class.  
- Since P(d) is constant across all classes, it does not affect which class has the maximum probability.  
- Therefore, classification is done by maximizing only:  

\[
P(c) \cdot P(d|c)
\]  

**Example (email classification with word “discount”):**  
- P(Spam) = 0.4  
- P(Not Spam) = 0.6  
- P(“discount” | Spam) = 0.3  
- P(“discount” | Not Spam) = 0.05  

Scores (ignoring P(d)):  
- Spam: 0.4 × 0.3 = **0.12**  
- Not Spam: 0.6 × 0.05 = **0.03**  

Since 0.12 > 0.03 → The email is classified as **Spam**.  

---

## Q2. Add-1 Smoothing  
**Given:** Priors P(-)=3/5, P(+)=2/5, Vocabulary size = 20.  
Negative class total tokens = 14.  

### Answer  
1. **Denominator for likelihood estimation:**  
\[
14 + 20 = 34
\]  

2. **P(predictable | -):**  
\[
(2+1)/34 = 3/34 \approx 0.0882
\]  

3. **P(fun | -):**  
\[
(0+1)/34 = 1/34 \approx 0.0294
\]  

---

## Q3. Worked Example Document Classification  
**Test document:** *“predictable no fun”*  

### Given Information  
- Priors: P(-) = 3/5, P(+) = 2/5  
- Vocabulary size = 20  
- Negative class: total tokens = 14 → denominator = 34  
  - P(predictable | -) = 3/34  
  - P(no | -) = 2/34 (assumed)  
  - P(fun | -) = 1/34  
- Positive class: total tokens = 9 → denominator = 29  
  - P(predictable | +) = 1/29  
  - P(no | +) = 2/29  
  - P(fun | +) = 3/29  

### Steps  
**Negative Score:**  
\[
(3/5) \cdot (3/34) \cdot (2/34) \cdot (1/34) \approx 9.16 \times 10^{-5}
\]  

**Positive Score:**  
\[
(2/5) \cdot (1/29) \cdot (2/29) \cdot (3/29) \approx 9.85 \times 10^{-5}
\]  

### Conclusion  
- Negative ≈ 9.16 × 10⁻⁵  
- Positive ≈ 9.85 × 10⁻⁵  
→ Document is classified as **Positive**.  

---

## Q4. Harms of Classification  

### 1. Representational Harm  
- **Definition**: Occurs when systems reinforce stereotypes or unfair associations about groups.  
- **Example**: Kiritchenko & Mohammad (2018) showed sentiment systems rated phrases about some groups more negatively, demonstrating bias in representation.  

### 2. Risk of Censorship  
- Toxicity classifiers may wrongly flag neutral or supportive speech as toxic.  
- **Example**: Dixon (2018), Oliva (2021) – terms like “gay” or “Black” often flagged as toxic, silencing marginalized voices.  

### 3. Classifier Performance on AAE / Indian English  
- These varieties differ in grammar, spelling, and vocabulary.  
- Lack of training data representation → classifiers misinterpret them → **lower accuracy**.  

---

## Q5. Evaluation Metrics from Confusion Matrix  

### Confusion Matrix  
| System \ Gold | Cat | Dog | Rabbit |  
|---------------|-----|-----|--------|  
| **Cat**       | 5   | 10  | 5      |  
| **Dog**       | 15  | 20  | 10     |  
| **Rabbit**    | 0   | 15  | 10     |  

### 1. Per-Class Metrics  
- **Cat**: Precision = 0.25, Recall = 0.25  
- **Dog**: Precision = 0.40, Recall ≈ 0.444  
- **Rabbit**: Precision = 0.40, Recall = 0.40  

### 2. Macro vs Micro Averaging  
- Macro Precision ≈ 0.35  
- Macro Recall ≈ 0.365  
- Micro Precision ≈ 0.368  
- Micro Recall ≈ 0.389  

**Interpretation**:  
- **Macro**: Equal weight to all classes.  
- **Micro**: Weighs larger classes more.  

### 3. Python Implementation  
---

## Q6. Bigram Probabilities and Zero-Probability Problem  

### 1. Sentence Probabilities (MLE)  
- **S1: `<s> I love NLP </s>`**  
\[
P(S1) = \frac{2}{3} \times 1 \times \frac{1}{2} \times 1 = \frac{1}{3} \approx 0.333
\]  

- **S2: `<s> I love deep learning </s>`**  
\[
P(S2) = \frac{2}{3} \times 1 \times \frac{1}{2} \times 1 \times \frac{1}{2} = \frac{1}{6} \approx 0.167
\]  

✅ **Conclusion:** S1 is more probable (0.333 > 0.167).  

---

### 2. Zero-Probability Problem  

- **MLE Estimate:**  
\[
P(\text{noodle} | \text{ate}) = \frac{0}{12} = 0
\]  

- **Problem:** Any unseen bigram → probability = 0 → entire sentence probability = 0 → breaks sentence probability and perplexity evaluation.  

- **Laplace (Add-1) Smoothing:**  
\[
P(\text{noodle} | \text{ate}) = \frac{0+1}{12+10} = \frac{1}{22} \approx 0.0455
\]  

---

## Q7. Backoff Model  

### 1. P(cats | I, like)  
\[
P(\text{cats} | I, \text{like}) = \frac{C(I, \text{like}, \text{cats})}{C(I, \text{like})} = \frac{1}{2} = 0.5
\]  

### 2. P(dogs | You, like) using Backoff  
- **Trigram:**  
\[
P(\text{dogs} | You, \text{like}) = \frac{0}{1} = 0
\]  

- **Backoff to Bigram:**  
\[
P(\text{dogs} | \text{like}) = \frac{1}{2+1} = \frac{1}{3} \approx 0.333
\]  

### 3. Why Backoff is Necessary  
- Without backoff, unseen trigrams → probability 0.  
- Backoff allows generalization by using bigram or unigram counts, ensuring **non-zero probabilities**.  

---

## Q8. Programming: Bigram Language Model Implementation  

### Training Corpus  
