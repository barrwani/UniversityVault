# Tense and Aspect
#linguistics 


## Verbal Complex 

Verbal complex of an English sentence has the following form:
`{PRES,PAST,FUT} (PERF) (PROG) VP`

Algorithm for predicting the constraints on meaning imposed by the verbal complex:
1. Draw a time line, mark point U (utterance)
2. Work through verbal elements from left to right executing whichever rules apply:
	1. PAST:  Add E (event) to the left of U
	2. PRES: change label from U to U,E
	3. FUT (modal will): Add E to right of U
	4. PERF: Change label from E to E,R. Move E leftwards.
	5. PROG: smear E ( -- becomes == )
	6. Verb: 
		1. Punctual: Point under E
		2. Durative: Wavy line under E
		3. Telic: Closed bracket at end of wavy line


## Inference Rules

Numerator shows premises, hypothesis, or assumptions. 
Denominator shows conclusion or result of the rule


$\frac{A\text{ and }B}{B}$ $\frac{A\text{ and }B}{A}$ $\frac{A}{A\text{ or }B}$ $\frac{B}{A\text{ or }B}$

$[[A\text{ and }B]] = [[A]] \cap [[B]]$
$[[A\text{ or }B]] = [[A]] \cup [[B]]$


## Existential *There*

When can a determiner occur in the following frame?
`Isn't/aren't there ___ ponies in the garden?`
1. Normally determiners take two arguments, NP and VP
2. NP and VP denote sets of individuals
3. If truth of a sentence of the form "Det NP VP" can be decided purely on the basis of examining the objects in the NP set, we say that the determiner is **conservative** on it's first argument
4. HYP: All determiners are conservative on their first argument 
	1. possible exceptions only and just if they are determiners
5. HYP: A determiner can occur in the existential there frame if and only if it is conservative on its second argument 
	1. If the truth of the sentence "Det NP VP" can be determined purely on the basis of examining the objects in the VP set


## Negative Polarity

Contexts that license Negative Polarities (NPIs)
1. Negation
2. Yes/No Questions
3. Conditionals
4. Superlatives
5. Inherent Negation
	1. Rarely
	2. Doubt, Refuses
6. Only

Core Negative Polarity: any N, ever.

Upwards Entailing (UE): Subset -> Superset


Downward Entailing (DE): Superset -> Subset

Hypothesis: Negative Polarities are licensed in DE contexts


1. Amy doubts that Bill ate fish
2. Amy doubts that Bill ate cod

1->2, Downwards Entailing.

3. Amy doubts that Bill ever ate any fish
	- Works

4. Amy believes that Bill ate fish
5. Amy believes that Bill ate cod
4->5 doesn't work, 5->4 does, Upwards Entailing.

6. Amy believes that Bill ate any fish
	- Doesn't work


A question is a DE iff whenever the answer to the superset is no, the answer to the subset is no.