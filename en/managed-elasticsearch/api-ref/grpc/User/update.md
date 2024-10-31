---
editable: false
sourcePath: en/_api-ref-grpc/mdb/elasticsearch/v1/api-ref/grpc/User/update.md
---

# Managed Service for Elasticsearch API, gRPC: UserService.Update {#Update}

Updates the specified user.

## gRPC request

**rpc Update ([UpdateUserRequest](#yandex.cloud.mdb.elasticsearch.v1.UpdateUserRequest)) returns ([operation.Operation](#yandex.cloud.operation.Operation))**

## UpdateUserRequest {#yandex.cloud.mdb.elasticsearch.v1.UpdateUserRequest}

```json
{
  "clusterId": "string",
  "userName": "string",
  "updateMask": "google.protobuf.FieldMask",
  "password": "string"
}
```

#|
||Field | Description ||
|| clusterId | **string**

Required field. ID of the Elasticsearch cluster the user belongs to.

To get the cluster ID, make a [ClusterService.List](/docs/managed-elasticsearch/api-ref/grpc/Cluster/list#List) request. ||
|| userName | **string**

Required field. Name of the user to be updated.

To get the name of the user, make a [UserService.List](/docs/managed-elasticsearch/api-ref/grpc/User/list#List) request. ||
|| updateMask | **[google.protobuf.FieldMask](https://developers.google.com/protocol-buffers/docs/reference/csharp/class/google/protobuf/well-known-types/field-mask)** ||
|| password | **string**

New password for the user. ||
|#

## operation.Operation {#yandex.cloud.operation.Operation}

```json
{
  "id": "string",
  "description": "string",
  "createdAt": "google.protobuf.Timestamp",
  "createdBy": "string",
  "modifiedAt": "google.protobuf.Timestamp",
  "done": "bool",
  "metadata": {
    "clusterId": "string",
    "userName": "string"
  },
  // Includes only one of the fields `error`, `response`
  "error": "google.rpc.Status",
  "response": {
    "name": "string",
    "clusterId": "string"
  }
  // end of the list of possible fields
}
```

An Operation resource. For more information, see [Operation](/docs/api-design-guide/concepts/operation).

#|
||Field | Description ||
|| id | **string**

ID of the operation. ||
|| description | **string**

Description of the operation. 0-256 characters long. ||
|| createdAt | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp. ||
|| createdBy | **string**

ID of the user or service account who initiated the operation. ||
|| modifiedAt | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

The time when the Operation resource was last modified. ||
|| done | **bool**

If the value is `false`, it means the operation is still in progress.
If `true`, the operation is completed, and either `error` or `response` is available. ||
|| metadata | **[UpdateUserMetadata](#yandex.cloud.mdb.elasticsearch.v1.UpdateUserMetadata)**

Service-specific metadata associated with the operation.
It typically contains the ID of the target resource that the operation is performed on.
Any method that returns a long-running operation should document the metadata type, if any. ||
|| error | **[google.rpc.Status](https://cloud.google.com/tasks/docs/reference/rpc/google.rpc#status)**

The error result of the operation in case of failure or cancellation.

Includes only one of the fields `error`, `response`.

The operation result.
If `done == false` and there was no failure detected, neither `error` nor `response` is set.
If `done == false` and there was a failure detected, `error` is set.
If `done == true`, exactly one of `error` or `response` is set. ||
|| response | **[User](#yandex.cloud.mdb.elasticsearch.v1.User)**

The normal response of the operation in case of success.
If the original method returns no data on success, such as Delete,
the response is [google.protobuf.Empty](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Empty).
If the original method is the standard Create/Update,
the response should be the target resource of the operation.
Any method that returns a long-running operation should document the response type, if any.

Includes only one of the fields `error`, `response`.

The operation result.
If `done == false` and there was no failure detected, neither `error` nor `response` is set.
If `done == false` and there was a failure detected, `error` is set.
If `done == true`, exactly one of `error` or `response` is set. ||
|#

## UpdateUserMetadata {#yandex.cloud.mdb.elasticsearch.v1.UpdateUserMetadata}

#|
||Field | Description ||
|| clusterId | **string**

ID of the Elasticsearch cluster the user belongs to. ||
|| userName | **string**

Name of the user that is being updated. ||
|#

## User {#yandex.cloud.mdb.elasticsearch.v1.User}

An Elasticsearch user.

#|
||Field | Description ||
|| name | **string**

Name of the Elasticsearch user. ||
|| clusterId | **string**

ID of the Elasticsearch cluster the user belongs to. ||
|#