# Alpheios User Vocabulary Requirements

## User Stories

### User Perspective

U1. As a user I want to save a word, with its context, to my list of words so that I can study it.

U2. As a user I want to be able to specify and change a degree of confidence in my knowledge of the word so that I can prioritize it.

U3. As a user I want to see all contexts in which I have seen a word.

U4. As a user I want to see the contexts in which a word occurs in the literature so that I can understand it better.

U5. As a user I want to be able to generate a target vocabulary list for any text I want to read so that I can see what I need to learn.

U6. As a user I want to be able to compare my word list against a proficiency target so that I can assess my progress.

U7. As a user I want to know how often a word appears in the literature across various facets (genre, author, time period, etc.) so that I can understaned its relevance to my studies.

U8. As a user I want to know how often a word appears in a given form in the literature across various facets (genre, author, time period, etc.) so that I can understand its relevance to my studies.

U9. As a user I want to be able to store data about a word.

U10. As a user I want to be able to generate a vocabulary list at the end a reading session so that I can see everything I had to look up in that session.


### Application Perspective

A1. As an application I want to store a word to a user's word list so that I can access it later

A2. As an application I want to retrieve a list of a user's words so that I can present them to the user

A3. As an application I want to store data about a word to a user's word list so I can access it later

A4. As an application I want to retrieve data about a word on a user's word list so that I can present it to the user.

A5. As an application I want to store occurrences of a user's interaction with a word on their word list so that this data is available for future uses.

A6. As an application I want to retrieve data about occurrences of a user's interaction with a word on their word list so that I can present individualized features.

A7. As an application I want to be able to store the context of any word that the user looks up (e.g. the sentence)  so that I can use it to analyze collocations to classify uses, detect alternative meanings and enable ranked searches for similar occurrences in the corpus.      

A8. As an application I want to be able to retrieve the context of any word that the user looks up (e.g. the sentence)  so that I can use it to analyze collocations to classify uses, detect alternative meanings and enable ranked searches for similar occurrences in the corpus.      

A9. As an application I want to be able to query statistical information about how often a word appears in the literature across various facets (genre, author, time period, etc.) so that I can use it to inform what features I offer for the word.

A10. As an application I want to be able to create a target vocabulary proficiency map from a target text that reflects all uses of each word in that text so that I can compare a user's word list against that map.

A11. As a application I want to be able to retrieve data about a word from external linked data sources so that I can present these sources to the user.

## Architectural Perspective

I1. Vocabulary functionality should be available via a API

I2. Vocabulary functionality should be language-agnostic (i.e same API for latin, greek, etc)

I3. It must be possible to aggregate user vocabulary data across multiple applications

I4. It must be possble to distribute all or part of user vocabulary data to individual applications and devices.

I5. User identification and authentication be decoupled from the vocabulary functionality

I6. Multiple applications must be able to store and retrieve data upon behalf of the same single user

I7. We need a scalable workflow for managing corpus-level vocabulary lists.

 
## Data we might want to attach to a word

* language 
* locations of user's encounters with the word
* Metadata identifying the work, author, etc.
* lemma
* morphological analyses and inflected forms
* definitions
* glosses
* orthographic variants
    * with an indication of their pattern of distribution
* pronunciation in Internation Phonetic Alphabet
    * chief variants and their distribution
    * including accent and syllable length
    * audio recording- male and female voice
        * alone and in sentences
    * rhyming words- by work- accumulation by author, period etc
        * use of partial rhymes
* earliest attested appearance
* frequency in all surviving works , including inscriptions, pots etc
    * by author, by genre, by year
* frequency of collocation words
    * by author, by genre, by year
* frequency of specific morphological forms
    * by work, author, genre, year, register etc
* frequency of specific syntactic roles
    * by work, author, genre, year, register etc
* position in works and sentences
    * relative and absolute 
* position in various wordnets
    * synset number
    * synonyms
    * antonyms
    * homonyms
    * hypernyms- and ancestors
    * hyponyms-  and children
* explanations and descriptions in the same language
    * including distinctions of alternative uses/meanings
    * examples of use appearing in dictionaries and texts
        * authentic/toy
* explanations and synonyms in various languages
    * frequency in translation of specific works, authors etc
    * entries in all available dictionaries
    * meaning groupings found in various dictionaries
* memorability
    * to specific kinds of learners
    * eg concreteness (nouns vs verbs), memorable sound patterns (strong contrasts
* sentimental weight
    * to specific kinds of learners
* evoked associations
    * other words
    * images
    * sounds  (klang)
* frequency of use by specific kinds of speakers - in drama, narratives ...
    * male female
    * age
    * role/ social position- clown, hero etc
* frequency of use in specific types of speech and for specific purposes
    * registers- formal, informal, ethnic,  etc
    * injunctions, imprecations, appeals, mockery, elegies, exhortations, etc
* etymology and cognates-
    * temporal and spatial dynamic spread
    * derived words in all modern languages
