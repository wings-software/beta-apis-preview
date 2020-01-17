Below is the  v1.0 of query schema for Audit Trail.

## Queries

```
extend type Query {
  # Get a list of audits. This returns paginated data.
  audits(filters: [ChangeSetFilter], limit: Int!, offset: Int): ChangeSetConnection @dataFetcher(name: changeSetConnection)
  # Get yaml diff for an audit (and a specific resource within the audit)
  auditChangeContent(filters: [ChangeContentFilter], limit: Int!, offset: Int): ChangeContentConnection  @dataFetcher(name: changeContentConnection)
}
```

## Schema

```
type ChangeContentList {
  data: [ChangeContent]
}

input ChangeContentFilter{
  changeSetId: String!
  resourceId: String
}

type ChangeSetConnection {
  pageInfo: PageInfo
  nodes: [ChangeSet]
}

type ChangeContentConnection {
  pageInfo: PageInfo
  nodes: [ChangeContent]
}

interface ChangeSet {
  id: String
  changes: [ChangeDetail]
  triggeredAt: DateTime
  request: RequestInfo
  failureStatusMsg: String
}

type UserChangeSet implements ChangeSet{
  id: String
  changes: [ChangeDetail]
  triggeredAt: DateTime
  request: RequestInfo
  failureStatusMsg: String
  triggeredBy: User
}

type GitChangeSet implements ChangeSet {
  id: String
  changes: [ChangeDetail]
  triggeredAt: DateTime
  request: RequestInfo
  failureStatusMsg: String
  author: String
  gitCommitId: String
  repoUrl: String
}

type RequestInfo {
  url: String
  resourcePath: String
  requestMethod: String
  responseStatusCode: Number
  remoteIpAddress: String
}

type ChangeDetail {
  resourceId: String
  resourceType: String
  resourceName: String
  operationType: String
  failure: Boolean
  appId: String
  appName: String
  parentResourceId: String
  parentResourceName: String
  parentResourceType: String
  parentResourceOperation: String
  createdAt: DateTime
}

type ChangeContent{
  changeSetId: String
  resourceId: String
  oldYaml: String
  oldYamlPath: String
  newYaml: String
  newYamlPath: String
}

""" Filters """

input ChangeSetFilter {
  time: TimeRangeFilter
}

# Filter by time
input TimeRangeFilter {
  # Filter within a specific time range
  specific: TimeRange
  # Filter for a relative time period
  relative: RelativeTimeRange
}

input TimeRange {
  from: DateTime!,
  to: DateTime
}

input RelativeTimeRange {
  timeUnit: TimeUnit!
  noOfUnits: Number!
}

enum TimeUnit {
  WEEKS,
  DAYS,
  HOURS,
  MINUTES
}
```
