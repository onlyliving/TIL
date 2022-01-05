# FastAPI

-   [link](https://fastapi.tiangolo.com/ko/)

### 요구사항

-   Python 3.6+

### 설치

```bash
$ pip install fastapi

# ASGI 서버 설치
$ pip install uvicorn
```

### 기본 예제

-   `main.py` 파일 생성

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {"item_id": item_id, "q": q}

```

### 실행

```bash
$ uvicorn main:app --reload
```

-   자동 대화형 API 문서 확인 : http://127.0.0.1:8000/docs
-   대안 API 문서 : http://127.0.0.1:8000/redoc

---

## CORS

-   웹 페이지 url과 api url의 url 주소가 다르면 `CORS ERROR` 발생.
-   [link](https://fastapi.tiangolo.com/ko/tutorial/cors/?h=cor)

### Use CORSMiddleware

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost.tiangolo.com",
    "https://localhost.tiangolo.com",
    "http://localhost",
    "http://localhost:8080",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


@app.get("/")
async def main():
    return {"message": "Hello World"}
```
