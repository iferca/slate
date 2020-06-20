# Authentication API

Authentication happens through OAuth V2 protocol. As part of your contract and in order to integrate "machine to machine" you'll receive 3 set of credentials. 
Each credential is form by a pair of Key and Secret that are expected during the Authentication process. 

These credentials represent different "Root scopes", they are ultimately defined by the SLA or agreement applying to you as a client of Satellogic. 
You'll receive three Root Scope that in general can be described as follows: 

* Admin scope: allows administration of the interactions with your API from the rest of the provided scopes like permissions

* Tasking scope: allows interacting with the satellite planning and tasking API and image and data serving API
 
* Read-only scope: allows only for read-only operations like listing opportunities, tasks, its progress and status and obtaining the tasking resulting images.

Using the Root Scopes, in your system you can create users and assign roles and permissions that ultimately will hit Satellogic's API using one of these scopes. So you can 
further segment and limit access to the API but never go beyond the scope under which the request happens. 

<aside class="success">
Root scopes allows for segmentation and provide with additional contention for your integration.
</aside>

## Obtaining a session token

> To authenticate, use a code like this:

```shell
curl --request POST --url "https://api.satellogic.com/oauth/token" \
     -H 'content-type: application/json' \
     --data '{ 
                "client_id":"8jk3ozaT6bVpoBICufsEg8Xw7rKOnjSo", \
                "client_secret":"Kls9GULwerGbbZ18NE0jslWCYZpaWksjdBTB84zGE7O36bDIkSZCqayOFbN-Qru", \
                "audience":"https://api.satellogic.com/v1/", \
                "grant_type":"client_credentials" \
             }'
```

> Example response

```json  
  {
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik5TRjJ3LXdKMTBtWmgxR3pkdnY3UiJ9.eyJpc3MiOiJodHRwczovL3NhdGVsbG9naWMtcG9jLmV1LmF1dGgwLmNvbS8iLCJzdWIiOiI4YnIzaXNpVDZiVmtxQklDeWVzRXoxWHc3cktPbmpTb0BjbGllbnRzIiwiYXVkIjoiaHR0cHM6Ly9zYXRlbGxvZ2ljLXBvYy5ldS5hdXRoMC5jb20vYXBpL3YyLyIsImlhdCI6MTU5MjY2NDA2NiwiZXhwIjoxNTkyNzUwNDY2LCJhenAiOiI4YnIzaXNpVDZiVmtxQklDeWVzRXoxWHc3cktPbmpTbyIsInNjb3BlIjoicmVhZDpjbGllbnRfZ3JhbnRzIGNyZWF0ZTpjbGllbnRfZ3JhbnRzIGRlbGV0ZTpjbGllbnRfZ3JhbnRzIHVwZGF0ZTpjbGllbnRfZ3JhbnRzIHJlYWQ6dXNlcnMgdXBkYXRlOnVzZXJzIGRlbGV0ZTp1c2VycyBjcmVhdGU6dXNlcnMgcmVhZDp1c2Vyc19hcHBfbWV0YWRhdGEgdXBkYXRlOnVzZXJzX2FwcF9tZXRhZGF0YSBkZWxldGU6dXNlcnNfYXBwX21ldGFkYXRhIGNyZWF0ZTp1c2Vyc19hcHBfbWV0YWRhdGEgcmVhZDp1c2VyX2N1c3RvbV9ibG9ja3MgY3JlYXRlOnVzZXJfY3VzdG9tX2Jsb2NrcyBkZWxldGU6dXNlcl9jdXN0b21fYmxvY2tzIGNyZWF0ZTp1c2VyX3RpY2tldHMgcmVhZDpjbGllbnRzIHVwZGF0ZTpjbGllbnRzIGRlbGV0ZTpjbGllbnRzIGNyZWF0ZTpjbGllbnRzIHJlYWQ6Y2xpZW50X2tleXMgdXBkYXRlOmNsaWVudF9rZXlzIGRlbGV0ZTpjbGllbnRfa2V5cyBjcmVhdGU6Y2xpZW50X2tleXMgcmVhZDpjb25uZWN0aW9ucyB1cGRhdGU6Y29ubmVjdGlvbnMgZGVsZXRlOmNvbm5lY3Rpb25zIGNyZWF0ZTpjb25uZWN0aW9ucyByZWFkOnJlc291cmNlX3NlcnZlcnMgdXBkYXRlOnJlc291cmNlX3NlcnZlcnMgZGVsZXRlOnJlc291cmNlX3NlcnZlcnMgY3JlYXRlOnJlc291cmNlX3NlcnZlcnMgcmVhZDpkZXZpY2VfY3JlZGVudGlhbHMgdXBkYXRlOmRldmljZV9jcmVkZW50aWFscyBkZWxldGU6ZGV2aWNlX2NyZWRlbnRpYWxzIGNyZWF0ZTpkZXZpY2VfY3JlZGVudGlhbHMgcmVhZDpydWxlcyB1cGRhdGU6cnVsZXMgZGVsZXRlOnJ1bGVzIGNyZWF0ZTpydWxlcyByZWFkOnJ1bGVzX2NvbmZpZ3MgdXBkYXRlOnJ1bGVzX2NvbmZpZ3MgZGVsZXRlOnJ1bGVzX2NvbmZpZ3MgcmVhZDpob29rcyB1cGRhdGU6aG9va3MgZGVsZXRlOmhvb2tzIGNyZWF0ZTpob29rcyByZWFkOmFjdGlvbnMgdXBkYXRlOmFjdGlvbnMgZGVsZXRlOmFjdGlvbnMgY3JlYXRlOmFjdGlvbnMgcmVhZDplbWFpbF9wcm92aWRlciB1cGRhdGU6ZW1haWxfcHJvdmlkZXIgZGVsZXRlOmVtYWlsX3Byb3ZpZGVyIGNyZWF0ZTplbWFpbF9wcm92aWRlciBibGFja2xpc3Q6dG9rZW5zIHJlYWQ6c3RhdHMgcmVhZDp0ZW5hbnRfc2V0dGluZ3MgdXBkYXRlOnRlbmFudF9zZXR0aW5ncyByZWFkOmxvZ3MgcmVhZDpzaGllbGRzIGNyZWF0ZTpzaGllbGRzIHVwZGF0ZTpzaGllbGRzIGRlbGV0ZTpzaGllbGRzIHJlYWQ6YW5vbWFseV9ibG9ja3MgZGVsZXRlOmFub21hbHlfYmxvY2tzIHVwZGF0ZTp0cmlnZ2VycyByZWFkOnRyaWdnZXJzIHJlYWQ6Z3JhbnRzIGRlbGV0ZTpncmFudHMgcmVhZDpndWFyZGlhbl9mYWN0b3JzIHVwZGF0ZTpndWFyZGlhbl9mYWN0b3JzIHJlYWQ6Z3VhcmRpYW5fZW5yb2xsbWVudHMgZGVsZXRlOmd1YXJkaWFuX2Vucm9sbG1lbnRzIGNyZWF0ZTpndWFyZGlhbl9lbnJvbGxtZW50X3RpY2tldHMgcmVhZDp1c2VyX2lkcF90b2tlbnMgY3JlYXRlOnBhc3N3b3Jkc19jaGVja2luZ19qb2IgZGVsZXRlOnBhc3N3b3Jkc19jaGVja2luZ19qb2IgcmVhZDpjdXN0b21fZG9tYWlucyBkZWxldGU6Y3VzdG9tX2RvbWFpbnMgY3JlYXRlOmN1c3RvbV9kb21haW5zIHVwZGF0ZTpjdXN0b21fZG9tYWlucyByZWFkOmVtYWlsX3RlbXBsYXRlcyBjcmVhdGU6ZW1haWxfdGVtcGxhdGVzIHVwZGF0ZTplbWFpbF90ZW1wbGF0ZXMgcmVhZDptZmFfcG9saWNpZXMgdXBkYXRlOm1mYV9wb2xpY2llcyByZWFkOnJvbGVzIGNyZWF0ZTpyb2xlcyBkZWxldGU6cm9sZXMgdXBkYXRlOnJvbGVzIHJlYWQ6cHJvbXB0cyB1cGRhdGU6cHJvbXB0cyByZWFkOmJyYW5kaW5nIHVwZGF0ZTpicmFuZGluZyBkZWxldGU6YnJhbmRpbmcgcmVhZDpsb2dfc3RyZWFtcyBjcmVhdGU6bG9nX3N0cmVhbXMgZGVsZXRlOmxvZ19zdHJlYW1zIHVwZGF0ZTpsb2dfc3RyZWFtcyBjcmVhdGU6c2lnbmluZ19rZXlzIHJlYWQ6c2lnbmluZ19rZXlzIHVwZGF0ZTpzaWduaW5nX2tleXMgcmVhZDpsaW1pdHMgdXBkYXRlOmxpbWl0cyIsImd0eSI6ImNsaWVudC1jcmVkZW50aWFscyJ9.dKai2a-M-6Cn_8byOurMMd3Vm8bWd3YYknZZ2NPGPkRzb8GkzZDKWxcHCM9waomlJKVF6w16HAByX439n7XnaXxYv_7IK9JOR0F2k-zIaWWw-GjttLDWZeTuPqQx2Zt3k-kwslX3NHBAji7-SW1cthCmQumCEeYlHLVWM64eSAUKTIGfCkFiLuaqx9-u4hCDOt3Bdr2kICYPJF7LN5Hn3TyNCPPRcHP-H8Q5a-tAwtvtPwo0z2fgaHGwWc91Wd3ucdQuB2g7iHoc5KrdtcK_2n5sdLj2LP6jAPUviUgIoyqe44YUzxLqwW1BpmD2IOxWVyd-bZNFmMp0rofT679KXw",
    "token_type": "Bearer"
    # TODO Expiration/TTL?
  }
``` 

In order to make request to the API you need a session token you can request using one of your Root Scope credentials. 
The resulting token is scoped withing the Root scope of the credentials you used to generate it. 

### Request payload description

Parameter | Description 
--------- | -----------
client_id | The client ID of the Root scope credentials you plan to get a token for
client_secret | The secret Id of the Root scope credentials you plan to get a token for
audience | The url of the API you want to access
grant_type | The type of token you are requesting

<aside class="notice">
The tokens are samples. The URLs could be different depending on your location and contract.
</aside>

