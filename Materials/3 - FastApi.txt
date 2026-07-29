# FastAPI Request Parameters Guide

## Table of Contents

1. Introduction
2. Path Parameters
3. Query Parameters
4. Body Parameters
5. Combining Multiple Parameter Types
6. Helper Functions in FastAPI
7. Parameter Validation
8. Common Real-World Examples
9. Summary

---

# 1. Introduction

Whenever a client sends a request to your FastAPI application, the data can come from several places:

* URL path
* Query string
* Request body
* Headers
* Cookies
* Forms

FastAPI provides helper functions that allow you to:

* Validate incoming data
* Add constraints
* Improve API documentation
* Provide aliases and descriptions
* Control how the request data is parsed

The most common helper functions are:

```python
Path()
Query()
Body()
Header()
Cookie()
Form()
```

---

# 2. Path Parameters

Path parameters are values embedded directly inside the URL.

Example:

```text
/users/15
/products/42/reviews
```

### Basic Example

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}
```

Request:

```text
GET /users/15
```

Response:

```json
{
    "user_id": 15
}
```

FastAPI automatically:

* Converts the type
* Performs validation
* Returns helpful error messages

---

## Using Path()

The Path() helper allows additional validation and metadata.

```python
from typing import Annotated
from fastapi import Path


@app.get("/users/{user_id}")
async def get_user(
    user_id: Annotated[
        int,
        Path(
            gt=0,
            le=1000,
            description="User ID must be between 1 and 1000"
        )
    ]
):
    return {"user_id": user_id}
```

Valid:

```text
/users/25
```

Invalid:

```text
/users/-10
/users/5000
```

---

## Common Path() Arguments

```python
Path(
    gt=0,
    ge=1,
    lt=100,
    le=50,
    title="User ID",
    description="Positive user ID",
    deprecated=False
)
```

### Notes

* Path parameters are always required.
* They cannot have default values.
* They cannot be optional.

---

# 3. Query Parameters

Query parameters appear after the '?' symbol.

Example:

```text
/products?page=2&limit=20
```

Query parameters are usually used for:

* Pagination
* Filtering
* Sorting
* Searching

---

## Basic Example

```python
@app.get("/products")
async def get_products(
    page: int = 1,
    limit: int = 10
):
    return {
        "page": page,
        "limit": limit
    }
```

Request:

```text
GET /products?page=2&limit=25
```

---

## Optional Query Parameters

```python
@app.get("/search")
async def search(
    q: str | None = None
):
    return {"query": q}
```

Requests:

```text
/ search
/ search?q=laptop
```

---

## Required Query Parameters

```python
@app.get("/search")
async def search(
    q: str
):
    return {"query": q}
```

Request:

```text
/ search?q=python
```

If q is not provided, FastAPI returns a validation error.

---

## Using Query()

```python
from fastapi import Query
from typing import Annotated


@app.get("/search")
async def search(
    q: Annotated[
        str | None,
        Query(
            min_length=3,
            max_length=50
        )
    ] = None
):
    return {"query": q}
```

---

## Numeric Validation

```python
page: Annotated[
    int,
    Query(
        ge=1,
        le=100
    )
]
```

Example:

```text
?page=5
```

Invalid:

```text
?page=0
?page=500
```

---

## Regular Expressions

```python
q: Annotated[
    str,
    Query(
        pattern="^fastapi$"
    )
]
```

Only accepts:

```text
?q=fastapi
```

---

## Alias Example

Sometimes APIs use different naming conventions.

```python
page_number: Annotated[
    int,
    Query(alias="page-number")
]
```

Client sends:

```text
?page-number=5
```

Your function receives:

```python
page_number == 5
```

---

## List Parameters

```python
tags: Annotated[
    list[str] | None,
    Query()
] = None
```

Request:

```text
/items?tags=python&tags=fastapi&tags=asyncio
```

Response:

```json
{
    "tags": [
        "python",
        "fastapi",
        "asyncio"
    ]
}
```

---

# 4. Body Parameters

Body parameters are sent inside the HTTP request body.

They are commonly used in:

* POST
* PUT
* PATCH

The body usually contains JSON data.

Example:

```json
{
    "name": "Aditya",
    "age": 24
}
```

---

## Using Pydantic Models

FastAPI uses Pydantic models for request validation.

```python
from pydantic import BaseModel


class User(BaseModel):
    name: str
    age: int


@app.post("/users")
async def create_user(user: User):
    return user
```

Request:

```json
{
    "name": "Aditya",
    "age": 24
}
```

---

## Multiple Body Parameters

```python
from pydantic import BaseModel


class User(BaseModel):
    name: str


class Address(BaseModel):
    city: str


@app.post("/profile")
async def create_profile(
    user: User,
    address: Address
):
    return {
        "user": user,
        "address": address
    }
```

Request:

```json
{
    "user": {
        "name": "Aditya"
    },
    "address": {
        "city": "Bangalore"
    }
}
```

---

## Using Body()

The Body() helper provides validation and metadata.

```python
from fastapi import Body


@app.post("/rating")
async def rate_product(
    rating: Annotated[
        int,
        Body(
            ge=1,
            le=5
        )
    ]
):
    return {"rating": rating}
```

Request:

```json
5
```

---

## Embedding Body Parameters

Without embedding:

```json
"Aditya"
```

With embedding:

```python
name: Annotated[
    str,
    Body(embed=True)
]
```

Request:

```json
{
    "name": "Aditya"
}
```

This is useful when designing cleaner APIs.

---

# 5. Combining Multiple Parameter Types

FastAPI automatically determines where data should come from.

Example:

```python
from typing import Annotated
from fastapi import Query, Path
from pydantic import BaseModel


class UserUpdate(BaseModel):
    name: str
    age: int


@app.put("/users/{user_id}")
async def update_user(
    user_id: Annotated[
        int,
        Path(gt=0)
    ],
    notify: Annotated[
        bool,
        Query()
    ] = False,
    user: UserUpdate = None
):
    return {
        "user_id": user_id,
        "notify": notify,
        "data": user
    }
```

Request:

```text
PUT /users/10?notify=true
```

Body:

```json
{
    "name": "Aditya",
    "age": 24
}
```

FastAPI understands:

```text
user_id -> Path

notify -> Query parameter

user -> Request body
```

---

# 6. Helper Functions in FastAPI

The most commonly used helper functions are:

| Helper Function | Used For         |
| --------------- | ---------------- |
| Path()          | Path parameters  |
| Query()         | Query parameters |
| Body()          | Request body     |
| Header()        | HTTP headers     |
| Cookie()        | Cookies          |
| Form()          | Form data        |

---

## Header()

Used for reading HTTP headers.

```python
from fastapi import Header


@app.get("/client")
async def get_client(
    user_agent: Annotated[
        str | None,
        Header()
    ] = None
):
    return {
        "user_agent": user_agent
    }
```

Header:

```text
User-Agent: Mozilla/5.0
```

Response:

```json
{
    "user_agent": "Mozilla/5.0"
}
```

---

## Header Aliasing

FastAPI automatically converts:

```python
user_agent
```

to:

```text
User-Agent
```

Underscores become hyphens automatically.

---

## Cookie()

Used to retrieve cookies.

```python
from fastapi import Cookie


@app.get("/session")
async def get_session(
    session_id: Annotated[
        str | None,
        Cookie()
    ] = None
):
    return {
        "session_id": session_id
    }
```

Request:

```text
Cookie: session_id=abc123
```

---

## Form()

Used for HTML form submissions.

```python
from fastapi import Form


@app.post("/login")
async def login(
    username: Annotated[
        str,
        Form()
    ],
    password: Annotated[
        str,
        Form()
    ]
):
    return {
        "username": username
    }
```

Form Data:

```text
username=admin
password=secret
```

---

# 7. Parameter Validation

FastAPI supports several validation options.

For numbers:

```python
gt=0
ge=0
lt=100
le=50
```

For strings:

```python
min_length=3
max_length=50
pattern="^[a-z]+$"
```

Metadata:

```python
title=
description=
deprecated=
alias=
examples=
```

---

## Example

```python
Query(
    min_length=3,
    max_length=50,
    description="Search keyword"
)
```

Swagger UI automatically displays this information.

---

# 8. Common Real-World Examples

### Pagination

```python
@app.get("/products")
async def get_products(
    page: int = 1,
    limit: int = 10
):
    pass
```

Request:

```text
/products?page=2&limit=20
```

---

### Filtering

```python
@app.get("/products")
async def get_products(
    category: str | None = None,
    brand: str | None = None
):
    pass
```

Request:

```text
/products?category=laptops&brand=apple
```

---

### User Details

```python
@app.get("/users/{user_id}")
async def get_user(user_id: int):
    pass
```

Request:

```text
/users/15
```

---

### Creating a User

```python
class User(BaseModel):
    name: str
    email: str


@app.post("/users")
async def create_user(user: User):
    pass
```

Request:

```json
{
    "name": "Aditya",
    "email": "abc@gmail.com"
}
```

---

### Updating a User

```python
@app.patch("/users/{user_id}")
async def update_user(
    user_id: int,
    user: User
):
    pass
```

Request:

```text
PATCH /users/15
```

Body:

```json
{
    "name": "Updated Name",
    "email": "updated@gmail.com"
}
```

---

# 9. Summary

| Parameter Type | Helper Function | Common Usage                          |
| -------------- | --------------- | ------------------------------------- |
| Path           | Path()          | Resource identifiers                  |
| Query          | Query()         | Filtering and pagination              |
| Body           | Body()          | JSON request payloads                 |
| Header         | Header()        | Authentication and client information |
| Cookie         | Cookie()        | Sessions and tracking                 |
| Form           | Form()          | Login forms and HTML submissions      |

### Rules to Remember

* Use Path() for URL path values.
* Use Query() for query string parameters.
* Use Body() for JSON payloads.
* Path parameters are always required.
* Query parameters can be optional or required.
* Use Pydantic models whenever possible for request bodies.
* Helper functions allow validation, metadata, aliases and better API documentation.
* FastAPI automatically generates OpenAPI and Swagger documentation from these definitions.

A useful rule of thumb is:

* Path parameters answer "Which resource?"
* Query parameters answer "How should I retrieve it?"
* Body parameters answer "What data should I send?"
