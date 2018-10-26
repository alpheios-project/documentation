# Alpheios Language Resources Overview

## Resource/Service Functions

### Basic

*Morphology Service*: takes a word as input and returns a morphological parse of that word expressed in the [Alpheios lexicon schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/lexicon.xsd)

*Lexicon Service*: takes a lemma as input and returns a full definition as either raw XML or formatted HTML

*Short Definition Index*: a simple index file mapping lemmas to short definitions as plain text 

*Grammar*: HTML Grammar indexed by alpheios inflection feature values 

### Advanced

*Inflection tables*: Description pending

*Treebank data*: treebank data enables display of tree diagrams and use of treebank data to disambiguate parser results. It must meet the following prerequisites:

* Has been aligned to a text at the word and sentence level
* Adheres to the Perseus/Alpheios Treebank Schema
* Uses one of the tagsets supported by Arethusa
    
*Translation Alignment data*: Texts can included aligned translations which have been linked at the word or phrase level by supplying the data-alpheios_align_ref attribute on elements which have been aligned.

*Translation Glosses*: Description pending

## Resource/Service Details

### Morphology Service Provided Engines

The [Morphology Service](https://github.com/alpheios-project/morphsvc) is a Python Flask app which provides a single API endpoint for multiple morphological engines.  New engines can be easily added as plugins, either as Python libraries, as a local binary, or as a remote service. 

The service plugin can provide a transformation from parser-specific output to the [Alpheios lexicon schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/lexicon.xsd), or the engine itself can provide the data in that format. The engine may optionally include a short definition or gloss for a term along with morphological attributes.

The service also adds an OA wrapper around the parser results. The service uses a cache layer to cache results, but engine performance must be within acceptable web response times.

Example service request/response:

Parse the Latin word 'aberis' via Whitaker's Words engine:

```
Request: http://morph.alpheios.net/api/v1/analysis/word?word=aberis&lang=lat&engine=whitakerLat

Input params:
  word=aberis
  lang=lat
  engine=whitakerLat

Response (XML): 

<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
  <oac:Annotation xmlns:oac="http://www.openannotation.org/ns/" rdf:about="urn:TuftsMorphologyService:aberis:whitakerLat">
    <dcterms:creator xmlns:dcterms="http://purl.org/dc/terms/">
      <foaf:Agent xmlns:foaf="http://xmlns.com/foaf/0.1/" rdf:about="net.alpheios:tools:wordsxml.v1"/>
    </dcterms:creator>
    <dcterms:created xmlns:dcterms="http://purl.org/dc/terms/">2018-10-26T16:11:08.195101</dcterms:created>
    <dc:rights xmlns:dc="http://purl.org/dc/elements/1.1/">Short definitions and morphology from Words by William Whitaker, Copyright 1993-2007.</dc:rights>
    <oac:hasTarget>
      <rdf:Description rdf:about="urn:word:aberis"/>
    </oac:hasTarget>
    <dc:title xmlns:dc="http://purl.org/dc/elements/1.1/"/>
    <oac:hasBody rdf:resource="urn:uuid:idm140190922138888"/>
    <oac:Body rdf:about="urn:uuid:idm140190922138888">
      <rdf:type rdf:resource="cnt:ContentAsXML"/>
      <cnt:rest xmlns:cnt="http://www.w3.org/2008/content#">
        <entry>
          <infl>
            <term xml:lang="lat">
              <stem>ab</stem>
              <suff>eris</suff>
            </term>
            <pofs order="3">verb</pofs>
            <conj>5th</conj>
            <var>1st</var>
            <tense>future</tense>
            <voice>active</voice>
            <mood>indicative</mood>
            <pers>2nd</pers>
            <num>singular</num>
          </infl>
          <dict>
            <hdwd xml:lang="lat">absum, abesse, abfui, abfuturus</hdwd>
            <pofs order="3">verb</pofs>
            <freq order="3">lesser</freq>
            <src>Lewis+Short</src>
          </dict>
          <dict>
            <hdwd xml:lang="lat">absum, abesse, afui, afuturus</hdwd>
            <pofs order="3">verb</pofs>
            <freq order="6">very frequent</freq>
            <src>Ox.Lat.Dict.</src>
          </dict>
          <mean>be away/absent/distant/missing; be free/removed from; be lacking; be distinct;</mean>
        </entry>
      </cnt:rest>
    </oac:Body>
  </oac:Annotation>
</rdf:RDF>

Response (JSON): 

{
  "RDF": {
    "Annotation": {
      "about": "urn:TuftsMorphologyService:aberis:whitakerLat",
      "creator": {
        "Agent": {
          "about": "net.alpheios:tools:wordsxml.v1"
        }
      },
      "created": {
        "$": "2018-10-26T16:13:21.592351"
      },
      "rights": {
        "$": "Short definitions and morphology from Words by William Whitaker, Copyright 1993-2007."
      },
      "hasTarget": {
        "Description": {
          "about": "urn:word:aberis"
        }
      },
      "title": {},
      "hasBody": {
        "resource": "urn:uuid:idm140190923968696"
      },
      "Body": {
        "about": "urn:uuid:idm140190923968696",
        "type": {
          "resource": "cnt:ContentAsXML"
        },
        "rest": {
          "entry": {
            "infl": {
              "term": {
                "lang": "lat",
                "stem": {
                  "$": "ab"
                },
                "suff": {
                  "$": "eris"
                }
              },
              "pofs": {
                "order": 3,
                "$": "verb"
              },
              "conj": {
                "$": "5th"
              },
              "var": {
                "$": "1st"
              },
              "tense": {
                "$": "future"
              },
              "voice": {
                "$": "active"
              },
              "mood": {
                "$": "indicative"
              },
              "pers": {
                "$": "2nd"
              },
              "num": {
                "$": "singular"
              }
            },
            "dict": [
              {
                "hdwd": {
                  "lang": "lat",
                  "$": "absum, abesse, abfui, abfuturus"
                },
                "pofs": {
                  "order": 3,
                  "$": "verb"
                },
                "freq": {
                  "order": 3,
                  "$": "lesser"
                },
                "src": {
                  "$": "Lewis+Short"
                }
              },
              {
                "hdwd": {
                  "lang": "lat",
                  "$": "absum, abesse, afui, afuturus"
                },
                "pofs": {
                  "order": 3,
                  "$": "verb"
                },
                "freq": {
                  "order": 6,
                  "$": "very frequent"
                },
                "src": {
                  "$": "Ox.Lat.Dict."
                }
              }
            ],
            "mean": {
              "$": "be away/absent/distant/missing; be free/removed from; be lacking; be distinct;"
            }
          }
        }
      }
    }
  }
}
```

In the above example, the whitaker's engine returned the data as follows:

```
<entry>
    <infl>
      <term xml:lang="lat">
        <stem>ab</stem>
        <suff>eris</suff>
      </term>
      <pofs order="3">verb</pofs>
      <conj>5th</conj>
      <var>1st</var>
      <tense>future</tense>
      <voice>active</voice>
      <mood>indicative</mood>
      <pers>2nd</pers>
      <num>singular</num>
    </infl>
    <dict>
      <hdwd xml:lang="lat">absum, abesse, abfui, abfuturus</hdwd>
      <pofs order="3">verb</pofs>
      <freq order="3">lesser</freq>
      <src>Lewis+Short</src>
    </dict>
    <dict>
      <hdwd xml:lang="lat">absum, abesse, afui, afuturus</hdwd>
      <pofs order="3">verb</pofs>
      <freq order="6">very frequent</freq>
      <src>Ox.Lat.Dict.</src>
    </dict>
    <mean>be away/absent/distant/missing; be free/removed from; be lacking; be distinct;</mean>
</entry>
```

The service adds the OA wrapper and the transformation to JSON.

### Lexicons

The Alpheios Lexicon service provides access to lexicons. Minimum requirements for a new lexicon is that it be provided in XML which complies with the [TEI P5 Dictionaries Guidelines](http://www.tei-c.org/release/doc/tei-p5-doc/en/html/DI.html). The Alpheios Lexicon service provides indexed access to the entries in the lexicon. An additional mapping may be needed to map the lemmas indexed in the lexicon to the lemmas identified by the morphological parser.

Example service request/response:

Retrieve the full definition formatted for the Latin word 'absum' from the Lewis and Short lexicon:

```
Request: https://repos1.alpheios.net/exist/rest/db/xq/lexi-get.xq?lx=ls&lg=lat&out=html&l=absum

Input params:
  lx=ls
  lang=lat
  l=absum

Reponse: 
<alph:output xmlns:alph="http://alpheios.net/namespaces/tei">
    <alph:source>From <title>
            <title>A Latin Dictionary</title>
        </title> (Charlton T. Lewis, Charles Short, Oxford, Clarendon Press, 1879) </alph:source>
    <alph:entry lemma-id="n254" body-key="">
        <entryFree id="n254" type="main" key="absum">
            <orth extent="full" lang="la">ab-sum</orth>, <itype>āfui</itype> (better than abfui),
		āfŭtārus (aforem, afore), <pos>v.
		n.</pos>, in its most general signif., 
	    <sense id="n254.0" n="I" level="1">
                <hi rend="ital">to be away from</hi>, <hi rend="ital">be
				absent.</hi>
            </sense>
            ...
        </entryFree>
    </alph:entry>
</alph:output>
```

### Short Definition Indices

Short definitions are provided via index files which map lemmas identified by the morphological parser to short definitions or glosses for those lemmas. These are simple | separated files, which can be generated from the TEI XML of a lexicon if provided.

Sample data:

```
...
ῥῆγμα|breakage, fracture
ῥῆγος|rug, blanket
ῥῆμα|that which is said or spoken, word, saying
ῥῆξις|breaking, bursting
ῥῆον|rhubarb
ῥῆσις|saying, speech
ῥῖγος|frost, cold
ῥῖμμα|throw, cast
ῥῖπος|mat or hurdle
ῥῖψις|throwing, hurling
...
```

### Grammar

Grammars can be provided in HTML. The minimum requirement is that an index be provided which maps values of morphological features to locations in the grammar.

Example index:

```
alph-case,part2.htm#alph-case
alph-case-ablative,part5.htm#alph-case-ablative
alph-case-accusative,part5.htm#alph-case-accusative
alph-case-dative,part5.htm#alph-case-dative
alph-case-genitive,part5.htm#alph-case-genitive
alph-case-locative,part5.htm#alph-case-locative
alph-case-nominative,part5.htm#alph-case-nominative
alph-case-vocative,part5.htm#alph-case-vocative
alph-comparison,part2.htm#alph-comp
alph-conjugation,part2.htm#alph-conj
alph-conjugation-1st,part2.htm#alph-conj-1st
alph-conjugation-2nd,part2.htm#alph-conj-2nd
alph-conjugation-3rd,part2.htm#alph-conj-3rd
alph-conjugation-4th,part2.htm#alph-conj-4th
alph-conjugation-irregular,part2.htm#alph-conj-irregular
alph-declension,part2.htm#alph-decl
alph-declension-1st,part2.htm#alph-decl-1st
alph-declension-2nd,part2.htm#alph-decl-2nd
alph-declension-3rd,part2.htm#alph-decl-3rd
alph-declension-4th,part2.htm#alph-decl-4th
alph-declension-5th,part2.htm#alph-decl-5th
alph-gender,part2.htm#alph-gender
alph-general-index,index.htm#alph-general-index
alph-mood,part5.htm#alph-mood
alph-mood-imperative,part5.htm#alph-mood-imperative
alph-mood-indicative,part5.htm#alph-mood-indicative
alph-mood-infinitive,part5.htm#alph-mood-infinitive
alph-mood-participle,part5.htm#alph-mood-participle
alph-mood-subjunctive,part5.htm#alph-mood-subjunctive
alph-number,part2.htm#alph-num
alph-part of speech,part2.htm#alph-pofs
alph-part of speech-adjective,part2.htm#alph-pofs-adjective
alph-part of speech-adverb,part3.htm#alph-pofs-adverb
alph-part of speech-conjunction,part3.htm#alph-pofs-conjunction
alph-part of speech-interjection,part3.htm#alph-pofs-interjection
alph-part of speech-noun,part2.htm#alph-pofs-noun
alph-part of speech-numeral,part2.htm#alph-pofs-numeral
alph-part of speech-preposition,part3.htm#alph-pofs-preposition
alph-part of speech-pronoun,part2.htm#alph-pofs-pronoun
alph-part of speech-supine,part5.htm#alph-pofs-supine
alph-part of speech-verb,part2.htm#alph-pofs-verb
alph-part of speech-verb participle,part5.htm#alph-pofs-verb_participle
alph-table-of-contents,toc.htm#alph-table-of-contents
alph-tense,part5.htm#alph-tense
alph-tense-present,part5.htm#alph-tense
alph-tense-imperfect,part5.htm#alph-tense
alph-tense-future,part5.htm#alph-tense
alph-tense-future perfect,part5.htm#alph-tense
alph-tense-pluperfect,part5.htm#alph-tense
alph-tense-perfect,part5.htm#alph-tense
alph-voice,part5.htm#alph-voice
```
