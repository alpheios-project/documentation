# Alpheios Language Resources Overview

## Resources and Roles

### Basic 

*Morphology Service*: takes a word as input and returns a morphological parse of that word expressed in the [Alpheios lexicon schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/lexicon.xsd)

*Lexicon Service*: takes a lemma as input and returns a full definition as either raw XML or formatted HTML

*Short Definition Index*: a simple index file mapping lemmas to short definitions as plain text 

*Grammar*: Grammar in HTML plus an index which maps alpheios inflection feature values to the file/target in the HTML 

### Advanced

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

And the service adds the OA wrapper and the transformation to JSON.

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
		n.</pos>, in its most general signif., <sense id="n254.0" n="I" level="1">
                <hi rend="ital">to be away from</hi>, <hi rend="ital">be
				absent.</hi>
            </sense>
            <sense id="n254.1" n="I" level="1"> In gen. </sense>
            <sense id="n254.2" n="A" level="2">
                <usg type="style">
                    <hi rend="ital">Absol.</hi>
                </usg> without designating the distance (opp. <hi rend="ital">adsum</hi>): <cit>
                    <quote lang="la">num ab domo absum?</quote>
                    <bibl n="Perseus:abo:phi,0119,009:5:2:16">
                        <author>Plaut.</author> Ep. 5, 2, 16</bibl>
                </cit>: <cit>
                    <quote lang="la">me absente atque insciente,</quote>
                    <bibl n="Perseus:abo:phi,0119,019:1:2:130">
                        <author>id.</author>
					Trin. 1, 2, 130</bibl>
                </cit>: <cit>
                    <quote lang="la">domini ubi absunt,</quote>
                    <trans>
                        <tr>are not at home</tr>, <tr>not present</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0134,003:3:5:53">
                        <author>Ter.</author>
					Eun. 3, 5, 53</bibl>
                </cit>: facile aerumnam ferre possum, si inde abest injuria, Caecil.
			ap. <bibl>
                    <author>Non.</author> 430,
			18</bibl>.—</sense>
            <sense id="n254.3" n="B" level="2">
			With reference to the distance in space or time; which is expressed
			either by a definite number, or, in gen., by the advs. <hi rend="ital">multum</hi>, <hi rend="ital">paulum</hi> (not <hi rend="ital">parum</hi>, v. below) <hi rend="ital">longe</hi>,
			etc.: <cit>
                    <quote lang="la">edixit, ut ab urbe abesset milia pass. ducenta,</quote>
                    <bibl n="Perseus:abo:phi,0474,022:12:29">
                        <author>Cic.</author>
					Sest. 12, 29</bibl>
                </cit>: <cit>
                    <quote lang="la">castra, quae aberant bidui,</quote>
                    <bibl n="Perseus:abo:phi,0474,057:5:16">
                        <author>id.</author>
					Att. 5, 16</bibl>
                </cit>: <cit>
                    <quote lang="la">hic locus aequo fere spatio ab castris
					Ariovisti et Caesaris aberat,</quote>
                    <bibl n="Perseus:abo:phi,0448,001:1:43">
                        <author>Caes.</author>
					B. G. 1, 43</bibl>
                </cit>: <cit>
                    <quote lang="la">haud longe abesse oportet,</quote>
                    <trans>
                        <tr>he ought not to be far hence</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0119,001:1:1:166">
                        <author>Plaut.</author> Am. 1, 1, 166</bibl>
                </cit>: <cit>
                    <quote lang="la">legiones magnum spatium aberant,</quote>
                    <bibl n="Perseus:abo:phi,0448,001:2:17">
                        <author>Caes.</author>
					B. G. 2, 17</bibl>
                </cit>: <cit>
                    <quote lang="la">menses tres abest,</quote>
                    <bibl n="Perseus:abo:phi,0134,002:1:1:66">
                        <author>Ter.</author>
					Heaut. 1, 1, 66</bibl>
                </cit>: <cit>
                    <quote lang="la">haud permultum a me aberit infortunium,</quote>
                    <bibl n="Perseus:abo:phi,0134,002:4:2:1">
                        <author>Ter.</author>
					Heaut. 4, 2, 1</bibl>
                </cit>; <bibl n="Perseus:abo:phi,0474,056:2:7">
                    <author>Cic.</author>
				Fam. 2, 7</bibl>.—With the <hi rend="ital">simple
			abl.</hi> for ab: <cit>
                    <quote lang="la">paulumque cum ejus villa abessemus,</quote>
                    <bibl>
                        <author>Cic.</author> Ac. 1, 1</bibl>
                </cit> Görenz; but, ab ejus villa, B. and K.; cf.: <cit>
                    <quote lang="la">nuptā abesse tuā,</quote>
                    <bibl>
                        <author>Ov.</author> R. Am. 774</bibl>
                </cit>.— With <hi rend="ital">inter</hi>: <cit>
                    <quote lang="la">nec longis inter se passibus absunt,</quote>
                    <bibl n="Perseus:abo:phi,0690,003:11:907">
                        <author>Verg.</author>
					A. 11, 907</bibl>
                </cit>.—With <hi rend="ital">prope</hi>, <hi rend="ital">propius</hi>, <hi rend="ital">proxime</hi>, to denote a short
			distance: <cit>
                    <quote lang="la">nunc nobis prope abest exitium,</quote>
                    <trans>
                        <tr>is not far from</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0119,003:2:3:8">
                        <author>Plaut.</author>
					Aul. 2, 3, 8</bibl>
                </cit>; <cit>
                    <quote lang="la">so with est: prope est a te Deus, tecum est,</quote>
                    <bibl n="Perseus:abo:phi,1014,001:Ep. 41">
                        <author>Sen.</author>
					Ep. 41</bibl>
                </cit>: <cit>
                    <quote lang="la">loca, quae a Brundisio propius absunt, quam tu,
					biduum,</quote>
                    <bibl n="Perseus:abo:phi,0474,057:8:14">
                        <author>Cic.</author>
					Att. 8, 14</bibl>
                </cit>: <cit>
                    <quote lang="la">quoniam abes propius,</quote>
                    <trans>
                        <tr>since you are nearer</tr>,</trans>
                    <bibl>
                        <author>id.</author> ib. 1, 1</bibl>
                </cit>: <cit>
                    <quote lang="la">existat aliquid, quod ... absit longissime a
					vero,</quote>
                    <bibl>
                        <author>id.</author> Ac. 2, 11, 36</bibl>
                </cit>; so <bibl n="Perseus:abo:phi,0474,034:chapter=13">
                    <author>id.</author> Deiot. 13</bibl>; Caes. ap. <bibl n="Perseus:abo:phi,0474,057:9:16 al">
                    <author>Cic.</author> Att.
				9, 16 al.</bibl>—Hence the phrase: tantum abest,
			ut—ut, <hi rend="ital">so far from</hi> —<hi rend="ital">that</hi>, etc. (Zumpt, § <cit>
                    <quote lang="la">779), the origin of which is evident from the
					following examples from Cic. (the first two of which have
					been unjustly assailed): id tantum abest ab officio, ut
					nihil magis officio possit esse contrarium, Off. 1, 14 (with
					which comp. the person. expression: equidem tantum absum ab
					ista sententia, ut non modo non arbitrer ... sed, etc.,</quote>
                    <bibl n="Perseus:abo:phi,0474,037:1:60:255">
                        <author>id.</author>
					de Or. 1, 60, 255</bibl>
                </cit>): <cit>
                    <quote lang="la">tantum abest ab eo, ut malum mors sit, ut
					verear, ne, etc.,</quote>
                    <bibl n="Perseus:abo:phi,0474,049:1:31:76">
                        <author>id.</author>
					Tusc. 1, 31, 76</bibl>
                </cit>: ego vero istos tantum abest ut ornem, ut effici non possit,
			quin eos oderim, <hi rend="ital">so far am I from</hi>—<hi rend="ital">that</hi>, <bibl n="Perseus:abo:phi,0474,035:11:14">
                    <author>id.</author> Phil. 11, 14</bibl>; sometimes <hi rend="ital">etiam</hi> or <hi rend="ital">quoque</hi> is added
			to the second clause, Lentul. ap. <bibl n="Perseus:abo:phi,0474,056:12:15:2">
                    <author>Cic.</author> Fam.
				12, 15, 2</bibl>; <bibl n="Perseus:abo:phi,1348,001:life=tib.:50">
                    <author>Suet.</author>
				Tib. 50</bibl>; more rarely <hi rend="ital">contra</hi>, <bibl n="Perseus:abo:phi,0914,001:6:31:4">
                    <author>Liv.</author> 6, 31,
				4</bibl>. Sometimes the second <hi rend="ital">ut</hi> is left
			out: <cit>
                    <quote lang="la">tantum afuit, ut inflammares nostros animos:
					somnum isto loco vix tenebamus,</quote>
                    <bibl n="Perseus:abo:phi,0474,039:80:278">
                        <author>Cic.</author>
					Brut. 80, 278</bibl>
                </cit>; on the contrary, once in Cic. with a third <hi rend="ital">ut</hi>: tantum abest ut nostra miremur, ut usque eo difficiles
			ac morosi simus, ut nobis non satisfaciat ipse Demosthenes, Or. 29,
			104.</sense>
            <sense id="n254.4" n="II" level="1"> Hence,
			</sense>
            <sense id="n254.5" n="A" level="2">
                <hi rend="ital">To be away from</hi> any thing unpleasant, <hi rend="ital">to be freed</hi> or <hi rend="ital">free from</hi>: <cit>
                    <quote lang="la">a multis et magnis molestiis abes,</quote>
                    <bibl n="Perseus:abo:phi,0474,056:4:3">
                        <author>Cic.</author>
					Fam. 4, 3</bibl>
                </cit>: <cit>
                    <quote lang="la">a culpa,</quote>
                    <bibl n="Perseus:abo:phi,0474,002:chapter=20">
                        <author>id.</author> Rosc. Am. 20</bibl>
                </cit>: a reprehensione temeritatis, Planc. ap. <bibl n="Perseus:abo:phi,0474,056:10:23">
                    <author>Cic.</author> Fam.
				10, 23</bibl>.</sense>
            <sense id="n254.6" n="B" level="2">
                <hi rend="ital">To be removed from</hi> a thing by will,
			inclination, etc.; <hi rend="ital">to be disinclined to</hi> (syn.
				<hi rend="ital">abhorreo</hi>)' a consilio fugiendi, <bibl n="Perseus:abo:phi,0474,057:7:24">
                    <author>Cic.</author> Att. 7,
				24</bibl>: <cit>
                    <quote lang="la">ab istis studiis,</quote>
                    <bibl n="Perseus:abo:phi,0474,028:chapter=25">
                        <author>id.</author> Planc. 25</bibl>
                </cit>: <cit>
                    <quote lang="la">ceteri a periculis aberant,</quote>
                    <trans>
                        <tr>kept aloof from</tr>, <tr>avoided</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0631,001:C. 6:3">
                        <author>Sall.</author>
					C. 6, 3</bibl>
                </cit>. toto aberant bello, <bibl n="Perseus:abo:phi,0448,001:7:63">
                    <author>Caes.</author> B. G. 7, 63</bibl>.</sense>
            <sense id="n254.7" n="C" level="2">
                <hi rend="ital">To be removed from a thing</hi> in regard to
			condition or quality, i. e. <hi rend="ital">to be different
			from</hi>, <hi rend="ital">to differ</hi> = abhorrere abest a tua
			virtute et fide, Brut. et Cass. ap. <bibl n="Perseus:abo:phi,0474,056:11:2">
                    <author>Cic.</author> Fam. 11,
				2</bibl>: istae <foreign lang="greek">κολακεῖαι</foreign> non
			longe absunt a scelere, <bibl n="Perseus:abo:phi,0474,057:13:30">
                    <author>id.</author> Att. 13, 30</bibl>: <cit>
                    <quote lang="la">haec non absunt a consuetudine somniorum,</quote>
                    <bibl>
                        <author>id.</author> Divin. 1, 21</bibl>
                </cit>, <pb n="13"/>
                <cb n="ABSU"/> 42.—Since improvement, as well as
			deterioration, may constitute the ground of difference, so absum
			may, according to its connection, designate the one or the other: <cit>
                    <quote lang="la">nullā re longius absumus a
					naturā ferarum,</quote>
                    <trans>
                        <tr>in nothing are we more elevated above the nature of
						the brute</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0474,055:1:16:50">
                        <author>Cic.</author>
					Off. 1, 16, 50</bibl>
                </cit>; <cit>
                    <quote lang="la">so also the much-contested passage,</quote>
                    <bibl n="Perseus:abo:phi,0474,028:7:17">
                        <author>Cic.</author>
					Planc. 7, 17</bibl>
                </cit>: longissime Plancius a te afuit, i. e. valde, plurimis
			suffragiis, te vicit, <hi rend="ital">was far from you in the number
				of votes</hi>, i. e. <hi rend="ital">had the majority;</hi> v.
			Wunder ad Planc. proleg. p. 83 sq.; on the other hand, <hi rend="ital">to be less</hi>, <hi rend="ital">inferior</hi>:
			longe te a pulchris abesse sensisti, Cic. Fragm. ap.
					<bibl>
                    <author>Non.</author> 339, 23</bibl>: <cit>
                    <quote lang="la">multum ab eis aberat L. Fufius,</quote>
                    <bibl>
                        <author>id.</author> Brut. 62, 222</bibl>
                </cit>; so <bibl n="Perseus:abo:phi,0893,006:370">
                    <author>Hor.</author> A. P. 370</bibl>.</sense>
            <sense id="n254.8" n="D" level="2">
                <hi rend="ital">Not to be suitable</hi>, <hi rend="ital">proper</hi>, or <hi rend="ital">fit for</hi> a thing: <cit>
                    <quote lang="la">quae absunt ab forensi contentione,</quote>
                    <bibl n="Perseus:abo:phi,0474,040:11:37">
                        <author>Cic.</author>
					Or. 11, 37</bibl>
                </cit>: <cit>
                    <quote lang="la">ab principis personā,</quote>
                    <bibl n="Perseus:abo:phi,0588,015:1:2">
                        <author>Nep.</author> Ep.
					1, 2</bibl>
                </cit>.</sense>
            <sense id="n254.9" n="E" level="2">
                <hi rend="ital">To be wanting</hi>, = desum, Pac. ap. <bibl n="Perseus:abo:phi,0474,048:5:11:31">
                    <author>Cic.</author> Fin.
				5, 11, 31</bibl> (Trag. Rel. p. 122 Rib.): <cit>
                    <quote lang="la">unum a praeturā tuā abest,</quote>
                    <trans>
                        <tr>one thing is wanting to your praetorship</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0119,009:1:1:25">
                        <author>Plaut.</author> Ep. 1, 1, 25</bibl>
                </cit>: quaeris id quod habes; <cit>
                    <quote lang="la">quod abest non quaeris,</quote>
                    <bibl n="Perseus:abo:phi,0134,002:5:4:16">
                        <author>Ter.</author>
					Heaut. 5, 4, 16</bibl>
                </cit>; cf. <bibl n="Perseus:abo:phi,0550,001:3:970">
                    <author>Lucr.</author> 3, 970</bibl> and 1095.—After
			Cicero, constr. in this signif. with <hi rend="ital">dat.</hi>: <cit>
                    <quote lang="la">quid huic abesse poterit de maximarum rerum
					scientiā?</quote>
                    <bibl n="Perseus:abo:phi,0474,037:1:11:48">
                        <author>Cic.</author>
					de Or. 1, 11, 48</bibl>
                </cit>: <cit>
                    <quote lang="la">abest enim historia litteris nostris,</quote>
                    <trans>
                        <tr>history is yet wanting to our literature</tr>,</trans>
                    <bibl n="Perseus:abo:phi,0474,044:2:5">
                        <author>id.</author> Leg.
					2, 5</bibl>
                </cit>.—So esp. in the poets: <cit>
                    <quote lang="la">donec virenti canities abest morosa,</quote>
                    <bibl n="Perseus:abo:phi,0893,001:1:9:17">
                        <author>Hor.</author>
					C. 1, 9, 17</bibl>
                </cit>; <bibl>3, 24, 64</bibl>; <bibl n="Perseus:abo:phi,0959,006:14:371">
                    <author>Ov.</author> M. 14,
				371</bibl>.—Hence the phrase non multum (neque
			multum), paulum, non (haud) procul, minimum, nihil abest, quin. <hi rend="ital">not much</hi>, <hi rend="ital">little</hi>, <hi rend="ital">nothing is wanting that</hi> (Zumpt, Gr. §
			540); but not parum, since <hi rend="ital">parum</hi> in good
			classical authors does not correspond in meaning with <hi rend="ital">non multum</hi>, but with <hi rend="ital">non
			satis</hi> (v. parum): <cit>
                    <quote lang="la">neque multum abesse ab eo, quin, etc.,</quote>
                    <bibl n="Perseus:abo:phi,0448,001:5:2:2">
                        <author>Caes.</author>
					B. G. 5, 2, 2</bibl>
                </cit>; and <hi rend="ital">absol.</hi>: <cit>
                    <quote lang="la">neque multum afuit quin,</quote>
                    <bibl n="Perseus:abo:phi,0448,002:2:35:4">
                        <author>id.</author>
					B. C. 2, 35, 4</bibl>
                </cit>: <cit>
                    <quote lang="la">paulumque afuit quin, ib. § 2: legatos
					nostros haud procul afuit quin violarent,</quote>
                    <bibl n="Perseus:abo:phi,0914,001:5:4">
                        <author>Liv.</author> 5,
					4 <hi rend="ital">fin.</hi>
                    </bibl>
                </cit>: <cit>
                    <quote lang="la">minimum afuit quin periret,</quote>
                    <trans>
                        <tr>was within a little of</tr>,</trans>
                    <bibl n="Perseus:abo:phi,1348,001:life=aug.:14">
                        <author>Suet.</author> Aug. 14</bibl>
                </cit>: <cit>
                    <quote lang="la">nihil afore credunt quin,</quote>
                    <bibl n="Perseus:abo:phi,0690,003:8:147 al">
                        <author>Verg.</author> A. 8, 147 al.</bibl>
                </cit>
            </sense>
            <sense id="n254.10" n="F" level="1"> Abesse alicui or
			ab aliquo, <hi rend="ital">to be wanting to</hi> any one, <hi rend="ital">to be of no assistance</hi> or <hi rend="ital">service to</hi> (opp. adsum): <cit>
                    <quote lang="la">ut mirari Torquatus desinat, me, qui Antonio
					afuerim, Sullam defendere,</quote>
                    <bibl n="Perseus:abo:phi,0474,015:chapter=5">
                        <author>Cic.</author> Sull. 5</bibl>
                </cit>: facile etiam absentibus nobis (<hi rend="ital">without our
				aid</hi>) veritas se ipsa defendet, <bibl>
                    <author>id.</author>
				Ac. 2, 11, 36</bibl>: <cit>
                    <quote lang="la">longe iis fraternum nomen populi Romani
					afuturum,</quote>
                    <bibl n="Perseus:abo:phi,0448,001:1:36">
                        <author>Caes.</author>
					B. G. 1, 36</bibl>
                </cit>. So also <bibl n="Perseus:abo:phi,0474,028:5:13">
                    <author>Cic.</author> Planc. 5, 13</bibl>: et quo plus
			intererat, eo plus aberas a me, <hi rend="ital">the more I needed
				your assistance</hi>, <hi rend="ital">the more you neglected
			me</hi>, v. Wunder ad h. l.; cf. also <bibl n="Perseus:abo:phi,0631,001:C. 20">
                    <author>Sall.</author> C. 20
					<hi rend="ital">fin.</hi>
                </bibl>
            </sense>
            <sense id="n254.11" n="G" level="1"> Cicero uses abesse to designate his banishment from
			Rome (which he would never acknowledge as such): <cit>
                    <quote lang="la">qui nullā lege abessem,</quote>
                    <bibl n="Perseus:abo:phi,0474,022:34:37">
                        <author>Cic.</author>
					Sest. 34, 37</bibl>
                </cit>; cf.: discessus. —Hence, <orth extent="full" lang="la">absens</orth>, <itype>entis</itype> (<hi rend="ital">gen. plur.</hi> regul. absentium; <cit>
                    <quote lang="la">absentum,</quote>
                    <bibl n="Perseus:abo:phi,0119,018:1:1:5">
                        <author>Plaut.</author>
					Stich. 1, 1, 5</bibl>
                </cit>), <pos>P. a.</pos>, <hi rend="ital">absent</hi> (opp.
			praesens). </sense>
            <sense id="n254.12" n="A" level="2"> In gen.: <cit>
                    <quote lang="la">vos et praesentem me curā levatis et
					absenti magna solatia dedistis,</quote>
                    <bibl n="Perseus:abo:phi,0474,039:3:11">
                        <author>Cic.</author>
					Brut. 3, 11</bibl>
                </cit>; so <bibl n="Perseus:abo:phi,0474,055:3:33:121">
                    <author>id.</author> Off. 3, 33, 121</bibl>; <bibl n="Perseus:abo:phi,0474,005:2:17">
                    <author>id.</author> Verr. 2,
				2, 17</bibl>: <cit>
                    <quote lang="la">quocirca (amici) et absentes adsunt et egentes
					abundant,</quote>
                    <bibl n="Perseus:abo:phi,0474,052:7:23">
                        <author>id.</author>
					Lael. 7, 23</bibl>
                </cit>: <cit>
                    <quote lang="la">ut loquerer tecum absens, cum coram id non
					licet,</quote>
                    <bibl n="Perseus:abo:phi,0474,057:7:15">
                        <author>id.</author>
					Att. 7, 15</bibl>
                </cit>: <cit>
                    <quote lang="la">me absente,</quote>
                    <bibl n="Perseus:abo:phi,0474,020:chapter=3">
                        <author>id.</author> Dom. 3</bibl>
                </cit>; <bibl n="Perseus:abo:phi,0474,024:chapter=50">
                    <author>id.</author> Cael. 50</bibl>: <cit>
                    <quote lang="la">illo absente,</quote>
                    <bibl n="Perseus:abo:phi,0474,006:chapter=17">
                        <author>id.</author> Tull. 17</bibl>
                </cit>; <bibl n="Perseus:abo:phi,0474,005:chapter=60">
                    <author>id.</author> Verr. 2, 60</bibl>: <cit>
                    <quote lang="la">absente accusatore,</quote>
                    <bibl>
                        <author>id.</author> ib. 2, 99</bibl>
                </cit> al.—<hi rend="ital">Sup.</hi>: <cit>
                    <quote lang="la">mente absentissimus,</quote>
                    <bibl n="Perseus:abo:phi,2468,001:Conf. 4:4">
                        <author>Aug.</author> Conf. 4, 4</bibl>
                </cit>.—Of things (not thus in Cic.): <cit>
                    <quote lang="la">Romae rus optas, absentem rusticus urbem tollis
					ad astra,</quote>
                    <bibl n="Perseus:abo:phi,0893,004:2:7:28">
                        <author>Hor.</author>
					S. 2, 7, 28</bibl>
                </cit>; so, <cit>
                    <quote lang="la">Rhodus,</quote>
                    <bibl>
                        <author>id.</author> Ep. 1, 11, 21</bibl>
                </cit>: <cit>
                    <quote lang="la">rogus,</quote>
                    <bibl n="Perseus:abo:phi,1294,001:9:77:8">
                        <author>Mart.</author>
					9, 77, 8</bibl>
                </cit>: <cit>
                    <quote lang="la">venti,</quote>
                    <bibl n="Perseus:abo:phi,1020,001:5:87">
                        <author>Stat.</author>
					Th. 5, 87</bibl>
                </cit>: <cit>
                    <quote lang="la">imagines rerum absentium,</quote>
                    <bibl n="Perseus:abo:phi,1002,001:6:2:29">
                        <author>Quint.</author> 6, 2, 29</bibl>
                </cit>: <cit>
                    <quote lang="la">versus,</quote>
                    <bibl n="Perseus:abo:phi,1254,001:20:10">
                        <author>Gell.</author>
					20, 10</bibl>
                </cit>.—</sense>
            <sense id="n254.13" n="B" level="2"> In
			partic. </sense>
            <sense id="n254.14" n="1" level="3"> In conversat.
			lang. </sense>
            <sense id="n254.15" n="(a)" level="5"> Praesens
			absens, <hi rend="ital">in one's presence or absence</hi>: <cit>
                    <quote lang="la">postulo ut mihi tua domus te praesente absente
					pateat,</quote>
                    <bibl n="Perseus:abo:phi,0134,003:5:8:29">
                        <author>Ter.</author>
					Eun. 5, 8, 29</bibl>
                </cit>.—</sense>
            <sense id="n254.16" n="(b)" level="5">
			Absente nobis turbatumst, <hi rend="ital">in our absence</hi> (so
			also: <cit>
                    <quote lang="la">praesente nobis, v. praesens),</quote>
                    <bibl n="Perseus:abo:phi,0134,003:4:3:7">
                        <author>Ter.</author>
					Eun. 4, 3, 7</bibl>
                </cit>; Afran. ap. <bibl>
                    <author>Non.</author> 76, 19</bibl> (Com.
			Rel. p. 165 Rib.).—</sense>
            <sense id="n254.17" n="2" level="3"> In polit. lang., <hi rend="ital">not appearing</hi> in
			public canvassings as a competitor: <cit>
                    <quote lang="la">deligere (Scipio) iterum consul absens,</quote>
                    <bibl n="Perseus:abo:phi,0474,043:6:11">
                        <author>Cic.</author>
					Rep. 6, 11</bibl>
                </cit>; so <bibl n="Perseus:abo:phi,0914,001:4:42:1">
                    <author>Liv.</author> 4, 42, 1</bibl>; <bibl n="Perseus:abo:phi,0914,001:10:22:9">10, 22,
			9</bibl>.—</sense>
            <sense id="n254.18" n="3" level="3"> =
			mortuus, <hi rend="ital">deceased</hi>, <bibl n="Perseus:abo:phi,0119,006:prol. 20">
                    <author>Plaut.</author>
				Cas. prol. 20</bibl>; <bibl n="Perseus:abo:phi,1056,001:7">
                    <author>Vitr.</author> 7</bibl>, praef. §
			8.—</sense>
            <sense id="n254.19" n="4" level="3"> Ellipt.:
			absens in Lucanis, <hi rend="ital">absent<cb n="ABSY"/> in
			Lucania</hi>, i. e. <hi rend="ital">absent and in Lucania</hi>,
				<bibl n="Perseus:abo:phi,0588,001:Hann. 5:3">
                    <author>Nep.</author> Hann. 5, 3</bibl>; so <bibl n="Perseus:abo:phi,0588,025:8:6">
                    <author>id.</author> Att. 8,
			6</bibl>.</sense>
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
