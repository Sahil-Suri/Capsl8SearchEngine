# Arweave Search Engine
We are developing a search engine, to better understand what happens when a user enters a query the below diagram shows the steps our engine takes to return results.

The engine uses Natural Language Processing (NLP) and the Okapi BM25 ranking function to produce relevant results.

![Blank diagram - Page 1 (1)](https://github.com/Sahil-Suri/Capsl8SearchEngine/assets/77424419/49696cde-2da6-4add-9610-1826f4d58d9a)


Once the user sends a query, it gets pre-processed through a process called query expansion. Here are the stages in greater detail:

## Tokenization
Tokenization removes the stop words and kstems known words to their root form. We are using Elasticsearch's built in "standard" tokenizer for this stage.

Technology used: We are using Elasticsearch's built in "standard" tokenizer for this stage

## NLP Engine
The first stage is the NLP engine which seeks to understand the query beyond what was entered by the user.

### Irregularity Detection
Irregularity detection seeks to find words that are not in the dictionary and correct potential misspellings. The query is then lemmatized, which is the process of grouping together the inflected forms of a word so they can be analysed as a single item. 

Technology used: We are using nspell (a Hunspell-like spell-checker w/o a tokenizer), a custom dictionary built from Wordnet, and POS for this stage.

### Semantic Analysis
Semantic Analysis seeks to understand the meaning of the query and find ways to expand the search to include synonyms and phrases the word may be a part of.

Technology used:  a custom dictionary built from Wordnet, and POS for this stage.

### Grammatical Analysis
Grammatical Analysis starts by tagging all the words with parts of speech tags, then an analysis is done to understand the subject, object and verb. The grammatical analysis is also used to find word pairs and phrases that are relevant to the query and better return results that understand the meaning of the query not just match the words of the query.

Technology used: We are using a POS parts of speach tagger and a custom dictionary built from Wordnet
## Special Signals
Special Signals are patterns that have been taught to the engine that can create different behaviors in the final results. One example of this is transaction IDs, which can indicate the user is looking for a specific transaction. Identifying these signals can allow the engine to find more relevant matches.

Technology used: Custom

## Keyword Detection
Keyword detection is the result of the NLP engine identifying keywords in previous queries. If a query matches a potential keyword, the keyword will be added to the expanded query and return higher quality results - this could also allow for keyword trends and adwords-like-keyword targeting to be built on top of our search engine.

Technology Used: Custom.

## Aspect Prioritization
Aspect prioritization takes phrases, subject, object, and verb identifications, keyword matches, synonyms and the original query to create a new query properly prioritizing these different elements.
Technology Used: Custom.

## Relevance + Ranking
The query is then sent to ElasticSearch which returns a list of documents that are relevant to the query using Okapi BM25 and the custom weights and logic embedded into the expanded query. The documents are then ranked and the top 10 documents are returned to the user.

Technology Used: Elasticsearch

# Indexing Arweave Transactions
We are currently running many GraphQl queries against the Goldsky GraphQL endpoint to pull transactions that are ANS-110 compliant. Due to the changes that have happened to ANS-110 over the year we are utilizing multiple configurations that ANS-110 could have taken to try and gather all of the relevant artifacts. Since this scraping started late into the development process we have not indexed the entirety of the ANS-110 compliant entries onchain and the sync will continue over the next few days. For each transaction we are attempting to parse the data onchain to extract text that could be used to produce search results, passing them through many of the same NLP processes that are done on the search engine side. We are also storing the raw data from the transaction onchain to allow for the user to view the original transaction and relevant tags.

# UDL
For each entry that gets stored into the index, we also check if there is any license tag attached to the content. If there is a license, we parse it to allow anyone to understand what licensing conditions are required to utilize a piece of data they found and would like to use. Since the UDL is new and we have not completed our sync with all ANS-110 compliant artifacts onchain, we have yet to identify any UDL licenses for the content we have indexed. We do expect this to change over the coming months as the license becomes more popular and our indexing efforts continue.
