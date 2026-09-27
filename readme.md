# FastAPI and Backend Development

# CHAPTER 1: INTRODUCTION
## REST APIs

**REST** = **Representational State Transfer**

**REST API** = A way for two applications to communicate using standard **HTTP methods**.

### Endpoint

- An **endpoint** is a URL on the server where a client can send a request.

Example:

```text
http://localhost:8000/shipment
```

## How REST API Works

```text
Client
   |
   | HTTP Request
   | GET /shipment
   ↓
Server / API
   |
   | Response
   ↓
Client
```

- Client sends an **HTTP request** to a URL called an **endpoint**.
- Server processes the request.
- Server responds with data, usually in **JSON**.

---

## HTTP Methods

| Method | Purpose |
|---|---|
| **GET** | Read data |
| **POST** | Create data |
| **PUT** | Update the entire record |
| **PATCH** | Update part of a record |
| **DELETE** | Remove data |

---

## HTTP Response Status Codes

#### Common Status Codes

| Code | Meaning |
|---|---|
| **200** | OK |
| **400** | Bad Request |
| **500** | Server Error |
| **418** | I'm a teapot |

#### Status Code Categories

| Range | Meaning |
|---|---|
| **2xx** | Success |
| **3xx** | Redirection |
| **4xx** | Client-side error |
| **5xx** | Server-side error |

> REST provides a standard way to structure communication between a client and a server (two programs).

---

## OpenAPI

**OpenAPI** is a **standard format for describing REST APIs**.

When you write a FastAPI application, it automatically generates:

- **OpenAPI specification**
- **Swagger UI documentation**
- **ReDoc documentation**

### Swagger

**Swagger** is a tool that reads an **OpenAPI specification** and creates an interactive UI.

The UI allows developers to:

- View API endpoints
- Understand the API
- Test the API

### OpenAPI Flow

```text
FastAPI Code
     ↓
Generates OpenAPI Specification
     ↓
Swagger / ReDoc reads specification
     ↓
Developers can view and test APIs
```

### FastAPI Documentation

If the application is running at:

```text
http://localhost:8000
```

FastAPI automatically provides:

```text
http://localhost:8000/docs
```

This is the **Swagger UI**.

#### ReDoc

```text
http://localhost:8000/redoc
```

ReDoc provides another interface for viewing API documentation.

> `/docs` → Swagger UI  
> `/redoc` → ReDoc

---

## FastAPI

**FastAPI** is a modern Python framework used to **build REST APIs quickly and efficiently**.

### REST API vs FastAPI

```text
REST API = Concept / Architecture
FastAPI  = Python framework used to implement REST APIs
```

### FastAPI vs Django vs Flask

| Framework | Main Use |
|---|---|
| **Django** | Build complete websites/web applications |
| **FastAPI** | Build backend APIs |
| **Flask** | Build small web apps/APIs with minimal setup |

---

## `__init__.py`

`__init__.py` is a special file that marks a directory as a **Python package**.

It:

- Allows modules within the directory to be imported.
- Can contain package initialization code.
- Can re-export commonly used classes and functions.

---

## Virtual Environment

### Create a Virtual Environment

```bash
python -m venv ./venv
```

### Activate the Virtual Environment

```bash
venv/Scripts/activate
```

---

#### Install FastAPI

```bash
pip install fastapi[all]
```

---

# CHAPTER 2: PATH PARAMETERS
## API Endpoint

Example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/shipment")
def get_shipment():
    return {
        "content": "wooden table",
        "status": "in transit"
    }
```

### Explanation

```python
@app.get("/shipment")
```

Means:

> Whenever someone sends a **GET request** to `/shipment`, run the function below.

```python
def get_shipment():
```

`get_shipment()` is the **route handler**.

### Flow

```text
Client
   |
   | GET /shipment
   ↓
FastAPI
   |
   | calls get_shipment()
   ↓
Returns dictionary
   |
   ↓
JSON response sent to client
```

FastAPI automatically converts the returned Python dictionary into a **JSON response**.

---

## Starting the FastAPI Server

Run:

```bash
fastapi dev
```

Then go to:

```text
<server_link>/<endpoint_name>
```

For example:

```text
http://localhost:8000/shipment
```

---

## FastAPI Documentation

### Swagger UI

```text
<server_link>/docs
```

- Interactive API documentation.
- Uses the OpenAPI specification.

### ReDoc

```text
<server_link>/redoc
```

- Another interface for viewing API documentation.
- Lighter documentation interface.

---

## Scalar

**Scalar** is another tool for **API testing and documentation**.

### Install Scalar

```bash
pip install scalar_fastapi
```

### Example

```python
from scalar_fastapi import get_scalar_api_reference

@app.get("/scalar", include_in_schema=False)
def get_scalar_docs():
    return get_scalar_api_reference(
        openapi_url=app.openapi_url,
        title="Scalar API",
    )
```

### Explanation

#### `@app.get("/scalar")`

Creates a **GET endpoint** at:

```text
/scalar
```

#### `get_scalar_docs()`

Route handler that runs when `/scalar` is visited.

#### `get_scalar_api_reference(...)`

Returns a **Scalar UI** page for viewing and testing APIs.

#### `openapi_url=app.openapi_url`

Uses your application's **OpenAPI specification**.

#### `title="Scalar API"`

Sets the title of the Scalar documentation page.

### Scalar Flow

```text
FastAPI Application
        ↓
OpenAPI Specification
        ↓
Scalar
        ↓
View / Test APIs
```

---

## Postman

**Postman** is a separate application used to **test APIs**.

It can be used to:

- Send API requests
- Test endpoints
- View responses

Postman can also use an **OpenAPI specification**.

> Postman can also be installed/used as an extension in VS Code.

---

## Type Hinting

Type hinting tells Python/developers what type a variable is expected to have.

Example:

```python
text: str = "value"
```

Here:

- `text` → variable
- `str` → expected type
- `"value"` → value

---

## Decorators

A **decorator is a function that adds extra features to the function where it's used.**

### Basic Example

```python
def decorator(func):

    def wrapper():
        print("Before")
        func()
        print("After")

    return wrapper
```

Using the decorator:

```python
@decorator
def greet():
    print("Hello")
```

Python internally treats it as:

```python
def greet():
    print("Hello")

greet = decorator(greet)
```

So:

```python
@decorator
def greet():
```

is essentially:

```python
greet = decorator(greet)
```

---

## Decorators with Arguments

Example:

```python
def log_function(func):

    def wrapper(name):
        print(f"Calling {func.__name__}")
        func(name)

    return wrapper
```

Using it:

```python
@log_function
def greet(name):
    print(f"Hello {name}")

greet("Nikita")
```

#### Output

```text
Calling greet
Hello Nikita
```

---

## Type Hinting in Functions

We can use `Callable` and `Any` to type-hint functions.

```python
from typing import Callable, Any

def decorator(func: Callable[[Any], None]):
    pass
```

### `Callable`

`Callable` means **a function/callable object**.

```python
Callable[[Any], None]
```

Means:

- `[Any]` → function takes **one argument of any type**
- `None` → function **returns nothing**

So this decorator expects a function like:

```python
def greet(name: str) -> None:
    print(f"Hello {name}")
```

---

## Path Parameters and Type Hinting

Path parameters allow us to pass values directly through the URL.

Example:

```python
from typing import Any
from fastapi import FastAPI

app = FastAPI()

@app.get("/shipment/{id}")
def get_shipment(id: int) -> dict[str, str | int | float]:
    return {
        "id": id,
        "weight": 1.2,
        "content": "wooden table",
        "status": "in transit"
    }
```

### Example Request

```text
GET /shipment/10
```

Here:

```text
{id}
```

is the **path parameter**.

FastAPI receives:

```text
id = 10
```

Because we specified:

```python
id: int
```

FastAPI expects `id` to be an integer.

### Return Type

```python
dict[str, str | int | float]
```

Means the function returns a dictionary where:

- Keys are `str`
- Values can be `str`, `int`, or `float`

Example response:

```json
{
    "id": 10,
    "weight": 1.2,
    "content": "wooden table",
    "status": "in transit"
}
```

---

## Route Ordering

**Route ordering** means the order in which FastAPI checks routes to find a match for a request.

FastAPI evaluates routes **from top to bottom**.

> The **first matching route wins**.

Therefore, the order in which routes are defined can matter, especially when a dynamic path parameter could match a fixed path.
