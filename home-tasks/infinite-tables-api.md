#### Methods:

##### auth

###### login
Request
POST /auth/login
Header: Content-Type: application/json

    {
        "username": "username",
        "password": "password"
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json
    
    {
        "token": "token",
        "refresh_token": "refresh_token"
    }

###### refresh token
Request
POST /auth/token/refresh
Header: Content-Type: application/json

    {
        "refresh_token": "refresh_token"
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json
    
    {
        "token": "token",
        "refresh_token": "refresh_token"
    }

###### logout

request:
POST /auth/logout
Role: admin, user
Authorization: Bearer token

response:

    HTTP/1.1 200 OK

##### users

###### users.create
request:
POST /users
Role: admin
Authorization: Bearer token

    {
        "username": "username",
        "password": "password"
    }

response:

    HTTP/1.1 201 Created
    Content-Type: application/json
    
        {
            "id": 2
        }


###### users.edit

request:
PUT /users/{id}
Role: admin
Authorization: Bearer token

    {
        "username": "username",
        "password": "password"
    }

response:

    HTTP/1.1 200 OK
    Content-Type: application/json
    
        {
            "id": 2
        }


###### users.delete

request:
DELETE /users/{id}
Role: admin
Authorization: Bearer token

##### sheets

###### sheet.create

request:  
POST users/{id}/sheets
Role: admin, user
Authorization: Bearer token

    {
        "title": "title"
    }

response:

    HTTP/1.1 201 Created
    Content-Type: application/json

    {
        "id": 2
    }

###### sheet.getById

request:  
GET /sheet/{id}
Role: admin, user
Authorization: Bearer token

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "title": 2
    }
    
###### sheet.getByUser

request:  
GET users/{id}/sheets
Role: admin, user
Authorization: Bearer token

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "title": "title"
    }
    
###### sheet.update

request:  
PUT /sheets/{id}
Role: admin, user
Authorization: Bearer token

    {
        "title": "title"
    }

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": 2
    }

###### sheet.delete

request:  
DELETE /sheet/{id}
Role: admin, user
Authorization: Bearer token

response:

    HTTP/1.1 200 OK


###### cells

###### cell.get

request:
GET sheets/{id}/cells?range=A0:D3
Role: admin, user
Authorization: Bearer token
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
            [
                {
                    "col": A,
                    "row": 0,
                    "value": 1234,
                },
            ]
    }
    
###### cell.update

request:
PUT sheets/{id}/cells
Role: admin, user
Authorization: Bearer token

    {
        "col": A,
        "row": 0,
        "value": 1234,
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

###### cells.operations.sum

request:
GET sheets/{id}/cells/operations/sum?dimension=row&start=A0&offset=10
Role: admin, user
Authorization: Bearer token
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "value": 47135008,
    }

###### cells.operations.percentile

request:
GET sheets/{id}/cells/operations/percentile?col=A&percent=80
GET sheets/{id}/cells/operations/percentile?row=10&percent=80
Role: admin, user
Authorization: Bearer token
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "value": 1234 
    }



Ошибки API (HTTP статусы 400/401/405/500 etc):


    HTTP/1.1 403 Forbidden
       Content-Type: application/problem+json
    
       {
        "type": "https://example.com/table-access-denied",
        "title": "Sheet doesn't exist or you don't have access rights.",
        "detail": "Problem detail.",
        "user": 1355825
       }

    HTTP/1.1 400 Bad Request
       Content-Type: application/problem+json
    
       {
       "type": "https://example.net/validation-error",
       "title": "Your request parameters didn't validate.",
       "invalid-params": [ {
                             "name": "range",
                             "reason": "must contain only positive integer"
                           },
                         ]
       }
    
RFC:
https://tools.ietf.org/html/rfc7807 Problem Details for HTTP APIs
https://tools.ietf.org/html/rfc6648 Deprecating "X-"
https://tools.ietf.org/html/rfc7519 JSON Web Token (JWT)


#### Database:

**sheet**

    title
    user_id

**cell**

    sheet_id
    col
    row
    value
    
**user**

    id
    username
    password

#### Front-end:

> нам известны крайние координаты области, которая у нас в памяти на стороне клиента
> когда мы достигаем приближаемся к краю области (координата крайней точки минус заданная величина) 
    
    1) идет запрос get подгружающий ячейки
	2) если (х2 - х1) > max_length or (y1 - y2) > max_length область хранимая в памяти подрезается


   