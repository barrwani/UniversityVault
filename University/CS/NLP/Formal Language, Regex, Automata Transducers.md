# Formal Languages, Regular Expressions, Automata, Transducers

## Outline
- Formal Languages in the Chomsky Hierarchy
- Regular Expressions
- Finite State Automata
- Finite State Transducers
- Some Sample CL tasks using Regexps
- Concluding Remarks

## Formal Language = Set of Strings of Symbols
A Formal Language Can Model a Phenomenon, such as written English.

Examples:
- All Combinations of the letters A and B: ABAB, AABB, AAAB, etc.
- Any number of As, followed by any number of Bs: AABB, AB, AAAAAAAABBB, etc.
- Mathematical Equations: 1+2=5, 2+3=4+16=6
- All the sentences of a simplified version of written English, e.g., "My pet wombat is invisible."
- A sequence of musical notation, e.g., the notes in Beethoven's 9th Symphony

## What is a Formal Grammar For?
A formal grammar consists of a set of rules that matches all and only instances of a formal language. It defines a formal language.

In Computer Science, formal grammars are used to generate and recognize formal languages (e.g., programming languages). Parsing a string of a language involves recognizing the string and recording the analysis showing it is part of the language.

A compiler translates from language X to language Y, which may include parsing language X and generating language Y. If all natural languages were formal languages, then Machine Translation systems could be compilers consisting of a parser producing a structured analysis of the source language sentence and a generator that can generate a target language sentence from this analysis.

## A Formal Grammar Consists Of
- N: a Finite set of nonterminal symbols (symbols that can be replaced by other symbols)
- T: a Finite set of terminal symbols (symbols that cannot be replaced by other symbols)
- R: a set of rewrite rules, e.g., XYZ → abXzY (replace the symbol sequence XYZ with abXzY)
- S: A special nonterminal that is the start symbol

## A Very Simple Formal Grammar
Language AB = 1 or more a, followed by 1 or more b, e.g., ab, aab, abb, aaaaaaabb, etc.

N = {A,B}
T = {a,b}
S = ∑
R = {A → a, A → Aa, B → b, B → Bb, ∑ → AB}

## Generating a Sample String
Start with ∑
Apply ∑→AB, Generate A B
Apply A→Aa, Generate A a B
Apply A→Aa, Generate A a a B
Apply A→a, Generate a a a B
Apply B→b, Generate a a a b

## Phrase Structure Tree for a a a b
Terminal symbols label leaves. Nonterminal symbols label non-leaves. The terms "leaf" and "non-leaf" are usually applied to trees. Most NLP graphs are trees.

## The Chomsky Hierarchy: Type 0 and 1
Type 0: No restrictions on rules. Equivalent to Turing Machine (general system capable of simulating any algorithm).

Type 1: Context-sensitive rules: αAβ → αγβ, where Greek letters = 0 or more nonterms/terms, A = nonterminal. The rule means: replace A with γ when A is between α and β.

For example, DUCK DUCK DUCK → DUCK DUCK GOOSE means convert DUCK to a GOOSE if preceded by 2 DUCKS.

## Chomsky Hierarchy Type 2: Context-free rules
Like context-sensitive, except left-hand side can only contain exactly one nonterminal.

Example Rule from linguistics:
NP → POSSP n PP
NP → Det n
NP → n
POSSP → NP 's
PP → p NP

Example tree: [NP [POSSP [NP [Det The] [n group]] 's] [n discussion] [PP [p about] [NP [n food]]]] for "The group's discussion about food".

## Chomsky Hierarchy Type 3: Regular (finite state) grammars
Rules: A → βa or A → ε (left regular) or A → aβ, or A → ε (right regular).

Like Type 2, except right hand side is constrained. Non-terminals precede (but don't follow) terminals in left regular grammar. Non-terminals follow (but don't precede) terminals in right regular grammar. The null string is allowed.

Example left regular rules from linguistics:
NP → POSSP n
NP → n
NP → det n
POSSP → NP 's

Example: [NP [POSSP [NP [det The] [n group]] 's] [n discussion]] for "The group's discussion".

## The Chomsky Hierarchy
Type0 ⊇ Type1 ⊇ Type2 ⊇ Type3

Type 3 grammars are least expressive but have most efficient processors. Processors for Type 0 grammars are most expressive but least efficient.

Complexity of recognizer for languages:
Type 0 = exponential; Type 1 = polynomial; Type 2 = O(n³); Type 3 = O(n log n)

## CL mainly features Type 2 & 3 Grammars
Type 3 grammars include regular expressions and finite state automata (aka finite state machines). They are the focal point of the rest of this talk. The Nooj platform for NLP works best in Windows.

Type 2 grammars are commonly used for natural language parsers. They are used to model syntactic structure in many linguistic theories (often supplemented by other mechanisms). They are important for later talks on constituent structure and parsing.

## Type 1.5 Grammars
Human Language is believed to be "mildly context sensitive" - less expressive than type 1 (context sensitive) but more expressive than type 2 (context free).

Some complex dependencies cannot be expressed in context free rules. Tree Adjoining Grammars (formalism by A. Joshi and others) may be able to handle these cases.

## Regular Expressions
The language of regular expressions (regexps) is a standardized way of representing search strings (Kleene, 1956).

Computer Languages with regexp facilities include Python, JAVA, Perl, Ruby, most scripting languages. UNIX utilities and text editors (grep, emacs, vi, etc.) and other systems like MySQL, Microsoft Office, Open Office support regexps.

## Breaking Down (BB|[^B]{2})
BB – a sequence consisting of 2 Bs in a row
| - disjunction operator
[^B] – a character that is not a B
{2} – something repeated twice

So the full expression matches BB (two Bs in a row) or a sequence of 2 items, neither is a B, e.g., +*, 12, AC, bf, xY, Ψ.

## Regexp = formula specifying set of strings
A regexp can be:
- The empty set
- The empty string
- A sequence of one or more characters (e.g., X, Y, or "This sentence contains characters like &T/**%P")
- Disjunction, concatenation or repetition of regexps

## Concatenation, Disjunction, Repetition
Concatenation: If X is a regexp and Y is a regexp, then XY is a regexp. Examples: If ABC and DEF are regexps, then ABCDEF is a regexp. If AB* and BC* are regexps, then AB*BC* is a regexp.

Disjunction: If X is a regexp and Y is a regexp, then X | Y is a regexp. Example: ABC|DEF will match either ABC or DEF.

Repetition: If X is a regexp than a repetition of X will also be a regexp. The Kleene Star: A* means 0 or more instances of A. Regexp(number): A(2) means exactly 2 instances of A.

## Regexp Notation
Disjunction of characters: [ABC] means the same thing as A | B | C. Character ranges are equivalent to lists: [a-zA-Z0-9] is equivalent to a|b|c|...|A|B|...|0|1|...|9.

Negation of character lists/sequences: ^ inside bracket means complement of disjunction, e.g., [^a-z] means a character that is neither a nor b nor c ... nor z.

Parentheses disambiguate scope of operators. For example, (ABC)(DEF) means ABC or ADEF? Actually, it means concatenation of (ABC) and (DEF). Without parentheses, defaults apply, e.g., ABC|DEF means ABC or DEF.

? signifies optionality: ABC? is equivalent to AB followed by optional C (AB or ABC).

+ indicates 1 or more: A(BC)+ is equivalent to A followed by one or more BC.

## Regexp Notation Special Symbols
Period means any character, e.g., A.*B matches A and B and any characters between.

Carrot (^) means the beginning of a line, e.g., ^ABC matches ABC at the beginning of a line. Note the dual usage of ^ as negation operator.

Dollar sign ($) means the end of a line, e.g., [.!?] *$ matches final punctuation, zero or more spaces and the end of a line.

Python's Regexp Module includes searching, groups and group numbers, compiling, and substitution. Similar modules exist for Java, Perl, etc.

## Other Details
See various manuals, e.g., https://docs.python.org/3/library/re.html.

Sets of characters: \w = [A-Za-z0-9_], \W = [^A-Za-z0-9_], etc.

All repetition modifiers are greedy by default, but there are non-greedy versions. Usually, unnecessary if you use appropriate parentheses.

## Regexp in NLTK's Chatbot
Running eliza: import nltk; from nltk.chat.eliza import *; eliza_chat()

NLTK's chatbots are in util.py and eliza.py in the nltk/chat/ directory. The Chat object (defined in util.py) includes a substitute method. For each pair in pairs, the 1st item is matched against the input string to produce an answer listed as the 2nd item. The use of %1 indicates repeated parts of the strings. The matching pattern is created with re.compile.

## Regexps in Python
import re imports the regexp package.

Example re functions:
- re.search(regexp, input_string) creates a search object
- re.sub(regexp, repl, string)

Search object methods:
- start() and end() output start and end position in the string
- group(0) outputs whole match
- group(N) outputs the nth group (item in parentheses)

Patterns can be compiled: Pattern1 = re.compile(r'[Aa]Bc'). Methods take additional parameters (e.g., starting position): Pattern1.search('ABcaBc', 2) starts search at position 2.

## Regexp with Unix tools
grep -E '$[0-9..]+' all-OANC | less

Different flavors of regexp used by grep: -P (Perl) and -E (Extended) seem to work pretty well.

In the program less: /\$[0-9..]+ highlights numeric instances. Note some problems with this regexp for characterizing money strings.

## RegExp to Search for Common Types of Numeric Strings
An XML (or html) tag: <[^>]+>

Money: \$[0-9\.,]+. Would this match the string '$,,,,,'? How might we handle cases like "$4 million"? What might be a better regexp for money? (Part 1 of homework)

Others: Dates, Roman Numerals, Social Security, Telephone Numbers, Zip Codes, Library Call Numbers, etc.

Time of Day can be done as a joint exercise.

## A "good" regexp?
It should match most sample cases of the target type of string. It should not match many "incorrect" strings. Sample "correct" and "incorrect" strings can be used to tune regexps. Running on a large set of sample data (like the all-OANC.txt file) can help. The regexp should "generalize" well to correctly match (and not match) cases that are not in the input data.

## NLTK's Regexp Language for Chunking
Example:
```

sentence = "The big grey dog with three heads was on my lap"
tokens = nltk.word_tokenize(sentence)
pos_tagged_items = nltk.pos_tag(tokens)
chunk_grammar = nltk.RegexpParser(r"""
NG: {<CD|DT|JJ|NN|PRP\$>}*(<NN|NNS>)
VG: {<MD|VB|VBD|VBN|VBZ|VBP|VBG>*<VB|VBD|VBN|VBZ|VBP|VBG><RP>?}
""")
chunk_grammar.parse(pos_tagged_items)
```

Structure: 1 rule per line. Nonterminal: Regexp. Regexp = terminals, nonterminals & operators (*+?{}...)

## Chunking Rules
On the right side, Nonterminals can precede terminals.

Example rules:
```

DTP: <PDT><DT|CD>
NG: <CD|DT|JJ|NN|PRP\$|DTP>*<NN|NNS>
PG: <RB><IN|TO>
VP: <VG><NG|PG>*
```
Rules assume Penn Treebank POS tags.


## The Penn Treebank II POS tagset
Verbs: VB, VBP, VBZ, VBD, VBG, VBN (base, present-non-3rd, present-3rd, past, -ing, -en)

Nouns: NNP, NNPS, NN, NNS (proper/common, singular/plural; singular includes mass + generic)

Adjectives: JJ, JJR, JJS (base, comparative, superlative)

Adverbs: RB, RBR, RBS, RP (base, comparative, superlative, particle)

Pronouns: PRP, PRP$ (personal, possessive)

Interrogatives: WP, WP$, WDT, WRB (compare to: PRP, PRP$, DT, RB)

Other Closed Class: CC, CD, DT, PDT, IN, MD

Punctuation: # $ . , : ( ) " " " !"

Weird Cases: FW (deja vu), SYM (@), LS (1, 2, a, b), TO (to), POS (s, '), UH (no, OK, well), EX (it/there)

Newer tags: HYPH, PU

## Finite State Automata
Devices for recognizing finite state grammars (include regexps).

Two types:
- Deterministic Finite State Automata (DFSA): Rules are unambiguous.
- NonDeterministic FSA (NDFSA): Rules are ambiguous. Sometimes more than one sequence of rules must be attempted to determine if a string matches the grammar (backtracking, parallel processing, look ahead).

Any NDFSA can be mapped into an equivalent (but larger) DFSA.

## DFSA for Regexp: A(ab)*ABB?
State transition table showing inputs A, B, a, b, ε and states Q0-Q5.

## DFSA algorithm
D-Recognize(tape, machine)
pointer = beginning of tape
current_state = initial state Q0
repeat until the end of the input is reached:
    look up (current_state, input symbol) in transition table
    if found: set current_state as per table look up
        advance pointer to next position on tape
    else: reject string and exit function
if current_state is a final state: accept the string
else: reject the string

## NDFSA for Regexp: A(ab)*ABB?
State transition table showing inputs A, B, a, b and states Q0-Q5.

## NDFSA algorithm
ND-Recognize(tape, machine)
agenda = {(initial state, start of tape)}
current_state = next(agenda)
repeat until accept(current_state) or agenda is empty:
    agenda = Union(agenda, look_up_in_table(current_state, next_symbol))
    current_state = next(agenda)
if accept(current_state): return(True)
else: false

Accept if at the end of the tape and current_state is a final state.

Next defined differently for different types of search: choose most recently added state first (depth first) or choose least recently added state first (breadth first).

## A Right Regular Grammar Equivalent to: A(ab)*ABB?
Q → A R S
R → ε
R → a b R
S → A B B
S → A B

(Red = Terminal, Black = Nonterminal)

## Hearst Patterns
Simple regexp-style rules from Hearst, et al. (1992). Automatic Acquisition of Hypernyms from Large Text Corpora. Coling 1992.

Hypernym(X,Y) means that X is a type of Y.

Example Text:
- The bow lute, such as the Bambara ndang, is ...
- Bruises, broken bones or other injuries ...
- Poodles are types of dogs

Example Hypernym Relations:
- H(Bambara ndang, bow lute)
- H(Bruises, injuries)
- H(broken bones, injuries)
- H(poodles, dogs)

## Hearst Patterns 2
The examples above all have a signal (or predicate) such as "such", "other", "type".

Examples without predicates:
- Yogurt, a popular dairy product (Apposition)
- The Model T (an antique car) (Parentheses)

Hearst paper suggests how to automatically extract hypernym relations from lots of text, possibly generating an ontology in the process. An initial experiment generated 226 examples, 180 of which matched manually annotated cases in WordNet.

## Hearst Patterns 3
Hearst patterns and similar methods can be implemented using regular expressions or similar techniques. The arguments must be close by. Sometimes specific words are required to signal the relation. Sometimes punctuation can be used.

A similar system was implemented for abbreviations (discussed when we talk about Terminology Extraction). Example: Already been chewed (ABC) → abbreviate(already been chewed, ABC). Schwartz and Hearst. 2003. A simple algorithm for identifying abbreviation definitions in biomedical text. Pacific Symposium on Biocomputing.


