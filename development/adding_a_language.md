# Adding support for a new language

1. Add morphology engine plugin to morphsvc

    1. Alpheios-compliant Service

        1. Remote

            If the morphology engine already outputs its data in the Alpheios lexicon schema, you just need to 
            implement a derivation of the `AlpheiosRemoteEngine` class.and then add the following information to the 
            service config file:
            
            ```
            ENGINES_<ENGINE_NAME>_CNAME = "morphsvc.lib.engines.<EnginePackageName>.<EngineClassName>"
            PARSERS_<ENGINE_NAME>_URI = "<unique identifier for the service>"
            PARSERS_<ENGINE_NAME>_REMOTE_URL = "<URL at which the service is deployed>"
            PARSERS_<ENGINE_NAME>_RIGHTS = "Rights statement for the service"
            ```
            
            And update the `ENGINES` setting in the service config file to include the lower-case name of the new engine:
                        
            E.g. 
            
            ```
            ENGINES = "myparser"
            ENGINES_MYPARSER_CNAME = "morphsvc.lib.engines.MyParser.MyParser"
            PARSERS_MYPARSER_URI = "example.org:myparser.v1"
            PARSERS_MYPARSER_REMOTE_URL = "http://example.org/myparser/word/"
            PARSERS_MYPARSER_RIGHTS = "MyParser is provided under license x by provider y."
            ```

        1. Local

    1. Non-Alpheios compliant Service
    
1. Add morphology client adapter to morph-client

    1. create an `ImportData` adapter for the LanguageModel in the `tufts/engine` folder
    
    1. add the language code to adapter mapping to the `config.json`
    
    1. import the new `ImportData` adapter in the `AlpheiosTuftsAdapter` class and add it to the `engineMap` property

1. Add Language model to data-models

    1. Add language symbol and iso codes to the Constants class:
    ```
    export const LANG_<LanguageName> = Symbol(`<language name>`)
    export const STR_LANG_CODE_<ISO_CODE> = '<iso code>'
    ```
    
    1. Create a new derivation of the `LanguageModel class, named `<LanguageName>LanguageModel` 
    
    1. Add the new mappings from language code to class to `LanguageModelFactory#MODELS`
       
    1. Implement a unit test for the new LanguageModel class
    
    1. Export the new LanguagModel class in the `driver.js` for the package
    
