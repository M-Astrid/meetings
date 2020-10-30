#### Methods:

##### table

###### table.create

request:  
POST /table/create

    {
        "title": "tableTitle",
        "user_id": "user_id"
    }

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    [
        {
            "id": 2
        }
    ]

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
GET /{userId}/tables

response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "title": 2
    }
    
###### table.update

request:  
POST /table/update

    {
        "id": 2,
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
POST /table/delete

    {
        "id": 2
    }

response:

    HTTP/1.1 200 OK


###### cells

###### cell.get

request:
GET /cell/get

    {
        "tableId": 232,
        "range": "0,0:4,3"
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "range": "0,0:4,3"
        "values":[
            [1, 3, null, 3],
            [1, 6, 17, 3],
            [8, -1, 4, 156],
        ]
    }
    
###### cell.update

request:
GET /cell/update

    {
        "tableId": 232,
        "range": range string (e.g. "0,0:4,3") or single cell "0,0"
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "range": "0,0:4,3"
        "values":[
            [1, 3, null, 3],
            [1, 6, 17, 3],
            [8, -1, 4, 156],
        ]
    }

###### cells.operations.sum

request:
GET /cells/operations/sum

    {
        "tableId": 232,
        "dimension": "row",         
        "range": "4,3:135008,3",
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "range": "4,3:135008,3",
        "dimension": "row", 
        "value": 47135008,
    }

###### cells.operations.percentile

request:
GET /cells/operations/percentile

    {
        "tableId": 232,
        "dimension": "col",       
        "range": "1,1:null",
    }
    
response:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "dimension": "col",       
        "range": "1,1:5",
        "value": [100%, 56%, 14%, 56%, 14%, 98%]
    }



Ошибки API (HTTP статусы 400/401/405/500 etc):


    HTTP/1.1 403 Forbidden
       Content-Type: application/problem+json
    
       {
        "type": "https://example.com/table-access-denied",
        "title": "Table doesn't exist or you don't have access rights.",
        "detail": "Your current balance is 30, but that costs 50.",
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


   