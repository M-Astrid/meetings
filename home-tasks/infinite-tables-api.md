#### Database:

**table**

    title
    user_id

**cell**

    table_id
    x
    y
    val

#### Front-end:

> нам известны крайние координаты области, которая у нас в памяти на стороне клиента
> когда мы достигаем приближаемся к краю области (координата крайней точки минус заданная величина) 
    
    1) идет запрос get подгружающий ячейки
	2) если (х2 - х1) > max_length or (y1 - y2) > max_length область хранимая в памяти подрезается


   
#### Methods:

##### table

###### table.create

request:  
POST /table/create

    {
        "title": "tableTitle"
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

###### cells.get

request:
GET /cells/get

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





    HTTP/1.1 404 Not Found
    Content-Type: application/problem+json
    Content-Language: en

    {
        "type": "https://example.com/probs/out-of-credit",
        "title": "You do not have enough credit.",
        "detail": "Your current balance is 30, but that costs 50.",
        "instance": "/account/12345/msgs/abc",
        "balance": 30,
        "accounts": ["/account/12345",
                 "/account/67890"]
    }