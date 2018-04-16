# Alpheios Data Services

Basic use case:

Given:
a _target_  we need to find a corresponding resource for (e.g. a word or a grammatical feature)
a _context_ for the target (e.g. the language, the words before and after, the url of the page, page metadata)
_resources_ Grammars, Lexicons, Annotations (various kinds)
_user preferences_  
_site preferences_

supply _target_ and _context_
filter resources by _preferences_
lookup in resource index
use index data
index data may be the final result or may be a reference to another resource

![Use Case](uc_lookupresource.png)

# Lexicon

@startuml
"app client" -> "lexicon client": get short definition
"lexicon client" -> "data store (GitHub)" : get index file
"data store (GitHub)" -> "lexicon client" : index file (CSV)
"lexicon client" -> "memory": index file (JSON)
"lexicon client" -> "app client": definition
@enduml

@startuml
"app client" -> "lexicon client": get full definition
"lexicon client" -> "data store" : get index file
"data store" -> "lexicon client" : index file (CSV)
"lexicon client" -> "memory": index file (JSON)
"lexicon client" -> "lexicon service": get definition
"lexicon service" -> "lexicon client": definition
"lexicon client" -> "app client": definition
@enduml

# Grammar

@startuml
"app client" -> "res client": get resource url
"res client" -> "data store (alpheios.net)": get index file
"data store (alpheios.net)" -> "res client" : index file (CSV)
"res client" - "memory": index file (JSON)
"res client" -> "app client": resource url
"app client" -> "resource url": GET
@enduml

# Treebank
