Problem statement:

We have the following User Stories in our requirements:

U4. As a user I want to see the contexts in which a word occurs in the literature so that I can understand it better.

U5. As a user I want to be able to generate a target vocabulary list for any text I want to read so that I can see what I need to learn.

U6. As a user I want to be able to compare my word list against a proficiency target so that I can assess my progress.

U7. As a user I want to know how often a word appears in the literature across various facets (genre, author, time period, etc.) so that I can understaned its relevance to my studies.

U8. As a user I want to know how often a word appears in a given form in the literature across various facets (genre, author, time period, etc.) so that I can understand its relevance to my studies.


Fulfillment of these user stories requires that we have a data set which

links distinct forms of each attested word to

1. the location in the work in which it appears 
1. position in wordnets
1. variant spellings
1. lemma
1. pronunciation

links attestations of each instance of a form to

1. morphological attributes
1. syntactic role and relationships
1. surrounding words
1. position in sentence
1. speaker metadata (for drama)
1. senses 
1. glosses

links lemmas to

1. principal parts
1. definitions
1. position in wordnets
1. variant spellings

links works to

1. metadata (genre, author, title, period, etc.)


We have various sources for this data currently:

1. Helma's database (alot of Greek, some Latin)
2. Giuseppe's lemmatized/morphology of Greek
3. James' aggregation of Helma and Giuseppe's data
4. Perseus and Alpheios treebank data (mostly Greek)
5. Gormans' Treebank data (Greek)
6. Harrington's Treebank data (Latin, AP Corpus excerpts +)
7. ....

Challenges 

Attestations come from various TEI XML editions of the primary source. Markup various, corrections have been applied 
so the sources all diverge in smaller or greater ways

No common set of persistent identifiers for either forms or lemmas

Attestations may be incorrect and need to be corrected later. 

Attestations may be ambiguous

Morpho-syntactic annotations and lemmatizations are subject to interpretation.

Morpho-syntactic annotations may use different tag sets.





