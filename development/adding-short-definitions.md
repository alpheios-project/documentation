## Instructions for adding a Short Definitions index to Alpheios

### Step 1. Create a short definitions file

An example file is here:

https://raw.githubusercontent.com/alpheios-project/mjm/master/dat/grc-mjm-defs.dat

the format is

`<lemma>|<short definition>|<source>`

If the lemma is a proper noun, it should have 2 entries:

```
<lower case lemma>|@`
@<initial caps lemma>|<short definition>|<source>
```

### Step 2. Host the short defintions file somewhere

The simplest approach is to host the file from a GitHub repository. 

Alpheios can be configured to retrieve the index directly from GitHub, e.g. as in this url for the Bailly Short Definitions https://raw.githubusercontent.com/DigiClass/alpheios-french-dictionary/master/data/bailly-grc-defs.dat

And if Alpheios decides to host the file, it can then be easily added to our deployment process for short definitions by adding it to our our puppet configuration file at  https://github.com/alpheios-project/puppet/blob/master/site-modules/profile/manifests/lexdata.pp

Follow any example in that file to add a new repo for short definitions -- it will then be automatically deployed at repos1.alpheios.net/lexdata at the path specified. 
https://repos1.alpheios.net/lexdata serves the files at `/usr/local/lexdata` on the server managed by this puppet file.

E.g. this is the entry for the Middle Liddell short definitions

```
   vcsrepo { '/usr/local/lexdata/v2/ml':
     ensure   => latest,
     revision => 'master',
     provider => git,
     source   => 'https://github.com/alpheios-project/ml.git'
   }
```

They are deployed at https://repos1.alpheios.net/lexdata/v2/mjm/dat/grc-mjm-defs.dat

* NOTE: currently the puppet files are applied via cron job once per week on Friday evenings. This schedule can be changed if needed. 

### Step 3. Add the dictionary to the alpheios-config-api configuration file

The configuration file is at https://github.com/alpheios-project/alpheios-config-api/blob/master/config/config.json

Add an entry to the `lexicons` object that matches the format for the other short definitions files. E.g. here's the entry for the Major Core Middle Liddle index:

```
            "https://github.com/alpheios-project/mjm": {
              "urls": {
                "short": "https://repos1.alpheios.net/lexdata/v2/mjm/dat/grc-mjm-defs.dat"
              },
              "langs": {
                "source": "grc",
                "target": "en"
              },
              "format": { "short" :"text/html" },
              "description": "Definitions derived from Wilfred E. Major's Core Greek Vocabulary, extended with definitions from the Middle Liddell and Logeion.",
              "rights_keys": {
                 "ML": "\"An Intermediate Greek-English Lexicon\" (Henry George Liddell, Robert Scott). Provided by the Perseus Digital Library at Tufts University. Edits and additions provided by Vanessa Gorman, University of Nebraska and Logeion.",
                 "Major": "Wilfred E. Major, Core Greek Vocabulary for the First Two Years of Greek. CPL Online, Winter 2008. Edits and additions provided by Vanessa Gorman, University of Nebraska.",
                 "ML,Logeion": "\"An Intermediate Greek-English Lexicon\" (Henry George Liddell, Robert Scott). Provided by the Perseus Digital Library at Tufts University. Edits and additions provided by Logeion.",
                 "Logeion": "Logeion (https://logeion.uchicago.edu/), Helma Dik, University of Chicago."
              }
            },
```

the keys in the `rights_keys` object should match the abbreviations used for the source field in the short defintions file, and the values should be the full description of the source. This is what will appear in the Credits link of the Alpheios popup.

To deploy the configuration change following the instructions in https://github.com/alpheios-project/alpheios-config-api#readme

Please deploy and test the file first in the DEV environment per the Deployment Instructions there. 

This will deploy the file at config-dev.alpheios.net instead of config.alpheios.net.  You can use a local build of the Alpheios embedded library by changing the value of the`configServiceUrl` setting at https://github.com/alpheios-project/embed-lib/blob/5268f94d2fa5878f20ac7d41b616e96745fc7418/src/embedded.js#L172


### Step 4. Add the ability to select the dictionary to Alpheios 

This is the only part that requires a new build of Alpheios to deploy new short definitions. 

In order for a user to select a short definitions dictionary in Alpheios, it needs to be added to https://github.com/alpheios-project/alpheios-core/blob/master/packages/components/src/settings/language-options-defaults.json

Ideally, these values should come from the config api rather than the hard-coded local settings. 

