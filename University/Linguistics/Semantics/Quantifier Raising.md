# Quantifier Raising
#linguistics 


## Quantificational DPs

Research question: What are the truth conditions for sentences containing quantificational DPs?

#### Algorithm:
1. Raise each quantificational DP to adjoin to S, leaving behind a copy of the DP's index. 
2. Insert a $\lambda x$ underneath the new S node, copying the DP's index
	1. This movement creates a structure that defines a set of objects. 
3. Give the quantificational determiner the same interpretation we gave it for the frame "Det NP VP"

Example:
1.  "Ann ate no squid"
2. Move "no squid" to adjoin to the matrix S node. 
3. Insert a $\lambda x$ node immediately under the raised DP, leaving a copy of the same variable $x$ in the original position of the DP.
4. Then we have \[ \[no squid] ($\lambda x$ \[Ann ate $x$] ) ] 

This theory makes good predictions about quantifier scope ambiguity. 


![[Pasted image 20241217201255.png]]

## Pronoun Values

Research Question: How do pronouns get their values?

#### Hypothesis

Pronouns can be assigned an index that matches the index of a quantificational DP.

When that happens, if the DP raises to a position that commands the pronoun, the pronoun can be bound by the quantifier. 


Example:

1. Ann gave no boy a picture of his mother
2. Assign both "every boy" and "his" the index $x$
3. This sentence will be true just in the case of the intersection of the set of boys with the set of objects $x$ such that "Ann gave $x$ a picture of $x$'s mother" is empty.