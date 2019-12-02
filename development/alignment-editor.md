# The Alpheios Alignment Editor 

Source repository: https://github.com/alpheios-project/alignment-editor

Dockerfile repository: https://github.com/alpheios-project/editors-docker

## Overview

The Alignment Editor is composed of the following parts:

* XHTML input forms and Javascript libraries provide the UI for creating and editing an aligment
* XQuery/XSLT provide transformation of the input/output 
* eXist serves the HTML, executes the XQuery and XSLT

It can store/retrieve the alignments created on it from the hosting eXist database, or it can store/retrieve 
data from an external repository that provides an Web API.

It does not itself support any user login functionality. When configured with an external data sources, it can use a session token stored 
in a cookie to a session-protected user data store. Otheriwse it all data is stored to shared files in the hosting eXist database. 

### Input Formats

When creating a new alignment, it can accept the following as input:

* plain text
* an XML document adhering to the [Alpheios Alignment schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/aligned-text.xsd)

If plain text input is supplied, it is tokenized  and transformed to to an XML document adhering to the [Alpheios Alignment schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/aligned-text.xsd).

### Editing Format

The Alignment XML is transformed to SVG for editing. The UI operates on the text as represented as a single SVG document. Editing takes place on the SVG document and not the underlying XML.

### Output Formats

Upon save, the alignment is transformed from SVG back to an XML document adhering to the [Alpheios Alignment schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/aligned-text.xsd) 
and stored in either the local eXist datastore or the external repository, depending upon the setup.

There are 2 options for exporting data:

* as an XML document adhering to the [Alpheios Alignment schema](https://github.com/alpheios-project/xml_ctl_files/blob/master/schemas/trunk/aligned-text.xsd)
* as an XHTML representation of the editing interface, in which the alignment data is represented as SVG, and a link 
to  external javascript and css files hosted on the eXist server that is hosting the editor.

### Contents of Aligned Data Files

The Alignment XML document contains only the individual words of the source text and the translation(s) and the data about 
how those words are aligned with each other. It can also contain comments on the word alignments. Depending upon how the 
alignment was created, it _may_ identify (e.g. with a link or other identifier) the original source of the text, but this is not
implicit in the schema and is entirely dependent upon the environment which is hosting the tool and the workflow it uses. 
It does not contain any information about how the text should be formatted for display, other than text direction.

## Uses of Alignment Editor Output

The Alpheios.net site currently demonstrates two different types of displays using data from the Alignment Editor

1. Enhanced Text Displays driven by TEI documents which had the Alignment Editor XML output merged back into the source TEI 
as attributes on word elements in the source TEI. These merged TEI documents were then transformed to HTML with alignment data contained
in attributes on HTML `span` elements containt the aligned words. 

2. Links to static exported XHTML displays (with the alignment data embedded as SVG referencing hosted javascript and css as described above)

3. Links to the Alignment Editor as hosted by the Perseids Project, in which the data is pulled directly from the Perseids 
repository and shown in read-only mode within the Alignment Editor interface (again, as SVG).

We do not currently have a up to date workflow which can be used at scale to produce the first type of display (enhanced text
displays from TEI XML).  When we produce the original Alpheios Enhanced Texts we used a sequence of XQuery scripts to merge 
Alignment and Treebank XML files back into a source TEI file. This process is fairly brittle, and if there is too much 
deviation between the TEI XML and the words as identified in the Alignment (and/or Treebank) XML, it fails.   Various 
attempts have made to create a repeatable workflow (e.g. see https://github.com/balmas/tei-digital-age) but none so far that
are scalable.

(Note: the Alpheios 3.0 code will provide mouse-over highlighting on alignments that are embedded in the HTML of an 
underlying document, where both the translation and the source text are in the same document and the aligned words are 
identified by CSS selectors supplied in the `data-alpheios_align_ref` attribute of a word. But there is currently no 
automated workflow to create such an HTML document from the Alignment Editor output).

## State of the code

The Alignment Editor was written in 2010 and the code has not been substantially updated since then.

The UI uses a custom modified version of 1.2.6 version of the JQuery javascript library and the now deprecated XHTML specification.

The XQuery code is version 1.0.

The XSLT translformations are version 1.0

The package itself has not been tested on any version of eXist later than 2.2.





