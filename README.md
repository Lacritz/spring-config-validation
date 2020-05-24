# Spring Configuration Validation
This project is a wrapper around the spring configuration mechanism and adds feature such as dry-run validation 
and exposing the configuration properties via REST. 

## Installation

## How to Use
 
Configuration IDs are accessible using `GET /rest/v1/config`.  
_Example:_ 
```json
[
  "com.lacritz.example.postgres",
  "com.lacritz.example.mysql",
  "com.lacritz.example.application"
]
```

Configurations are accessible using `GET /rest/v1/config/{id}`  
_Example:_
```json
{
  "id": "com.lacritz.example.postgres",
  "group": "Database",
  "properties": [
    {
      "name": "com.lacritz.example.postgres.url",
      "type": "URL" 
    },
    {
      "name": "com.lacritz.example.postgres.user",
      "type": "string"
    },
    {
      "name": "com.lacritz.example.postgres.password",
      "type": "char[]"
    }
  ]
}
```

Validation for a specific configuration can be triggered using `POST /rest/v1/config/{id}/validate`.  
The property snippet must be sent as part of the body.  
_Example (*.json):_
```json
[
  {"com.lacritz.example.postgres.url": "http://localhost:5432"},
  {"com.lacritz.example.postgres.user": "lacritz"},
  {"com.lacritz.example.postgres.password": "secret"}
]
```  
_Example (*.properties)_
```properties
com.lacritz.example.postgres.url: "http://localhost:5432"
com.lacritz.example.postgres.user: "lacritz"
com.lacritz.example.postgres.secret: "secret"
``` 

The server will respond with a validation result.  
_Example:_
```json
{
  "id": "com.lacritz.example.postgres",
  "group": "Database",
  "properties": [
    {
      "name": "com.lacritz.example.postgres.url",
      "status": true
    },
    {
      "name": "com.lacritz.example.postgres.user",
      "valid": false,
      "reason": "The user must not be empty."
    },
    {
      "name": "com.lacritz.example.postgres.password",
      "valid": true
    }
  ]
}
```
