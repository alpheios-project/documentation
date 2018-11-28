# Design for Managing Offline Content

### Requirements Overview
The [Reader User Stories](https://github.com/alpheios-project/documentation/blob/master/design/reader-user-stories.csv) call various data and content served by the application to be available fully or partially while offline.

**App Level Content/Data**

 * text pages
 * morphology service data
 * treebank data 
 * short definition data 
 * full definition data 
 * inflection tables 
 * grammar pages

**User Level Content/Data**
 * word lists
 * commentary
 * bookmarks
 * text pages
 * + various other possibilities for the future including treebank data, images, sound files, etc.

Another way to look at this data is by use case:

* Text Retrieval
* Lexical Queries
* User Data Queries
  
## Current Strategies for Client Side Caching
Currently, the PWA prototype uses no pre-caching of this data and requests for text and lexical query data are cached via the CacheAPI on a per-request basis. The app javascript itself is the only thing that is currently pre-cached.

The Lexical Query calls upon a number of remote services to retrieve data. Some data is kept in memory as Javascript objects but no other client-side caching strategy is used.  There are some optimizations we could make to the existing code to reduce memory usage (for example, combining short definition and full definition index files, moving full definition index files to the server elminating the need for the full definition index, moving all definition index functionality to the server) but it makes sense to examine these in the context of the overall need for a strategy for offline content.

 
This [article](https://developers.google.com/web/fundamentals/instant-and-offline/web-storage/offline-for-pwa) recommends that we use the CacheAPI only for the static network resources needed to load the app (i.e. the core javascript, css, icons, and use IndexedDB for all dynamic data.

We also need to be aware that we cannot guarantee the amount of space avaliable for our offline content on the user's devices. Each browser imposes different restrictions. Chrome and Firefox allow a percentage of free space and Safari has a fixed limit of 50MB (these limits may be subject to change). So whatever solution we devise must take into account the need for caching partial datasets and all functionality must be able to have fallback strategies to retrieve data from online sources if it is missing in the offline cache.

## Data Sizes

* The lexicon short definition files range from .3MB to 7MB per lexicon.
* The lexicon full definition indeices range from .1BM to over 5MB per lexicon.
* The minified js which contains the full set of inflection table data is currently nearly 1MB

*Estimates still need to be prepared for all other data sources.*



