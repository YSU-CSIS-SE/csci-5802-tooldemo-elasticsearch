### <a href="https://ysu-csis-se.github.io/csci-5802-tooldemo-elasticsearch/">Back</a>

# Tutorials using Elasticsearch

## First some key concepts

**Node** - A single running instance of Elasticsearch.

**Cluster** - A collection of one or more nodes (can index and search across all nodes in a cluster).

**Index** - A collection of different types of documents and document properties.

**Type/Mapping** - A collection of documents sharing a set of common fields present in the same index.

**Document** - A collection of fields in a specific manner defined in JSON format (belongs to a type inside an index with a unique UID).

**Shard** - Horizontally subdivided indexes in a node (contains all the properties of a document with less JSON objects than an index).

**Replicas** - Copy of an index or shard (allows for parallel searches and improved redundancy).

## Video Tutorial
A video tutorial walking through the following tutorials can be found on [Youtube](https://www.youtube.com/watch?v=052AIXklNHU&feature=youtu.be) as well as embedded below.

<iframe width="560" height="315" src="https://www.youtube.com/embed/052AIXklNHU" frameborder="0" allowfullscreen></iframe>


## Elasticsearch Index
Let's add some data to our Elasticsearch server. To index a first JSON object we make a POST request to the REST API: `http://localhost:9200/<index>/<type>/[<id>]`.

Index and type are required while the id part is optional If there isn't an index with that name on the server already one will be created using default configuration. By default an index will be created with 5 shards and 1 replica for each primary shard.

```

 PUT games
 {
   "settings" : {
     "index" : {
       "number_of_shards" : 4,
       "number_of_replicas" : 3
     }
   }
 }
```

The above command would create an index called games with 4 shards and 3 replicas of the primary shards. These shards allow you to horizontally split/scale your current volume and distribute and parallelize operations across shards for increased performance and throughput.

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

### Match & Filter
The **match** query is of type boolean. It means that the text provided is analyzed and the analysis process constructs a boolean query from the provided text. It also matches a text or phrase with the values of one or more fields. For example,

```
{
  "query": {
        "match": {
          "title": "kill"
        }
    }
}
```
finds all of the movies with title containing the string "kill". We will not spend more time talking about matching, because **match** is most useful when paired with filtering and fuzzy searches.

For filtering let us add another JSON object to our Elasticsearch server to make it more interesting to **match** and **filter**.
```
{
    "title": "Kill Bill: Vol. 1",
    "director": "Quentin Tarantino",
    "year": 2003,
    "genres": ["Action", "Crime", "Thriller"]
}
```

Now when we find all of the movies with title containing the string "kill" there will be two JSON objects returned. However, what do we do if we wanted to filter out those movies with year not equal to 1962? Well, we can use **match**, **filter**, and **bool** in the following way (you must do filtering this way, **filtered** queries have been deprecated).

```
POST http://localhost:9200/movies/movie/_search
{
  "query": {
    "bool": {
      "must": {
        "match": {
          "title": "kill"
        }
      },
      "filter": {
        "term": {
          "year": "1962"
        }
      }
    }
  }
}

```

As you can see below, because we filtered by the year 1962 we did not get the "Kill Bill: Vol. 1" movie back as a JSON object.

```
{
  "took":64,
  "timed_out":false,
  "_shards":{
    "total":5,
    "successful":5,
    "failed":0
  },
  "hits":{
    "total":1,
    "max_score":0.6099695,
    "hits":[{
      "_index":"movies",
      "_type":"movie",
      "_id":"4",
      "_score":0.6099695,
      "_source":{
        "title": "To Kill a Mockingbird",
        "director": "Robert Mulligan",
        "year": 1962
      }
    }]
  }
}
```

### Fuzzy Searches
If you anticipate human error in your searches, such as misspellings, then fuzzy searches are for you. Elasticsearch supports many different fuzzy requests and fuzzy JSON syntax. However, the most versatile way to make a fuzzy search request can be found below using **match** and the **fuzziness** option (which must be present for the fuzzy search to work). For more information about fuzzy searching and other acceptable fuzzy search request syntax check out [https://www.elastic.co/blog/found-fuzzy-search](https://www.elastic.co/blog/found-fuzzy-search).

```
POST http://localhost:9200/movies/movie/_search
{
  "query": {
    "match": {
      "title": {
        "query": "kiilll",
        "fuzziness": 2
      }
    }
  }
}
```

In the above request, we are searching our data for JSON objects with titles that contain a string very close to the string "kiilll". Below you can see the response from this request, which is as we would expect.

```
{
  "took":333,
  "timed_out":false,
  "_shards":{
    "total":5,
    "successful":5,
    "failed":0
  },
  "hits":{
    "total":2,
    "max_score":0.30498475,
    "hits":[{
      "_index":"movies",
      "_type":"movie",
      "_id":"4",
      "_score":0.30498475,
      "_source":{
        "title": "To Kill a Mockingbird",
        "director": "Robert Mulligan",
        "year": 1962
      }
    },
    {
      "_index":"movies",
      "_type":"movie",
      "_id":"5",
      "_score":0.14384104,
      "_source":{
        "title": "Kill Bill: Vol. 1",
        "director": "Quentin Tarantino",
        "year": 2003,
        "genres": ["Action", "Crime", "Thriller"]
      }
    }]
  }
}
```

There are also many other options you may supply to your fuzzy search as seen below.

```
{
    "value": "kiilll",
    "boost": 1.0,
    "fuzziness": 2, //maximum edit distance
    "prefix_length": 0, //number of initial characters which will not be fuzzied
    "max_expansions": 100 //maximum number of terms that the fuzzy query will expand to
}
```

These mini-tutorials are not comprehensive. In fact, Elasticsearch has many many more JSON APIs found [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html). Also, for more comprehensive tutorials on Elasticsearch see [http://joelabrahamsson.com/elasticsearch-101/](http://joelabrahamsson.com/elasticsearch-101/) and [https://www.tutorialspoint.com/elasticsearch/index.htm](https://www.tutorialspoint.com/elasticsearch/index.htm).
