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
7. Now that Elasticsearch is up and running we can interact with its JSON based REST API using any HTTP client to talk to it (curl, [Sense](https://chrome.google.com/webstore/detail/sense-beta/lhjgkmllcaadmopgmanpapmpjgmfcfig?hl=en), [RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/)). We will use [Fiddler](https://www.telerik.com/download/fiddler).

8. Follow the Fiddler link above and download the .exe file to install Fiddler on Windows.

9. Launch the downloaded .exe file and follow the steps to install Fiddler to completion.

9. Once you have installed Fiddler, launch it.

  ![Fiddler Start](/images/fiddlerstart.png)

## Elasticsearch Index
Let's add some data to our Elasticsearch server. To index a first JSON object we make a POST request to the REST API: `http://localhost:9200/<index>/<type>/[<id>]`.

Index and type are required while the id part is optional If there isn't an index with that name on the server already one will be created using default configuration.

We index the following movie with the following Fiddler set up:

![Fiddler Index](/images/fiddlerindex.png)

Our index name is **movies**, type name is **movie**, and id is **1**. Press execute to make the request.

You may double click on the completed process on the left in order to view the request response data:

![Fiddler Index Response](/images/fiddlerindexresponse.png)

To delete any index, type, or id JSON object execute `DELETE http://localhost:9200/<index>/<type>/[<id>]` without a request body.

## Searching
Once we have data on our server we may query it using many different requests. We will focus on searching to give an idea of how querying works.

We only have 1 movie in our data, so let us make a few more `POST http://localhost:9200/movies/movie/[<id>]` calls with the following request bodies.

```
{
    "title": "Apocalypse Now",
    "director": "Francis Ford Coppola",
    "year": 1979
}

{
    "title": "Lawrence of Arabia",
    "director": "David Lean",
    "year": 1962
}

{
    "title": "To Kill a Mockingbird",
    "director": "Robert Mulligan",
    "year": 1962
}
```

You may also use bulk add: `POST http://localhost:9200/movies/_bulk` with a specialized request body.

In order to search with ElasticSearch we make requests using `POST http://localhost:9200/<index>/<type>/_search` where index and type are both optional (they change where we are searching across, all indices, all types in an index, or all ids of a certain type).

In order to make a more useful search request (instead of returning all indices, types, or ids) we also need to supply a request body with a query. The request body should be a JSON object which looks like

```
{
    "query": {
        //Query DSL here
    }
}
```

in which we can use ElasticSearch's query DSL (many different Query DSLs can be found in the Search APIs).

For example, we search for the word "kill" in all of our movie type documents using:

```
POST http://localhost:9200/movies/movie/_search

{
    "query": {
        "query_string": {
            "query": "kill"
        }
    }
}
```

Giving the following response:

```
{
  "took":31,
  "timed_out":false,
  "_shards":{
    "total":5,
    "successful":5,
    "failed":0
  },
  "hits":{
    "total":1,
    "max_score":0.66747504,
    "hits":[{
      "_index":"movies",
      "_type":"movie",
      "_id":"4",
      "_score":0.66747504,
      "_source":{
        "title": "To Kill a Mockingbird",
        "director": "Robert Mulligan",
        "year": 1962
      }
    }]
  }
}
```

## APIs and Their Uses
We've barely scratched the surface of the capabilities of Elasticsearch in this short tutorial. See the table below for a list of APIs and a short description of their capabilities.

| API  | Description |
|--------- | ----------------- |
| Index/Get API |  API for creating, updating, and retrieving JSON objects. |
| Search API | API for searching content in Elasticsearch. Either a user can search by sending a get request with query string as a parameter or a query in the message body of post request. Mainly all the search APIs are multi-index, multi-type (uses Query DSL). |
| Aggregations Framework/API | Framework to collect all the data selected by the search query. This framework consists of many building blocks, which help in building complex summaries of the data (avg, cardinality, and stats aggregations). |
| Cluster API | This API is used for getting information about and making changes to a cluster and its nodes. |

For more information and comprehensive tutorials for Elasticsearch see [http://joelabrahamsson.com/elasticsearch-101/](http://joelabrahamsson.com/elasticsearch-101/) and [https://www.tutorialspoint.com/elasticsearch/index.htm](https://www.tutorialspoint.com/elasticsearch/index.htm).
