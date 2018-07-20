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

1. Add Language model to data-models
