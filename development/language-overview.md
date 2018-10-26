# Language Overview

## Basic Resources and Roles

1. Morphology Service: takes a word as input and returns a morphological parse of that word expressed in the [Alpheios lexicon schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/lexicon.xsd)

2. Lexicon Service: takes a lemma as input and returns a full definition, formated as HTML

3. Short Definition Index: a simple index file mapping lemmas to short definitions as plain text 

4. Grammar: Grammar in HTML plus an index which maps alpheios inflection feature values to the file/target in the HTML 

## Advanced Resources and Roles

1. Treebank data

2. Translation alignments


## Resource/Service Details

### Morphology Service

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

And the service adds the OA wrapper and the transformation to JSON.


