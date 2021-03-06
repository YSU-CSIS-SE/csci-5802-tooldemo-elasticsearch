### <a href="https://ysu-csis-se.github.io/csci-5802-tooldemo-elasticsearch/">Back</a>

# Getting Started with Elasticsearch
The steps to setup and run Elasticsearch in Windows are as follows (for setup steps on Linux & UNIX machines see [https://www.tutorialspoint.com/elasticsearch/elasticsearch_installation.htm](https://www.tutorialspoint.com/elasticsearch/elasticsearch_installation.htm)):

1. Install/check for Java 7 or higher.
2. Download the Windows .zip file for [Elasticsearch](https://www.elastic.co/downloads/elasticsearch).
3. Unzip the .zip file to an easy to find path and Elasticsearch is installed there.
4. Open a command prompt and go to the Elasticsearch bin folder using `cd <filepath>`.
5. Run **elasticsearch.bat** using `elasticsearch`. You may come across two errors: "could not reserve enough space for <number>KB object heap" or "JAVA_HOME is not set". To resolve these errors respectively add the following variables to your system path:

  ![Java Options](/images/javaoptions.png)

  ![Java Home](/images/javahome.png)

6. You should see a lot of output in your command prompt and it should not terminate. Your Elasticsearch web interface and server is now up and running. Browse to [**http://localhost:9200**](http://localhost:9200) to check if the server is running. It should return a JSON object with information about the installed Elasticsearch.

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
7. Now that Elasticsearch is up and running we can interact with its JSON based REST API using any HTTP client to talk to it (curl, [Sense](https://chrome.google.com/webstore/detail/sense-beta/lhjgkmllcaadmopgmanpapmpjgmfcfig?hl=en), [RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/)). We will use [Fiddler](https://www.telerik.com/download/fiddler) in the tutorials on the [Tutorials](tutorials.md) page.

8. Follow the Fiddler link above and download the .exe file to install Fiddler on Windows.

9. Launch the downloaded .exe file and follow the steps to install Fiddler to completion.

9. Once you have installed Fiddler, launch it.

  ![Fiddler Start](/images/fiddlerstart.png)

This would be a good time to head to the [Tutorials](tutorials.md) page to run some Elasticsearch HTTP request examples with Fiddler.
