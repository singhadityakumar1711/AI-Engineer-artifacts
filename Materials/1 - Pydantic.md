# Pydantic

Most modern AI frameworks (LangChain, LangGraph, OpenAI SDK, FastAPI, Instructor, LlamaIndex, CrewAI, MCP servers) use Pydantic extensively for:

- Validating LLM outputs
- Defining structured inputs/outputs
- Configuration management
- API contracts
- Agent state management

---

## 1. BaseModel

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

user = User(name="Aditya", age=24)

print(user.name)
```

Output:

```text
Aditya
```

---

## 2. Type Validation

```python
class User(BaseModel):
    age: int

user = User(age="24")
print(user.age)
```

Output:

```text
24
```

Pydantic automatically converts compatible types whenever possible.

---

## 3. Field Constraints

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    price: float = Field(gt=0)      # Greater than 0
    rating: float = Field(ge=0, le=5)  # Greater than or equal to 0 and less than or equal to 5
```

Valid:

```python
Product(price=100, rating=4.5)
```

Invalid:

```python
Product(price=-10, rating=6)
```

---

## 4. Optional Fields

```python
from typing import Optional

class User(BaseModel):
    name: str
    phone: Optional[str] = None
```

Equivalent in modern Python (3.10+):

```python
phone: str | None = None
```

---

## 5. Lists and Dictionaries

```python
from typing import List

class Document(BaseModel):
    chunks: List[str]


doc = Document(
    chunks=["chunk1", "chunk2"]
)
```

---

## 6. Nested Models

```python
from pydantic import BaseModel

class Address(BaseModel):
    city: str
    country: str

class User(BaseModel):
    name: str
    address: Address

user = User(
    name="Aditya",
    address={
        "city": "Bangalore",
        "country": "India"
    }
)
```

Accessing nested fields:

```python
print(user.address.city)
```

Output:

```text
Bangalore
```

---

## 7. model_dump()

Converts a Pydantic model into a dictionary.

```python
user.model_dump()
```

Example output:

```python
{
    "name": "Aditya",
    "address": {
        "city": "Bangalore",
        "country": "India"
    }
}
```

---

## 8. model_validate()

Converts a dictionary into a Pydantic model.

```python
data = {
    "name": "Aditya",
    "age": 24
}

user = User.model_validate(data)
```

Equivalent syntax:

```python
user = User(**data)
```

---

## 9. Validators

### Single Field Validation

```python
from pydantic import BaseModel, field_validator

class User(BaseModel):
    age: int

    @field_validator("age")
    @classmethod
    def validate_age(cls, value):
        if value < 18:
            raise ValueError("Must be adult")
        return value
```

Valid:

```python
User(age=24)
```

Invalid:

```python
User(age=15)
```

### Data Transformation

Validators can also transform values.

```python
class User(BaseModel):
    name: str

    @field_validator("name")
    @classmethod
    def clean_name(cls, value):
        return value.strip().title()

user = User(name="   aditya kumar singh   ")
print(user.name)
```

Output:

```text
Aditya Kumar Singh
```

### Multiple Fields

```python
from pydantic import BaseModel, field_validator

class User(BaseModel):
    age: int
    email: str

    @field_validator("age")
    @classmethod
    def validate_age(cls, value):
        if value < 18:
            raise ValueError("Must be adult")
        return value

    @field_validator("email")
    @classmethod
    def validate_email(cls, value):
        if "@" not in value:
            raise ValueError("Invalid email")
        return value
```

### Field() vs field_validator()

Use `Field()` for simple constraints:

```python
age: int = Field(ge=18)
temperature: float = Field(ge=0, le=2)
```

Use `field_validator()` for custom validation logic:

```python
@field_validator("email")
@classmethod
def validate_company_email(cls, value):
    if not value.endswith("@company.com"):
        raise ValueError("Company email required")
    return value
```

---

## 10. Enums

Useful when AI models must return specific categories.

```python
from enum import Enum
from pydantic import BaseModel

class Sentiment(str, Enum):
    positive = "positive"
    negative = "negative"
    neutral = "neutral"

class Review(BaseModel):
    sentiment: Sentiment

review = Review(sentiment="positive")
print(review)
```

Output:

```text
sentiment=<Sentiment.positive: 'positive'>
```

---

## 11. Loading Secret Keys from a .env File

```python
from pydantic_settings import BaseSettings, SettingsConfigDict
from openai import OpenAI

class Settings(BaseSettings):
    OPENAI_API_KEY: str
    DATABASE_URL: str

    model_config = SettingsConfigDict(
        env_file=".env"
    )

settings = Settings()

client = OpenAI(
    api_key=settings.OPENAI_API_KEY
)
```

---

## 12. API Contracts

```python
from pydantic import BaseModel
from fastapi import FastAPI

class SummaryRequest(BaseModel):
    text: str
    max_words: int

class SummaryResponse(BaseModel):
    summary: str
    word_count: int

app = FastAPI()

@app.post("/summarize")
def summarize(request: SummaryRequest) -> SummaryResponse:

    return SummaryResponse(
        summary="Short version",
        word_count=2
    )
```

Pydantic models are commonly used in FastAPI to define request and response schemas, providing automatic validation and API documentation.
