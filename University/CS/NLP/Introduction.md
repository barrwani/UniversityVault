# Introduction
#cs #linguistics 

Involves interpretation and generation of (human) text and speech. 

Evaluation:
- System evaluation based on "unseen" data
	- Using development set for training as opposed to test set
	- Training on test set leads to system not being ready for unseen data
	- Separate test set from cross-validation

#### Computational Linguistics Applications:

- Machine translation
	- Input = sentences in 'source' language
	- Output = sentences in 'target' language
- Spoken language
	- dictation
- Information Retrieval
	- Document query search
	- Web search differs, uses ranking algorithm for relevance and order

#### Level of Analysis

- Sentence splitters are >95% accurate
- Words, tokens, other small units
	- [[Morphology]], segmentation, part of speech, lexicography, punctuation, etc.
- Units of sound including [[Phonemic Analysis|phonetics and phonology]]
- Sequences of units based on statistics

File -> Paragraphs -> Sentences -> Phrases -> NP, VP, S, words

- Syntax
	- NLP
		- Descriptive grammars for individual languages
			- Using systems that perform well for specific languages
		- Combo of "frameworks" adopted based on available resources
	- Theoretical Linguistics
		- Theory may not descriptively be adequate for single language
		- Must handle all languages
- Discourse, pragmatics: discourse arguments, anaphora
- [[University/Linguistics/Advanced Semantics/Introduction|Semantics]]: Sense disambiguation, 

#### Models of Blocks of Text, Paragraphs, Documents

- Units represented as Vector of weights
	- Weights represent properties of words:
		- frequency
		- rareness
		- co-occurrence with other words
	- Derived by expectation maximization and other approaches to optimization
		- update weights until condition is satisfied
	- Units often assigned features based on vector similarity

#### Lowest Level Syntax Processing

- Part of Speech Tags
	- Apply a set of part of speech tags to a set of tokens 
	- "They/PRB announced /VBD in/IN unison/NN"
	- NLTK: `nltk.pos_tag(tokens)`
	  
- Tokenization and segmentation
	- Given a sentence, determine the words or word-like units that consist of 
		- They announced in unison,  "we don't agree with each other"
		- Tokenization: "They | announced | in | unison | , " | We | do| n't | agree | with | each | other | . |"
			- Controversial: Don't, each other
	- NLTK: `nltk.word_tokenize(sentence)`

#### Low Level Syntax Processing

- Named Entity Tagging
	- Mark boundaries of names of type PERSON, ORGANIZATION, FACILITY, GPE, LOCATION, ...
	- NLTK: `nltk.chunk.ne_chunk(nltk.pos_tag(nltk.word_tokenize(test_sentence)))`
- Chunking
	- Mark verb groups and/or noun groups, convenient approximations of syntactic units
	- $[_{NG} \text{ The book }] \text{ with }[_{NG} \text{ the blue cover }] [_{VG}\text{ will end up }] \text{ on } [_{NG} \text{ the shelf }]$  

#### Semantics

- Meaning
- Word Sense Disambiguation
	- same word diff meaning
- Predicate argument structure
	- characterize predictable paraphrase
- Anaphora
	- avoiding repetition, using diff word to refer to same thing
	- Coreference, bridging, type coreference, other coreference
- Discourse argument structure
	- "they wanted to steal the diamonds", "however, they did not possess the necessary skills"
- "Semantic Parsing" aka GLARF