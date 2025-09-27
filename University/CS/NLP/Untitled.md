

## File I/O in Python
```python
# Reading files
with open('filename.txt', 'r') as file:
    content = file.read()  # entire file as string
    lines = file.readlines()  # list of lines

# Writing files
with open('output.txt', 'w') as file:
    file.write("Some text\n")

# Processing line by line
with open('file.txt', 'r') as file:
    for line in file:
        process(line)
```

## Regular Expressions (re module)
```python
import re

# Basic patterns
pattern = r'\bword\b'  # raw strings recommended
result = re.search(pattern, text)  # find first match
all_matches = re.findall(pattern, text)  # all matches

# Common patterns:
# \d - digits, \w - word chars, \s - whitespace
# [abc] - character set, * - 0 or more, + - 1 or more
# ^ - start of string, $ - end of string

# Example: find all email addresses
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
```

## NLTK Basics
```python
import nltk
# You might need to download data first:
# nltk.download('punkt')  # for tokenizers
# nltk.download('stopwords')  # common stop words

from nltk.tokenize import word_tokenize, sent_tokenize
from nltk.corpus import stopwords

# Tokenization
sentences = sent_tokenize(text)  # split into sentences
words = word_tokenize(text)      # split into words

# Stop words
stop_words = set(stopwords.words('english'))
filtered_words = [word for word in words if word.lower() not in stop_words]

# Frequency analysis
from nltk import FreqDist
freq_dist = FreqDist(words)
common_words = freq_dist.most_common(10)
```

Let me break this down step by step. This is actually a great example of **chunking** with NLTK!

## Understanding the Code

### 1. **POS Tagging Basics**
```python
pos_tagged_items = nltk.pos_tag(tokens)
```
This assigns part-of-speech tags to each word:
- `NN` = noun, `JJ` = adjective, `VB` = verb, `DT` = determiner, etc.

### 2. **Chunk Grammar = Regex for POS Tags**
The patterns like `{(<CD|DT|JJ|NN|PRP\$>)*(<NN|NNS>)}` are **regex patterns for POS tags**, not for the words themselves.

## Regex Crash Course for Chunking

### Basic Syntax for Chunk Patterns:
```
{ pattern }     # defines a chunk
<POS_TAG>       # matches a specific POS tag
*               # zero or more of the previous
?               # zero or one of the previous
|               # OR operator
()              # grouping
```

### Breaking Down Your Examples:

**Noun Group (NG) Pattern:**
```python
NG: {(<CD|DT|JJ|NN|PRP\$>)*(<NN|NNS>)}
```
Translation: "A noun group starts with zero-or-more of (CD OR DT OR JJ OR NN OR PRP$), followed by a noun (NN or NNS)"

Example: "the big grey dog" → `DT JJ JJ NN`

**Verb Group (VG) Pattern:**
```python
VG: {<MD|VB|VBD|VBN|VBZ|VBP|VBG>*<VB|VBD|VBN|VBZ|VBP|VBG><RP>?}
```
Translation: "A verb group has zero-or-more auxiliary verbs, followed by a main verb, optionally followed by a particle"

Example: "may have ended up" → `MD VB VBN RP`

## Writing Your Own Chunk Patterns

### Common POS Tags to Know:
- `NN`/`NNS` - nouns
- `VB`/`VBD`/`VBZ` etc. - verbs  
- `JJ` - adjectives
- `RB` - adverbs
- `DT` - determiners (the, a, an)
- `IN` - prepositions
- `CD` - numbers

### Pattern Building Blocks:
```python
# Simple noun phrase: determiner + adjective + noun
NP: {<DT><JJ><NN>}

# More flexible: any number of adjectives before noun
NP: {<DT><JJ>*<NN>}

# Even more flexible: optional determiner, any adjectives, noun
NP: {(<DT>)?<JJ>*<NN>}

# Multiple options: noun or pronoun
NP: {<NN|NNS|PRP>}

# Prepositional phrase: preposition + noun phrase
PP: {<IN><NP>}
```

## Debugging Your Patterns

Notice how they fixed "right" from `JJ` (adjective) to `RB` (adverb). The POS tagger isn't perfect - you'll need to check its work.

**Quick test pattern:**
```python
# Test a simple pattern first
test_grammar = nltk.RegexpParser(r"""
NP: {<DT><NN>}  # just determiner + noun
""")
```

## Your Assignment Approach

1. **Start simple** - get a basic pattern working
2. **Test frequently** - print results after each change
3. **Build incrementally** - add one piece at a time
4. **Check POS tags** - make sure the tagger is labeling words correctly
