---
editable: false
sourcePath: en/_api-ref-grpc/compute/v1/api-ref/grpc/SnapshotSchedule/listSnapshots.md
---

# Compute Cloud API, gRPC: SnapshotScheduleService.ListSnapshots {#ListSnapshots}

Retrieves the list of snapshots created by the specified snapshot schedule.

## gRPC request

**rpc ListSnapshots ([ListSnapshotScheduleSnapshotsRequest](#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsRequest)) returns ([ListSnapshotScheduleSnapshotsResponse](#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsResponse))**

## ListSnapshotScheduleSnapshotsRequest {#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsRequest}

```json
{
  "snapshotScheduleId": "string",
  "pageSize": "int64",
  "pageToken": "string"
}
```

#|
||Field | Description ||
|| snapshotScheduleId | **string**

ID of the snapshot schedule to list created snapshots for.

To get a snapshot schedule ID, make a [SnapshotScheduleService.List](/docs/compute/api-ref/grpc/SnapshotSchedule/list#List) request. ||
|| pageSize | **int64**

The maximum number of results per page to return. If the number of available
results is larger than `pageSize`, the service returns a [ListSnapshotScheduleOperationsResponse.nextPageToken](/docs/compute/api-ref/grpc/SnapshotSchedule/listOperations#yandex.cloud.compute.v1.ListSnapshotScheduleOperationsResponse)
that can be used to get the next page of results in subsequent list requests.

Default value: 100. ||
|| pageToken | **string**

Page token. To get the next page of results, set `pageToken` to the
[ListSnapshotScheduleOperationsResponse.nextPageToken](/docs/compute/api-ref/grpc/SnapshotSchedule/listOperations#yandex.cloud.compute.v1.ListSnapshotScheduleOperationsResponse) returned by a previous list request. ||
|#

## ListSnapshotScheduleSnapshotsResponse {#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsResponse}

```json
{
  "snapshots": [
    {
      "id": "string",
      "folderId": "string",
      "createdAt": "google.protobuf.Timestamp",
      "name": "string",
      "description": "string",
      "labels": "string",
      "storageSize": "int64",
      "diskSize": "int64",
      "productIds": [
        "string"
      ],
      "status": "Status",
      "sourceDiskId": "string",
      "hardwareGeneration": {
        // Includes only one of the fields `legacyFeatures`, `generation2Features`
        "legacyFeatures": {
          "pciTopology": "PCITopology"
        },
        "generation2Features": "Generation2HardwareFeatures"
        // end of the list of possible fields
      },
      "kmsKey": {
        "keyId": "string",
        "versionId": "string"
      }
    }
  ],
  "nextPageToken": "string"
}
```

#|
||Field | Description ||
|| snapshots[] | **[Snapshot](#yandex.cloud.compute.v1.Snapshot)**

List of snapshots created by the specified snapshot schedule. ||
|| nextPageToken | **string**

Token for getting the next page of the list. If the number of results is greater than
the specified [ListSnapshotScheduleSnapshotsRequest.pageSize](#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsRequest), use `next_page_token` as the value
for the [ListSnapshotScheduleSnapshotsRequest.pageToken](#yandex.cloud.compute.v1.ListSnapshotScheduleSnapshotsRequest) parameter in the next list request.

Each subsequent page will have its own `next_page_token` to continue paging through the results. ||
|#

## Snapshot {#yandex.cloud.compute.v1.Snapshot}

A Snapshot resource. For more information, see [Snapshots](/docs/compute/concepts/snapshot).

#|
||Field | Description ||
|| id | **string**

ID of the snapshot. ||
|| folderId | **string**

ID of the folder that the snapshot belongs to. ||
|| createdAt | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)** ||
|| name | **string**

Name of the snapshot. 1-63 characters long. ||
|| description | **string**

Description of the snapshot. 0-256 characters long. ||
|| labels | **string**

Resource labels as `key:value` pairs. Maximum of 64 per resource. ||
|| storageSize | **int64**

Size of the snapshot, specified in bytes. ||
|| diskSize | **int64**

Size of the disk when the snapshot was created, specified in bytes. ||
|| productIds[] | **string**

License IDs that indicate which licenses are attached to this resource.
License IDs are used to calculate additional charges for the use of the virtual machine.

The correct license ID is generated by the platform. IDs are inherited by new resources created from this resource.

If you know the license IDs, specify them when you create the image.
For example, if you create a disk image using a third-party utility and load it into Object Storage, the license IDs will be lost.
You can specify them in the [yandex.cloud.compute.v1.ImageService.Create](/docs/compute/api-ref/grpc/Image/create#Create) request. ||
|| status | enum **Status**

Current status of the snapshot.

- `STATUS_UNSPECIFIED`
- `CREATING`: Snapshot is being created.
- `READY`: Snapshot is ready to use.
- `ERROR`: Snapshot encountered a problem and cannot operate.
- `DELETING`: Snapshot is being deleted. ||
|| sourceDiskId | **string**

ID of the source disk used to create this snapshot. ||
|| hardwareGeneration | **[HardwareGeneration](#yandex.cloud.compute.v1.HardwareGeneration)**

If specified, forces the same HardwareGeneration features to be applied to the instance
created using this snapshot as source for the boot disk. Otherwise the current default will be used. ||
|| kmsKey | **[KMSKey](#yandex.cloud.compute.v1.KMSKey)**

Key encryption key info. ||
|#

## HardwareGeneration {#yandex.cloud.compute.v1.HardwareGeneration}

A set of features, specific to a particular Compute hardware generation.
They are not necessary supported by every host OS or distro, thus they are fixed to an image
and are applied to all instances created with it as their boot disk image.
These features significantly determine how the instance is created, thus cannot be changed after the fact.

#|
||Field | Description ||
|| legacyFeatures | **[LegacyHardwareFeatures](#yandex.cloud.compute.v1.LegacyHardwareFeatures)**

Includes only one of the fields `legacyFeatures`, `generation2Features`. ||
|| generation2Features | **[Generation2HardwareFeatures](#yandex.cloud.compute.v1.Generation2HardwareFeatures)**

Includes only one of the fields `legacyFeatures`, `generation2Features`. ||
|#

## LegacyHardwareFeatures {#yandex.cloud.compute.v1.LegacyHardwareFeatures}

A first hardware generation, by default compatible with all legacy images.
Allows switching to PCI_TOPOLOGY_V2 and back.

#|
||Field | Description ||
|| pciTopology | enum **PCITopology**

- `PCI_TOPOLOGY_UNSPECIFIED`
- `PCI_TOPOLOGY_V1`
- `PCI_TOPOLOGY_V2` ||
|#

## Generation2HardwareFeatures {#yandex.cloud.compute.v1.Generation2HardwareFeatures}

A second hardware generation, which by default assumes PCI_TOPOLOGY_V2
and UEFI boot (with UEFI related features).

#|
||Field | Description ||
|| Empty | > ||
|#

## KMSKey {#yandex.cloud.compute.v1.KMSKey}

#|
||Field | Description ||
|| keyId | **string**

ID of KMS symmetric key ||
|| versionId | **string**

Version of KMS symmetric key ||
|#