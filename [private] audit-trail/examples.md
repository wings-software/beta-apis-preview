## Query to fetch audit trails, with no filters

```
{
  audits(limit:5){
    nodes{
      id
      triggeredAt
      request{
        url
        resourcePath
        requestMethod
        remoteIpAddress
        requestMethod
      }
      changes{
        appId
        appName
        operationType
      }
    }
    pageInfo{
      hasMore
      limit
      total
      offset
    }
  }
}
```

## Query to fetch audit trails, with specific time range filter

```
{
  audits(
    filters:{
      time: {
        specific:{
          from:1577567777550
          to: 1577567829417
        }
      }
    }
    limit:20){
    nodes{
      id
      triggeredAt
      request{
        url
        resourcePath
        requestMethod
        remoteIpAddress
        requestMethod
      }
      changes{
        appId
        
        resourceId
        appName
        operationType
      }
    }
    pageInfo{
      hasMore
      limit
      total
      offset
    }
  }
}
```

## Query to fetch audit trails, with relative time range filter 

```
{
  audits(
    filters:{
      time: {
        relative:{
          timeUnit:WEEKS
          noOfUnits: 2
        }
      }
    }
    limit:20){
    nodes{
      id
      triggeredAt
      request{
        url
        resourcePath
        requestMethod
        remoteIpAddress
        requestMethod
      }
      changes{
        appId
        
        resourceId
        appName
        operationType
      }
    }
    pageInfo{
      hasMore
      limit
      total
      offset
    }
  }
}
```

## Query to fetch audit trails, with resources filter 

```
{
  audits(
    filters:{
     resources:[APPLICATION, PIPELINE]
    }
    limit:20){
    nodes{
      id
      triggeredAt
      request{
        url
        resourcePath
        requestMethod
        remoteIpAddress
        requestMethod
      }
      changes{
        appId
        resourceType
        resourceId
        appName
        operationType
      }
    }
    pageInfo{
      hasMore
      limit
      total
      offset
    }
  }
}
```

## Query to fetch yaml diff, for a given changeSetId

```
{
  auditChangeContent(filters:{
    changeSetId: "fwtwORSKRe20lpbaLolJcg"
  }, limit: 20, offset: 0){
    nodes{
      resourceId
      changeSetId
      oldYaml
      newYaml
    }
  }
}
```

## Query to fetch yaml diff, for a given changeSetId and a resourceId

```
{
  auditChangeContent(filters:{
    changeSetId: "bW5RoOEYSGq0__49UUXSJw"
    resourceId:"2xaD-GJbQcC4DcabKgWcJA"
  }, limit: 1){
    nodes{
      resourceId
      changeSetId
      oldYaml
      newYaml
    }
  }
}
```

## Combined queries

```
{
  auditChangeContent(filters:{
    changeSetId: "fwtwORSKRe20lpbaLolJcg"
  }, limit: 20, offset: 0){
    nodes{
      resourceId
      changeSetId
      oldYaml
      newYaml
    }
  }
  
  audits(limit:100){
    nodes{
      id
      triggeredAt
      request{
        url
        resourcePath
        requestMethod
        remoteIpAddress
        requestMethod
      }
      changes{
        appId
        appName
        operationType
      }
    }
    pageInfo{
      hasMore
      limit
      total
      offset
    }
  }
}
```