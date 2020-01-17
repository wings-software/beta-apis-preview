Below is the v1.0-alpha of query schema for Audit Trail.

## Queries

```
# Get difference in terms of YAML for a changeSet (and a specific resource within the changeSet).This returns paginated data.
auditChangeContent(
filters: [ChangeContentFilter]
limit: Int!
offset: Int
): ChangeContentConnection


# Get a list of changeSets.This returns paginated data.
audits(
  filters: [ChangeSetFilter]
  limit: Int!
  offset: Int
): ChangeSetConnection
```

## Schema

```
input ChangeContentFilter {
  # Unique ID of a changeSet
  changeSetId: String!
  
  # Unique ID of dependent or child resource, e.g.Environment, Services, etc.
  resourceId: String
}

type ChangeContentList {
  data: [ChangeContent]
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
  # List of all changeDetails
  changes: [ChangeDetails]

  # Failure message
  failureStatusMsg: String

  # Unique ID of a changeSet
  id: String

  # HTTP request that triggered the changeSet
  request: RequestInfo

  # Timestamp when changeSet was triggered
  triggeredAt: DateTime
}

type UserChangeSet implements ChangeSet{
  # List of all changeDetails
  changes: [ChangeDetails]

  # Failure message
  failureStatusMsg: String

  # Unique ID of a changeSet
  id: String

  # HTTP request that triggered the changeSet
  request: RequestInfo

  # Timestamp when changeSet was triggered
  triggeredAt: DateTime

  # User who triggered the changeSet
  triggeredBy: User
}

type GitChangeSet implements ChangeSet {
  # Git author who triggered the changeSet
  author: String

  # List of all changeDetails
  changes: [ChangeDetails]

  # Failure message
  failureStatusMsg: String

  # Git commit ID that triggered the changeSet
  gitCommitId: String

  # Unique ID of a changeSet
  id: String

  # Git repository URL that triggered the changeSet
  repoUrl: String

  # HTTP request that triggered the changeSet
  request: RequestInfo

  # Timestamp when changeSet was triggered
  triggeredAt: DateTime
}

type RequestInfo {
  # IP Address of request source
  remoteIpAddress: String

  # HTTP Request method
  requestMethod: String

  # Resource endpoint
  resourcePath: String

  # Response status code
  responseStatusCode: Number

  # Request URL
  url: String
}

type ChangeDetail {
  # Application ID
  appId: String

  # Application name
  appName: String

  # Timestamp of changeDetails creation
  createdAt: DateTime

  # Indicator of successful processing of the event that caused this changeSet
  failure: Boolean

  # Create / Update / Delete operation
  operationType: String

  # Unique ID of parent resource, e.g., Application
  parentResourceId: String
  
  # Parent resource name
  parentResourceName: String

  # Create / Update / Delete operation on parent resource
  parentResourceOperation: String

  # Parent resource type
  parentResourceType: String

  # Unique ID of dependent or child resource, e.g., Environment, Services, etc.
  resourceId: String

  # Resource name
  resourceName: String

  # Resource type
  resourceType: String
}

type ChangeContent{
  # Unique ID of a changeSet
  changeSetId: String
  
  # New YAML content after the changeSet got triggered
  newYaml: String

  # New YAML path after the changeSet got triggered
  newYamlPath: String

  # Old YAML content before the changeSet got triggered
  oldYaml: String

  # Old YAML path before the changeSet got triggered
  oldYamlPath: String

  # Unique ID of a resource, e.g.Application, Environment
  resourceId: String
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
