---
Title: RediSearch Quick Start Tutorial
description:
weight: $weight
alwaysopen: false
categories: ["RS"]
---
For this quick start, you will need the following:

- [A Redis Enterprise Software cluster with set up already
    complete]({{< relref "/rs/getting-started/quick-setup.md" >}})
- redis-cli

### Create a new database that uses the Module

1. In the Redis Modules field, click ![Add](/images/rs/icon_add.png#no-click "Add").
1. Select the RediSearch module.
1. If you want the module to use a custom configuration,
click **Add configuration** and enter the optional custom configuration.
1. Click ![Save](/images/rs/icon_save.png#no-click "Save").

    For example:
    {{< video "/images/rs/multiple-modules.mp4" "Adding multiple modules" >}}

1. Click the **Activate** button

## Creating Indexes

Let's create a new index called "myidx". When you define the index, you
must pass in the structure of the data you will be adding to the index.
In this example, we have four things we input, the title, body, url, and
value. In this example, we have three TEXT and one NUMERIC values. The
title has a weight of 5.0.

Connect to redis.

```src
$ redis-cli -p 12543
127.0.0.1:12543>
```

Run this command:

```src
127.0.0.1:12543> FT.CREATE myIdx SCHEMA title TEXT WEIGHT 5.0 body TEXT url TEXT value NUMERIC
```

### Add info to test index

Now add some data to this index. We will add an object which key will be
doc1 and then adds a title of "hello world", body of "my favorite
object", and url of [https://redislabs.com/](https://redislabs.com/) to the object as follows:

```src
127.0.0.1:12543> FT.ADD myIdx doc1 1.0 FIELDS title "hello world" body "My first object" url "https://redislabs.com/"
OK
```

### Search the Index

Do a search on this index for any object with the word "first":

```src
127.0.0.1:12543> FT.SEARCH myIdx "first" LIMIT 0 10
1) (integer) 1
2) "doc1"
3) 1) "title"
2) "hello world"
3) "body"
4) "My first object"
5) "url"
6) "https://redislabs.com/"
```

### Drop the Index

Now that we are done with it, we can drop the index.

```src
127.0.0.1:12543> FT.DROP myIdx
OK
```

### Auto-Complete and Search Engine Suggestions

Let's add a suggestion for the search engine to use

```src
127.0.0.1:12543> FT.SUGADD autocomplete "hello world" 100
"(integer)" 1
```

Make sure the suggestion is there:

```src
127.0.0.1:12543> FT.SUGGET autocomplete "he"
1) "hello world"
```
