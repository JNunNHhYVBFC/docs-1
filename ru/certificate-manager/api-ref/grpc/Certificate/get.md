---
editable: false
sourcePath: en/_api-ref-grpc/certificatemanager/v1/api-ref/grpc/Certificate/get.md
---

# Certificate Manager API, gRPC: CertificateService.Get

Returns the specified certificate.

To get the list of available certificates, make a [List](/docs/certificate-manager/api-ref/grpc/Certificate/list#List) request.

## gRPC request

**rpc Get ([GetCertificateRequest](#yandex.cloud.certificatemanager.v1.GetCertificateRequest)) returns ([Certificate](#yandex.cloud.certificatemanager.v1.Certificate))**

## GetCertificateRequest {#yandex.cloud.certificatemanager.v1.GetCertificateRequest}

```json
{
  "certificate_id": "string",
  "view": "CertificateView"
}
```

#|
||Field | Description ||
|| certificate_id | **string**

Required field. ID of the certificate to return.

To get the ID of a certificate use a [CertificateService.List](/docs/certificate-manager/api-ref/grpc/Certificate/list#List) request. ||
|| view | enum **CertificateView**

The output type of the certificate.

- `BASIC`: Output basic information about the certificate.
- `FULL`: Output full information about the certificate including domain challenges. ||
|#

## Certificate {#yandex.cloud.certificatemanager.v1.Certificate}

```json
{
  "id": "string",
  "folder_id": "string",
  "created_at": "google.protobuf.Timestamp",
  "name": "string",
  "description": "string",
  "labels": "map<string, string>",
  "type": "CertificateType",
  "domains": [
    "string"
  ],
  "status": "Status",
  "issuer": "string",
  "subject": "string",
  "serial": "string",
  "updated_at": "google.protobuf.Timestamp",
  "issued_at": "google.protobuf.Timestamp",
  "not_after": "google.protobuf.Timestamp",
  "not_before": "google.protobuf.Timestamp",
  "challenges": [
    {
      "domain": "string",
      "type": "ChallengeType",
      "created_at": "google.protobuf.Timestamp",
      "updated_at": "google.protobuf.Timestamp",
      "status": "Status",
      "message": "string",
      "error": "string",
      // Includes only one of the fields `dns_challenge`, `http_challenge`
      "dns_challenge": {
        "name": "string",
        "type": "string",
        "value": "string"
      },
      "http_challenge": {
        "url": "string",
        "content": "string"
      }
      // end of the list of possible fields
    }
  ],
  "deletion_protection": "bool",
  "incomplete_chain": "bool"
}
```

A certificate. For details about the concept, see [documentation](/docs/certificate-manager/concepts/).

#|
||Field | Description ||
|| id | **string**

ID of the certificate. Generated at creation time. ||
|| folder_id | **string**

ID of the folder that the certificate belongs to. ||
|| created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Creation timestamp. ||
|| name | **string**

Name of the certificate.
The name is unique within the folder. ||
|| description | **string**

Description of the certificate. ||
|| labels | **object** (map<**string**, **string**>)

Certificate labels as `key:value` pairs. ||
|| type | enum **CertificateType**

Type of the certificate.

- `CERTIFICATE_TYPE_UNSPECIFIED`
- `IMPORTED`: The certificate is imported by user.
- `MANAGED`: The certificate is created by service. ||
|| domains[] | **string**

Fully qualified domain names of the certificate. ||
|| status | enum **Status**

Status of the certificate.

- `STATUS_UNSPECIFIED`
- `VALIDATING`: The certificate domains validation are required. Used only for managed certificates.
- `INVALID`: The certificate issuance is failed. Used only for managed certificates.
- `ISSUED`: The certificate is issued.
- `REVOKED`: The certificate is revoked.
- `RENEWING`: The certificate renewal is started. Used only for managed certificates.
- `RENEWAL_FAILED`: The certificate renewal is failed. Used only for managed certificates. ||
|| issuer | **string**

[Distinguished Name](https://tools.ietf.org/html/rfc1779) of the certificate authority that issued the certificate. ||
|| subject | **string**

[Distinguished Name](https://tools.ietf.org/html/rfc1779) of the entity that is associated with the public key contained in the certificate. ||
|| serial | **string**

Serial number of the certificate. ||
|| updated_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time when the certificate is updated. ||
|| issued_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time when the certificate is issued. ||
|| not_after | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time after which the certificate is not valid. ||
|| not_before | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time before which the certificate is not valid. ||
|| challenges[] | **[Challenge](#yandex.cloud.certificatemanager.v1.Challenge)**

Domains validation challenges of the certificate. Used only for managed certificates. ||
|| deletion_protection | **bool**

Flag that protects deletion of the certificate ||
|| incomplete_chain | **bool**

Mark imported certificates without uploaded chain or with chain which not lead to root certificate ||
|#

## Challenge {#yandex.cloud.certificatemanager.v1.Challenge}

Domain validation challenge.

#|
||Field | Description ||
|| domain | **string**

Domain of the challenge. ||
|| type | enum **ChallengeType**

Type of the challenge.

- `CHALLENGE_TYPE_UNSPECIFIED`
- `DNS`: Domain validation type that using DNS-records.
- `HTTP`: Domain validation type that using HTTP-files. ||
|| created_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time when the challenge is created. ||
|| updated_at | **[google.protobuf.Timestamp](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#timestamp)**

Time when the challenge is updated. ||
|| status | enum **Status**

Status of the challenge.

- `STATUS_UNSPECIFIED`
- `PENDING`: The challenge is waiting to be completed.
- `PROCESSING`: The challenge is awaiting approval from Let's Encrypt.
- `VALID`: The challenge is complete.
- `INVALID`: The rights check for a specific domain failed or the one-week period allocated for the check expired. ||
|| message | **string**

Description of the challenge. ||
|| error | **string**

Error of the challenge. ||
|| dns_challenge | **[DnsRecord](#yandex.cloud.certificatemanager.v1.Challenge.DnsRecord)**

DNS-record.

Includes only one of the fields `dns_challenge`, `http_challenge`.

Data of the challenge. ||
|| http_challenge | **[HttpFile](#yandex.cloud.certificatemanager.v1.Challenge.HttpFile)**

HTTP-file.

Includes only one of the fields `dns_challenge`, `http_challenge`.

Data of the challenge. ||
|#

## DnsRecord {#yandex.cloud.certificatemanager.v1.Challenge.DnsRecord}

#|
||Field | Description ||
|| name | **string**

Name of the DNS record. ||
|| type | **string**

Type of the DNS-record. ||
|| value | **string**

Value of the DNS-record. ||
|#

## HttpFile {#yandex.cloud.certificatemanager.v1.Challenge.HttpFile}

#|
||Field | Description ||
|| url | **string**

Location of the HTTP file. ||
|| content | **string**

Content of the HTTP file. ||
|#