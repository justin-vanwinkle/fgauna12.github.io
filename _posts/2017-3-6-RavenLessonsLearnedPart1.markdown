---
layout: post
title:  "RavenDB Lessons Learned"
date:   2017-03-006 21:52:14 -0500
categories: RavenDB
comments: true
---


## When you try to boil the ocean, bad things happen

### 128 - The out-of-the-box result limit on the client-side

Sometimes we get lazy, and we do something like this:

``` csharp

public class BirdRepository : IRepository<Bird>
{
	public IEnumerable<Bird> GetAll()
    {
    	return _documentSession.Query<Bird>().ToList();
    }
}

```

Besides the silliness of trying to use the repository pattern, we're trying to load *all* the documents into memory because that's how we laid out our architecture. Let's say that `IRepository<Bird>` forces us to implement a `GetAll()` method. 

Later in the testing process or even in production, you might notice that after 128 documents in your collection, you're always getting 128 results. This is because RavenDB is built to be **safe-by-default**, i.e. it won't let you boil the ocean and load all the documents into memory.

### 1024 - The out-of-the-box result limit on the server side

Similarly, if you try to by-pass the aformetioned limit by doing a `Take(1000)` or even worse `Take(int.Max)`, Raven will only give you 1024 documents because this is the default confirmation on the server side. 
In other less obvious circumstances, if you have a transformer and you're trying to group results, you will reach this limit as well.

### So what should you do then?

- Use [paging](https://ravendb.net/docs/article-page/3.5/csharp/indexes/querying/paging), it's really easy
- If you **have** to iterate through all the documents because you're exporting to a CSV file or building a report, you can use the [Streaming API](https://ravendb.net/docs/article-page/3.5/csharp/client-api/session/querying/how-to-stream-query-results). This API allows you to iterate through the whole collection one document at a time. 


## Web API - Identity Seperator

When first using RavenDB, one of the first challenges was the Web API routing and Raven IDs working nicely together.

For instance, when attempting to build a route to `GET` a resource by ID:

``` csharp
GET api/birds/{id}
```

You'll immediately notice the problem when the value of `{id}` is `birds/1`. The Web API router won't be able to find the matching controller action.

Your first instinct might be try to use `int` or `Guid` type ids in your raven documents. But resist this urge. There's a simpler way.



## Polymorphic Documents

The RavenDB [documentation](https://ravendb.net/docs/article-page/3.5/csharp/indexes/indexing-polymorphic-data) on this topic is quite helpful. There's a couple of strategies on how you can store polymorphic data in RavenDB.

Similar to how Entity Framework does it, you can store the entities seperately or together. 

```
documentStore.Conventions.IdentityPartsSeparator = "-";
```

Tada! Everything else should fall in place. No special code or side effects later on.

### Seperate Collections - Multi-Map Indexes


``` csharp

public class Birds_ByName : AbstractMultiMapIndexCreationTask
{
	public Birds_ByName()
	{
		AddMap<Owl>(owls => from o in owls select new { o.Name });

		AddMap<Eagle>(dogs => from e in eagles select new { e.Name });
	}
}

```

Then you can query all birds:

``` csharp

IList<Bird> results = session
	.Query<Bird, Birds_ByName>()
	.Where(x => x.Name == "Bald")
	.ToList();
    
```

### Same Collection - Overriding Default Tag Name

``` csharp
documentStore.Conventions.FindTypeTagName = type =>
{
  if (typeof(Bird).IsAssignableFrom(type))
  return "Birds";
  return DocumentConvention.DefaultTypeTagName(type);
}
```
In this manner, the documents will be stored as part of the same
collection. All queries and indexes will be against the new tagname. 

``` csharp
from animal in docs.Birds
select new { animal.Name }
``` 

## Large Document Collection? Read On

Won't happen in most applications, but if you have limited resources on your RavenDB host, but you expect a very large document collection (1 million+), you might consider not storing the index results in memory. 

You can toggle this feature right from the index definition.

``` csharp
public class Birds_ByName : AbstractIndexCreationTask<Bird>
{
  public Birds_ByName()
  {
    // Map Function Here

    DisableInMemoryIndexing = true;
  }
}
```