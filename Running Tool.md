# How to Run Elasticsearch Draft
<!-- how to setup, run, and use the tool -->
Before using Elasticsearch you must understand a few key concepts/words:

**Node** - A single running instance of Elasticsearch.

**Cluster** - A collection of one or more nodes (can index and search across all nodes in a cluster).

**Index** - A collection of different types of documents and document properties.

**Type/Mapping** - A collection of documents sharing a set of common fields present in the same index.

**Document** - A collection of fields in a specific manner defined in JSON format (belongs to a type inside an index with a unique UID).

**Shard** - Horizontally subdivided indexes in a node (contains all the properties of a document with less JSON objects than an index).

**Replicas** - Copy of an index or shard (allows for parallel searches and improved redundancy).


## Getting Started with Elasticsearch
The steps to setup and run Elasticsearch in Windows are as follows (for setup steps on Linux & UNIX machines see [https://www.tutorialspoint.com/elasticsearch/elasticsearch_installation.htm](https://www.tutorialspoint.com/elasticsearch/elasticsearch_installation.htm)):

1. Install/check for Java 7 or higher.
2. Download the Windows .zip file for [Elasticsearch](https://www.elastic.co/downloads/elasticsearch).
3. Unzip the .zip file to an easy to find path and Elasticsearch is installed there.
4. Open a command prompt and go to the Elasticsearch bin folder using `cd <filepath>`.
5. Run **elasticsearch.bat** using `elasticsearch`. You may come across two errors: "could not reserve enough space for <number>KB object heap" or "JAVA_HOME is not set". To resolve these errors respectively add the following variables to your system path:

  ![Java Options](/images/javaoptions.png)

  ![Java Home](/images/javahome.png)

6. You should see a lot of output in your command prompt and it should not terminate. Your Elasticsearch web interface and server is now up and running. Browse to **http://localhost:9200** to check if the server is running. It should return a JSON object with information about the installed Elasticsearch.

  ```
  {
    "name" : "MU4GP0a",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "8cDIy1QORDeQ_QKFPlVUQQ",
    "version" : {
      "number" : "5.3.0",
      "build_hash" : "3adb13b",
      "build_date" : "2017-03-23T03:31:50.652Z",
      "build_snapshot" : false,
      "lucene_version" : "6.4.1"
    },
    "tagline" : "You Know, for Search"
  }
  ```

7. Move to the next section to learn how to use some useful API features and to learn about all of the APIs and features Elasticsearch has to offer.

## APIs and Their Uses