### <a href="https://ysu-csis-se.github.io/csci-5802-tooldemo-elasticsearch/">Back</a>

# Use Cases

The main use scenario that all of the use cases below branch off of is the need to have a full-text search engine on an HTTP web interface.

Throughout the scenarios below "documents" can be replaced by "any database collection".

| Use Case | Description  |
|---- | ---------- |
|Classical full text search | The most common use case for Elasticsearch is the "classical" full text search (website with an input box accompanied by a magnifying glass icon). Typically a user will have a "Big Data" amount of documents to be searched/queried. Elasticsearch handles "Big Data" searches with ease; particularly, simple searches, fuzzy searches, and aggregations during searches. Simple searches just require the top ten results for a regular, non-fuzzy match query. See the next two use cases for details on the other two searches.|
|Spell checker | Sometimes a search must be lenient toward spelling errors (a person's name, a website, etc.). Elasticsearch supports Fuzzy searches which allows searching with spelling errors. |
|Auto completer | Sometimes a user (Facebook, Google, Firefox, etc.) might require auto-completion in their search box (usually involving searching while a user types). Elasticsearch provides a lot of different features to assist in building advanced auto-completion searches. It also provides a family of suggesters. |
|Analytics | Along with Logstash and Kibana (Elasticsearch works easily with these two tools), Elasticsearch can generate and store visualizations and aggregations, particularly on time-series data. This use is common for social media platforms, such as Facebook. Machine learning capabilities are coming soon!|
|Multi-tenancy | Some users may have multiple customers or people with separate collections of documents where they should only be able to search the documents that belong to them. Elasticsearch allows efficient management of these separate collections. |
|General purpose document store | Sometimes a user may have a large number of documents that need to be stored with minimal management or search needs. Elasticsearch can house an extensive amount of documents as it is a distributed system.|
