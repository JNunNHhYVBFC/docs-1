---
editable: false
sourcePath: en/_api-ref/organizationmanager/api-ref/Group/list.md
---

# Method list
Retrieves the list of group resources.
 

 
## HTTP request {#https-request}
```
GET https://organization-manager.{{ api-host }}/organization-manager/v1/groups
```
 
## Query parameters {#query_params}
 
Parameter | Description
--- | ---
organizationId | Required. ID of the organization to list groups in. To get the organization ID, use a [list](/docs/organization/api-ref/Organization/list) request.  The maximum string length in characters is 50.
pageSize | The maximum number of results per page to return. If the number of available results is larger than [pageSize](/docs/organization/api-ref/Group/list#query_params), the service returns a [nextPageToken](/docs/organization/api-ref/Group/list#responses) that can be used to get the next page of results in subsequent list requests. Default value: 100.  Acceptable values are 0 to 1000, inclusive.
pageToken | Page token. Set [pageToken](/docs/organization/api-ref/Group/list#query_params) to the [nextPageToken](/docs/organization/api-ref/Group/list#responses) returned by a previous list request to get the next page of results.  The maximum string length in characters is 100.
filter | A filter expression that filters resources listed in the response. The expression must specify: 1. The field name. Currently you can use filtering only on the [Group.name](/docs/organization/api-ref/Group#representation) field. 2. An `=` operator. 3. The value in double quotes (`"`). Must be 3-63 characters long and match the regular expression `[a-z][-a-z0-9]{1,61}[a-z0-9]`.  The maximum string length in characters is 1000.
 
## Response {#responses}
**HTTP Code: 200 - OK**

```json 
{
  "groups": [
    {
      "id": "string",
      "organizationId": "string",
      "createdAt": "string",
      "name": "string",
      "description": "string"
    }
  ],
  "nextPageToken": "string"
}
```

 
Field | Description
--- | ---
groups[] | **object**<br><p>A Group resource. For more information, see <a href="/docs/organization/groups">Groups</a>.</p> 
groups[].<br>id | **string**<br><p>ID of the group.</p> 
groups[].<br>organizationId | **string**<br><p>ID of the organization that the group belongs to.</p> 
groups[].<br>createdAt | **string** (date-time)<br><p>Creation timestamp.</p> <p>String in <a href="https://www.ietf.org/rfc/rfc3339.txt">RFC3339</a> text format.</p> 
groups[].<br>name | **string**<br><p>Name of the group.</p> 
groups[].<br>description | **string**<br><p>Description of the group.</p> 
nextPageToken | **string**<br><p>This token allows you to get the next page of results for list requests. If the number of results is larger than <a href="/docs/organization/api-ref/Group/list#query_params">pageSize</a>, use the <a href="/docs/organization/api-ref/Group/list#responses">nextPageToken</a> as the value for the <a href="/docs/organization/api-ref/Group/list#query_params">pageToken</a> query parameter in the next list request. Each subsequent list request will have its own <a href="/docs/organization/api-ref/Group/list#responses">nextPageToken</a> to continue paging through the results.</p> 