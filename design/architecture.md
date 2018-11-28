# Alpheios Architectural Overview

## Alpheios Reading Environment 

### Applications

#### Browser Extensions
The Browser Extensions for Chrome, Safari and Firefox which work with any text anywhere on the desktop only. The [webextension](https://github.com/alpheios-project/webextension) repository contains the code enables Alpheios 
to act as a browser extension. It relies on the Alpheios modules for most Alpheios functionality, and itself contains the code for embedding itself in the browser toolbar, menus and content pages.

#### Alpheios Embedded Library

The Embedded library allows text providers to enable Alpheios functionality to be embedded directly in their pages without requiring users to install the Alpheios browser extension to access it.

The [embed-lib](https://github.com/alpheios-project/embed-lib) repository contains the code for embedding the Alpheios
library in a specific HTML page.  It relies on the Alpheios modules for most Alpheios functionality, and itself 
contains the API code for embedding Alpheios in any HTML page.

#### Alpheios Reader Application
The Alpheios Reader Application will be an Alpheios-hosted Reading Environment implemented as a Progressive Web Application.

Various prototypes for this are underway.  The [pwa-prototype](https://github.com/alpheios-project/pwa-prototype) repository contains a prototype prototype mobile UI served as a progressive web application, relying on the Alpheios modules for core Alpheios functionality. The [alpheios-nemo-ui](https://github.com/alpheios-project/alpheios_nemo_ui) is a Python Flask Application which serves text content. It uses the Alpheios embedded library to provide Alpheios core functionality.  

### User Stories 

![Use Case Diagram](app-use-cases.svg)

***(Open the diagram in a new tab to activate the links to the user stories)***

### Client Side Libraries (Javascript)

These modules are dependencies for the higher level Alpheios products. Each of these modules contains various dependencies on 
3rd party libraries. 

#### Data Models
[alpheios-data-models](https://github.com/alpheios-project/data-models) contains core data model objects

#### Components
[alpheios-components](https://github.com/alpheios-project/components) contains ui components and controllers and supporting libraries . The currently separate [alpheios-inflection-tables](https://github.com/alpheios-project/inflection-tables) and [alpheios-experience](https://github.com/alpheios-project/experience) repos should be merged into this one.

#### Client Adapters
[alpheios-client-adapters](https://github.com/alpheios-project/client-adapters) contains client adapter libraries for webservice interactions (work in progress, it's an aggregation of previously separate adapter repositories, [alpheios-lexicon-client](https://github.com/alpheios-project/lexicon-client), [alpheios-morph-client](https://github.com/alpheios-project/morph-client), [alpheios-res-client](https://github.com/alpheios-project/res-client), [alpheios-lemma-client](https://github.com/alpheios-project/lemma-client) 

#### Build Tools
[alpheios-node-build](https://github.com/alpheios-project/node-build) is a set of common libraries for building and optimizing the code


### Web Services

#### Morphology Services

#### Lexicon Services

#### Grammars

### Other Supporting Code

#### CapiTainS

...

#### Gardener and Arethusa

...

### Auxiliary Modules

#### Inflection Games

#### Lexical Tests

## Alpheios Annotation Tools

### Alignment Editor

### Arethusa

