---
editable: false
sourcePath: en/_api-ref-grpc/mdb/mysql/v1/api-ref/grpc/ResourcePreset/list.md
---

# Managed Service for MySQL API, gRPC: ResourcePresetService.List {#List}

Retrieves the list of available resource presets.

## gRPC request

**rpc List ([ListResourcePresetsRequest](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsRequest)) returns ([ListResourcePresetsResponse](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsResponse))**

## ListResourcePresetsRequest {#yandex.cloud.mdb.mysql.v1.ListResourcePresetsRequest}

```json
{
  "pageSize": "int64",
  "pageToken": "string"
}
```

#|
||Field | Description ||
|| pageSize | **int64**

The maximum number of results per page to return.

If the number of available results is larger than `pageSize`, the API returns a [ListResourcePresetsResponse.nextPageToken](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsResponse) that can be used to get the next page of results in the subsequent [ResourcePresetService.List](#List) requests. ||
|| pageToken | **string**

Page token that can be used to iterate through multiple pages of results.

To get the next page of results, set `pageToken` to the [ListResourcePresetsResponse.nextPageToken](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsResponse) returned by the previous [ResourcePresetService.List](#List) request. ||
|#

## ListResourcePresetsResponse {#yandex.cloud.mdb.mysql.v1.ListResourcePresetsResponse}

```json
{
  "resourcePresets": [
    {
      "id": "string",
      "zoneIds": [
        "string"
      ],
      "cores": "int64",
      "memory": "int64"
    }
  ],
  "nextPageToken": "string"
}
```

#|
||Field | Description ||
|| resourcePresets[] | **[ResourcePreset](#yandex.cloud.mdb.mysql.v1.ResourcePreset)**

List of resource presets. ||
|| nextPageToken | **string**

The token that can be used to get the next page of results.

If the number of results is larger than [ListResourcePresetsRequest.pageSize](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsRequest), use the `nextPageToken` as the value for the [ListResourcePresetsRequest.pageToken](#yandex.cloud.mdb.mysql.v1.ListResourcePresetsRequest) in the subsequent [ResourcePresetService.List](#List) request to iterate through multiple pages of results.

Each of the subsequent [ResourcePresetService.List](#List) requests should use the `nextPageToken` value returned by the previous request to continue paging through the results. ||
|#

## ResourcePreset {#yandex.cloud.mdb.mysql.v1.ResourcePreset}

An object that represents MySQL resource preset.
A resource preset defines hardware configuration for cluster hosts.

See [the documentation](/docs/managed-mysql/concepts/instance-types) for details.

#|
||Field | Description ||
|| id | **string**

ID of the resource preset that defines available computational resources (vCPU, RAM, etc.) for a cluster host. ||
|| zoneIds[] | **string**

IDs of availability zones where the resource preset is available. ||
|| cores | **int64**

Number of CPU cores for a MySQL host created with the preset. ||
|| memory | **int64**

RAM volume for a MySQL host created with the preset, in bytes. ||
|#