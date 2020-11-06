#### Methods:

##### table

###### table.create

request:  
POST users/{userId}/tables

    {
        "title": "tableTitle"
    }

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": 2
    }

###### table.getById

request:  
GET /table/{id}

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "title": 2
    }
    
###### table.getByUser

request:  
GET users/{userId}/tables

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "title": "title"
    }
    
###### table.update

request:  
PUT /tables/{id}

    {
        "title": "tableTitle"
    }

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "id": 2
    }

###### table.delete

request:  
DELETE /table/{id}

response:

    HTTP/1.1 200 OK


###### cells

###### cell.get

request:
GET tables/{id}/cells?range=0,0:4,3
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "range": "0,0:4,3"
        "values":[
            {
                x: 0,
                y: 0,
                value: null,
            },
            {
                x: 0,
                y: 1,
                value: 1234,
            },
        ]
    }
    
###### cell.update

request:
PUT tables/{id}/cells

    {
        "range": 0,0:4,3
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

###### cells.operations.sum

request:
GET tables/{id}/cells/operations/sum?dimension=row&range=4,3:135008,3
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "value": 47135008,
    }

###### cells.operations.percentile

request:
GET tables/{id}/cells/operations/percentile?dimension=col&range=4,3:135008,3
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "value": [100%, 56%, 14%, 56%, 14%, 98%]
    }



Ошибки API (HTTP статусы 400/401/405/500 etc):


    HTTP/1.1 403 Forbidden
       Content-Type: application/problem+json
    
       {
        "type": "https://example.com/table-access-denied",
        "title": "Table doesn't exist or you don't have access rights.",
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
https://tools.ietf.org/html/rfc7807



#### Database:

**table**

    title
    user_id

**cell**

    table_id
    x
    y
    val
    
**user**

    id
    ...

#### Front-end:

> нам известны крайние координаты области, которая у нас в памяти на стороне клиента
> когда мы достигаем приближаемся к краю области (координата крайней точки минус заданная величина) 
    
    1) идет запрос get подгружающий ячейки
	2) если (х2 - х1) > max_length or (y1 - y2) > max_length область хранимая в памяти подрезается


   