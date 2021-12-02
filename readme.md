# General information:

- All response must be Content-Type: application/json
- Upon a succesful request 200 status code should be returned, with the given response examples for each endpoint shown below in the **Endpoints** section
- Upon error an appropriate status code should be returned, along with an error response, which describes the error:
 ```
    {
        "error": "Error message"
    }
```

# Endpoints


## Lists endpoint

- Method: GET
- Request parameters (query):
    - api_key (string): The API key that will be used for authorization
- Response:
```
{   
    "page": 1,
    "total_pages": 2,
    "lists": [
        {
            "id": "abcde1234",
            "name": "My list",
            "count": 123
        },
        {
            "id": "efgh5678",
            "name": "Another list",
            "count": 543
        }
    ]
}
```
- **"page" (integer):** The current page
- **"total_pages" (integer):** The amount of total pages
- **"lists" (object array):** An array of list objects
    - list object
        - **"id" (string):** The ID of the list
        - **"name" (string):** The name of the list
        - **"count" (integer):** The amount of active contacts in the list


If your system only handles a single list per user, then the response should only contain a single list in the array. If your system doesn't use pagination and returns all the data for the request, then the "page" and "total_pages" both should be 1 and the **lists** array should contain all the data

## List endpoint

- Method: GET
- Request parameters (query):
    - api_key (string): The API key that will be used for authorization
    - list_id (string): The ID of the list that needs to be requested
- Response:
```
{
    "page": 1,
    "total_pages": 10,
    "id": "abcde1234",
    "name": "My list",
    "count": 2354,
    "contacts": [
        "contact1@example.com",
        "contact2@example.com"
    ]
}
```
- **"page" (integer):** The current page
- **"total_pages" (integer):** The amount of total pages
- **"id" (string):** The ID of the list
- **"name" (string):** The name of the list
- **"count" (integer):** The amount of active contacts in the list
- **"contacts" (string array):** An array of email addresses

The maximum amount of contacts should be 10000 per page. If your system doesn't use pagination and returns all the data for the request, then the "page" and "total_pages" both should be 1 and the **"contacts"** array should contain all the data. This endpoint should only return the active contacts for the given list.

## Action

- Method: POST
- Request parameters (query):
    - api_key (string): The API key that will be used for authorization
- Request parameters (body):
```
{
    "list_id": "abcdefg1234",
    "action": "unsubscribe",
    "contacts": [
        "contact1@example.com",
        "contact2@example.com",
        "contact3@example.com"
    ]
}
```
- **"list_id" (string):** The ID of the list
- **"action" (string enum):** The action that will be performed. Can be **"unsubscribe"** or **"delete"**
- **"contacts" (string array):** The contact email addresses, which the action will be performed on.

The request will be Content-Type: application/json. The endpoint should not send errors when email addresses, which doesn't exist anymore on the list are sent.

- Response:
    - 201 empty response

