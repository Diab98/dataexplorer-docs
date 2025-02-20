---
title: Alter database cache policy command - Azure Data Explorer
description: "This article describes the .alter database cache policy command in Azure Data Explorer."
ms.reviewer: yonil
ms.topic: reference
ms.date: 11/29/2021
---
# .alter database cache policy

Change the database cache policy. To speed up queries on data, Azure Data Explorer caches data on its processing nodes, SSD, or even in RAM. With the [cache policy](cachepolicy.md), Azure Data Explorer can describe data artifacts so that important data can take priority.  

## Syntax

`.alter` `database` *DatabaseName* `policy` `caching` *PolicyParameter*

## Arguments

*DatabaseName* - Specify the name of the database.
*PolicyParameter* - Define one or more policy parameters. For parameters, see [cache policy](cachepolicy.md). 

## Returns

Returns a JSON representation of the policy.

## Example

The following example sets the caching policy to include the last 30 days.

```kusto
.alter database MyDatabase policy caching hot = 30d
```

**Output**

|PolicyName|EntityName|Policy|ChildEntities|EntityType|
|---|---|---|---|---|
|ClusterRequestClassificationPolicy| database1 |{"DataHotSpan": {"Value": "30.00:00:00"},"IndexHotSpan": { "Value": "30.00:00:00" }} | | |

### Define hot-cache windows

This command sets the caching policy to include the last 30 days and additional data from January and April 2021.

```kusto
.alter database MyDatabase policy caching 
        hot = 30d,
        hot_window = datetime(2021-01-01) .. datetime(2021-02-01),
        hot_window = datetime(2021-04-01) .. datetime(2021-05-01)
```

**Output**

|PolicyName|EntityName|Policy|ChildEntities|EntityType|
|---|---|---|---|---|
|CachingPolicy| |{"DataHotSpan": { "Value": "30.00:00:00" }, "IndexHotSpan": {    "Value": "30.00:00:00" },"HotWindows": [{ "MinValue": "2021-01-01T00:00:00Z", "MaxValue": "2021-02-01T00:00:00Z" }, { "MinValue": "2021-04-01T00:00:00Z", "MaxValue": "2021-05-01T00:00:00Z" }]}| |
