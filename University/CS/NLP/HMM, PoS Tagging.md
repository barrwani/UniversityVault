# HMM and Part of Speech Tagging

## Outline
- Parts of Speech Tagsets
- Rule-based POS Tagging
- HMM POS Tagging
- Homework Assignment

## Part of Speech Tags Standards
There is no standard set of parts of speech that is used by all researchers for all languages. The most commonly used English tagset is that of the Penn Treebank at the University of Pennsylvania.

POS tagsets assume particular tokenizations, e.g., Mary's → Mary + 's. They distinguish inflections: e.g., eat/VB, eat/VBP, eats/VBZ, ate/VBD. Different instances of the same string can have different tags: She wants to eat/VB; They eat/VBP. He eats/VBZ, Those are good eats/NNS.

Annotators and POS taggers assign tags to each token in a sentence, no exceptions.

## The Penn Treebank II POS tagset

Verbs: VB, VBP, VBZ, VBD, VBG, VBN (base, present-non-3rd, present-3rd, past, -ing, -en)

Nouns: NNP, NNPS, NN, NNS (proper/common, singular/plural; singular includes mass + generic)

Adjectives: JJ, JJR, JJS (base, comparative, superlative)

Adverbs: RB, RBR, RBS, RP (base, comparative, superlative, particle)

Pronouns: PRP, PRP$ (personal, possessive)

Interrogatives: WP, WP, WDT, WRB (compare to: PRP, PRP, DT, RB)

Other Closed Class: CC, CD, DT, PDT, IN, MD

Punctuation: # $ . , : ( ) " " " !"

Weird Cases: FW (deja vu), SYM (@), LS (1, 2, a, b), TO (to), POS (s, '), UH (no, OK, well), EX (it/there)

Newer tags: HYPH, PU

## What do POS tags represent?
A dictionary of terminal symbols, assuming a context free phrase structure grammar (non-terminals represent phrases). There is variation regarding how specific the labels should be, e.g., should they include inflection? Should all verbs have one POS – V, or should we adopt PTB approach with VB, VBP, VBZ, VBD, VBG, VBN, or some other approach?

## Part of Speech Tagging
POS taggers assign 1 POS tag to each input token, e.g., The/DT silly/JJ man/NN is/VBZ a/DT professor/NN ./PU.

Different ways of breaking down POS tagging: use separate "tokenizer" that divides string into list of tokens (POS tagger processes output) or incorporate tokenizer into POS tagger.

Different ways of breaking down parsing: use separate POS tagger (output of tagger is input to parser) or assign POS tags as part of parsing.

Accurate POS tagging is "easier" than accurate parsing. POS tags may be sufficient information for some tasks.

## Some Tokenization Rules for English
1. Divide at spaces and hyphens.
2. Divide before punctuation that is followed by: a space or the end of the line. Define punctuation as any non-letter/non-number: !@#$%^&*()\-+={}[]|;:'"`~<,>.?/. Punctuation followed by a space, other punctuation, or at the end of line should be separated from words: ...and he left.") → and he left . "
3. Break off the following as separate tokens when followed by a space or end of line: 's, n't, 'd, 've, 'm, 'll, 're, ... (a short list)
4. Abbreviations are exceptions to rule 2: Period after abbreviations should not be separate from words. Most cases covered by list of 100 items (or if sentence end is known). Final periods are not duplicated after abbreviations (consistency issues). These periods serve 2 functions simultaneously (argument for duplication). These periods occupy a single character position (argument against duplication – difficulty with calculating character offsets).

## Sentence Boundaries
Most POS taggers assume sentence divisions.

Sample sentence splitting rules: End sentence after . ? !, possibly others (; ; ...). Begin quotes are part of next sentence. End quotes are part of previous sentence. But post-abbreviation (inc, co, ...) periods are ambiguous: next character is lowercase – not sentence end; next character is uppercase or number – possible sentence end.

Most POS taggers assume sentence boundaries are given. Multiple sentences within quotes are assumed separate: <S> She said, "This is the way things are. </S> <S> This is this.</S> <S> That is that."</S>

Sentence Splitting is a potentially good Final Project Topic.

## Rule-based POS Tagger

Method: Assign lists of potential POS tags to each word based on dictionary. Manual rules for Out of Vocabulary (OOV) words: Ex: Non-initial capital → NNP, ends in S → VBZ|NNS, default → NN|JJ; etc. Apply hand-written constraints until each word has only one possible POS.

Sample Constraints:
1. DT cannot immediately precede a verb
2. No verb can immediately precede a tensed verb: VBZ, VBP, VBD. Example: Untensed: VB (base form), VBN & VBG (past & present participles)

Example: The/DT book{NN|VB|VBP} is/VBZ on/IN the/DT table{NN|VB|VBP} becomes The/DT book/NN is/VBZ on/IN the/DT table/NN because DT cannot precede VB or VBP, and VBZ cannot be preceded by VB or VBP.

## Probability
Estimate of probability of future event based on past observations: P(event) = num of events / num of trials.

Conditional Probability: probability of X given Y: P(X|Y) = P(X, Y) / P(Y).

Examples relating to POS tags:
- Out of 200 DT tags, 150 of them are tagging the word the. If a word is tagged DT, there is a 75% chance that word is the. Example of likelihood probability.
- The POS after a DT is NN 120 times and JJ 60 times: A word following DT is 120/200 = 60% likely to be a singular noun (NN) and 60/200 = 30% likely to be a base adjective (JJ). Examples of transition probability (probability of tag NN or JJ, given previous tag DT).

## More Math Terminology
N instances of a variable looked at individually: Xn is the same as X1, X2, X3, ..., xn in sequence.

The product of instances of X from 1 to n: ∏ from i=1 to n of Xi.

Max = the maximum number in a set.

Argmax = the choice of variable values that maximizes a formula.

## Probabilistic Models of POS tagging
For tokens w1, ..., wn, find the most probable corresponding sequence of possible tags t1, ..., tn. We assume that "probable" means something like "most frequently observed in some manually tagged corpus of words".

Penn Treebank II (a common training corpus) has 1 million words from the Wall Street Journal, tagged for POS (and other attributes).

The specific sequence (sentence) is not in the training corpus, so the actual "probability" is 0. Common practice: estimate probability given assumptions, e.g., assume that we can estimate probability of whole tag sequence by multiplying simpler probabilities, e.g., sequences of 2 consecutive tags.

## Probabilistic Assumptions of HMM Tagging
Goal: t̂ = argmax_{t1^n} P(t1^n|w1^n) - choose the tag sequence of length n that is most probable given the input token sequence.

Bayes Rule: P(x|y) = P(y|x)P(x)/P(y) - way to derive the probability of x given y when you know the probability of y given x, the probability of x and the probability of y.

Applying Bayes Rule to Tag Probability: t̂ = argmax_{t1^n} P(w1^n|t1^n)P(t1^n) / P(w1^n)

## Simplifying Assumptions for HMMs
Simplification: Drop the denominator since it is same for all the tag sequences (the word sequence is given): t̂ = argmax_{t1^n} P(w1^n|t1^n)P(t1^n)

For each tag sequence calculate the product of the probability of the word sequence given the tag sequence (likelihood) and the probability of the tag sequence (prior probability). Still too hard.

Two simplifying assumptions make it possible to estimate the probability of tag sequences given word sequences:
1. If the probability of a word is only dependent on its own POS tag: P(w1^n|t1^n) ≈ ∏_{i=1}^n P(wi|ti)
2. If the probability of a POS tag is only dependent on the previous POS tag: P(t^n) ≈ ∏_{i=1}^n P(ti|t_{i-1})

The result of these assumptions: t̂ ≈ argmax_{t1^n} ∏_{i=1}^n P(wi|ti)P(ti|t_{i-1})

Note: B & E represent tag before/after sentence (Begin & End). HMM taggers are fast and achieve accuracy scores of about 93-95%.

## Estimating Probability of Tag Sequence
We assume that: t̂ = argmax_{t1^n} ∏_{i=1}^n P(wi|ti)P(ti|t_{i-1})

Acquire frequencies from a training corpus:
- Word Frequency with given POS: suppose "book" occurs 14 times in a corpus: 10 times (.001) as NN (there are 10000 instances of NN in the corpus); 3 times (.003) as VBP (the corpus has 1000 VBPs); and 1 instance of book (.002) as VB (the corpus has 500 VBs).
- Given the previous tag, how often does each tag occur: suppose DT is followed by NN 80,000 times (.53), JJ 30,000 times (.2), NNS 20,000 times (.13), VBN 3,000 (.02) times, ... out of a total of 150,000 occurrences of DT.

All possible tags for sequence: The/DT book/(NN|VB|VBP) is/VBZ on/IN the/DT table/(NN|VB|VBP)

Hypothetical probabilities for highest scoring tag sequence: The/DT book/NN is/VBZ on/IN the/DT table/NN with probabilities: The/DT=.4, book/NN=.001, is/VBZ=.02, on/IN=.1, the/DT=.4, table/NN=.0005, and transitions: B DT = .61, DT NN = .53, NN VBZ =.44, VBZ IN = .12, IN DT = .05, DT NN = .53, NN E .31.

Product = (.4 × .61)(.001 × .53)(.02 × .44)(.1 × .12)(.4 × .05)(.0005 × .53)(.1 × .31) ≈ 2.4 × 10^{-13}

## Defining an HMM
A Weighted Finite-state Automation (WFSA) where each transition arc is associated with a probability. The sum of all arcs outgoing from a single node is 1.

Markov chain is a WFSA in which an input string uniquely determines path through the Automation.

Hidden Markov Model (HMM) is a slightly different case because some information (previous POS tags) is unknown (or hidden).

HMM consists of:
- Q = set of states: q0 (start state), ..., qF (final state)
- A = transition probability matrix of n × n probabilities of transitioning between any pair of n states (n = F+1). Called prior probability or transition probability of a tag sequence.
- O = sequence of T observations (words) from a vocabulary V
- B = sequence of observation likelihoods (probability of observation generated at state) - Called likelihood (of word sequence given tag sequence), aka emission probability.

## Viterbi Algorithm for HMM
Observed_Words = w1 ... wT
States = q0, q1 ... qN, qF

A = N × N matrix such that aij is the probability of the transition from qi to qj
B = likelihood table such that bi(wt) is the probability that POS i is realized as word t

viterbi = (N+2) × T matrix  # columns are states, rows are words
backpointer = (N+2) × T matrix  # highest scoring previous cells for viterbi

for states q from 1 to N:  ## BEGINNING
    initialize viterbi[q,1] to a0,q * bq(w1) # score transition 0 → q given w1
    initialize backpointer[q,1] to 0 (start state)

for word w from 2 to T:  ## Middle
    for state q from 1 to N:    # for T-1 × N (w,q) pairs
        viterbi[q,w] ← max over q' of viterbi[q',w-1] * a_{q',q} * bq(ww)  # score = maximum previous * prior * likelihood
        backpointer[q,w] ← argmax over q' of viterbi[q',w-1] * a_{q',q}  # backpointer = maximum previous

viterbi[qF,T] ← max over q of viterbi[q,T] * a_{q,qF}  # END score = maximum previous * prior * likelihood
backpointer[qF,T] ← argmax over q of viterbi[q,T] * a_{q,qF}  # backpointer = maximum previous

return(best_path)  # derive by following backpointers from (qF,T) to q0

## Unknown (OOV) Words
Possibility 1: Assume all POS tags have the same probability (e.g., 1/1000). In effect, only use transitions to predict the correct tag.

Possibility 2: Use morphology (prefixes, suffixes), orthography (uppercase/lowercase), hyphenation.

Possibility 3: Words occurring once in corpus = instances of UNKNOWN_WORD. Distribution of UNKNOWN_WORD used for OOV words.

Possibility 4: Modify Likelihood Score using Smoothing. Unsmoothed Likelihood of "the" as a determiner: Instances of POS, Word = 200 instances of DT, the / 1200 instances of DT = 1/6. Add One (aka La Place) Smoothing for "the" as a determiner: (200 + 1) / (1200 + 1) = 201/1201. Example for OOV (DT = "enough") with Add One Smoothing: (0 + 1) / (1200 + 1) = 1/1201. Other Smoothing methods: Add K Smoothing (add some constant K), estimates for rare occurrences not found in the data.

Possibility 5: Some combination, e.g., divide UNKNOWN_WORD into morphological classes like UNKNOWN_WORD_ENDING_IN_S.

## Homework


Guidance on Program:

Implement Simple version of training stage first. Data has 2 fields (separated by tab): word and POS. Start of file = begin of sentence. Blank line = begin and end of sentence. End of file = end of sentence.

Make 2 hash tables of hash tables (e.g., Python dictionaries of dictionaries):
1. POS → table of frequencies of words that occur with that POS. Example: likelihood[DT] → {'the':1500, 'a':200, 'an':100, ...}. Hash table of POSs with each value a hash table from words to frequencies.
2. STATE → table of frequencies of following states. Example: Transition['Begin_Sent'] → {'DT':1000, 'NNP':500, 'VB':200, ...}. Example: Transition['DT'] → {'NN':500, 'NNP':200, 'VB':30, ...}. Hash table of states with each value a hash table from states to frequencies. States = Begin_Sent, End_Sent and all POSs. Alternative to dictionary of dictionaries: 2 dimensional array: each dimension should be 2 + number of different POS tags.

Record the set of words that occur in the corpus (1 or more times). Use this to identify OOV.

Go through the data one line at a time. Record frequencies for both 1 and 2. Loop through hash table and convert frequencies into probabilities: freq/total = probability.

## Simple Version of Transducer
Make a 2 dimensional array (or equivalent). Columns represent tokens at positions in the text: 0 = start of sentence, N = Nth token (word punctuation at position N), Length+1 = end of sentence. Rows represent S states: the start symbol, the end symbol and all possible POS (NN, JJ, ...). Cells represent the likelihood that a particular word is at a particular state.

Traverse the chart as per the algorithm (fish sleep slides, etc.) – once per sentence.
- For all states at position 1, multiply transition probability from Start (position 0) by likelihood that word at position 1 occurs in that state. Choose highest score for each cell.
- For n from 2 to N (columns)
    - for each cell [n,s] in column n and each state [n-1,s'] in column n-1:
    - get the product of:
        - likelihood that token n occurs in state s
        - the transition probability from s' to s
        - the score stored in [n-1,s']
- At each position [n,s], record the max of the s scores calculated.

## Calculating Probabilities
The probability of each transition to state N for token T is assumed to be the product of 3 factors:
- Probability that state N occurs with token T: There is 100% chance that the start state will be at the beginning of the sentence. There is 100% chance that the end state will be at the end of the sentence. If a token was observed in the training corpus, look up probability from table. For Out of Vocabulary words, use a strategy (e.g., 1/1000 or 100% divided by number of states).
- Probability that state N occurs given previous state: Look up in table, calculate for every possible previous state.
- Highest Probability of previous state (calculate for each previous state).

For each new state, choose the highest score (this is the bigram model). Choose the POS tag sequence resulting in the highest score in the end state.

## OOV Strategies
Default (use until other parts of program are debugged): Assume all POS tags have the same probability (e.g., 1/1000). In effect, only use transitions to predict the correct tag.

Morphology: Use prefixes, suffixes, uppercase/lowercase, hyphenation, to predict POS classes of OOV words. Assign "made up" values based on these features. Perhaps hard-code unusual punctuation based on Penn Treebank specs.

Compute probability of UNKNOWN_WORD: Treat words occurring once in training collectively as UNKNOWN_WORD. Don't record them separately (recalculate likelihood table). UNKNOWN_WORD probability used for OOV words by transducer.

Use Smoothing (as per these slides, J & M or elsewhere).

Combination: UNKNOWN_ending_in_s, UNKNOWN_ending_in_ed, UNKNOWN_with_capital_letter, ...

## How you Might Improve your Score
Do error analysis on development corpus – base changes on your findings. Example: Are there errors for punctuation (which should be almost unambiguous)?

Implement a trigram algorithm (see Jurafsky and Martin 2nd edition p. 149). 4-gram is a waste of time for this size corpus.

Note: A clever OOV system contributes more to score than trigram.

Manual rule system using constraints, e.g., for words with frequency>1, assume the disjunction of observed labels is possible. Rule out possibilities according to constraints. Run this and compare results with HMM system. Figure out way of combining results with HMM based on error analysis.

Voting, weighted combinations, etc.

## BIO Tags
A way to recognize multi-token chunks using Viterbi and similar methods.

Constituents consist of 3 kinds of token tags:
- B – Begin Constituent
- I – Inside Constituent
- O – Outside Constituent

Constituent → BI*

Example: Names: Adam Meyers teaches at New York University in the United States.
Adam/B-Name Meyers/I-Name teaches/O-Name at/O-Name New/B-Name York/I-Name University/I-Name in/O-Name the/O-Name United/B-Name States/I-Name ./O-Name

Noun Groups: The book on the wooden table is covered with dust.
The/B-NG book/I-NG on the/B-NG wooden/I-NG table/I-NG is covered with dust/B-NG.

## Techniques
Similar to the HMM model we just discussed to handle some types of constituents (more detail in a later class). The transition probability is the same, but the likelihood probability is replaced with sets of features.