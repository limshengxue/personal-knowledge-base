2025-11-02 13:55

Tags: [[fastapi]]

# FastAPI Dependencies
## [[Dependency Injection]]
- There is a way for code to declare things that it requires to work and use (dependencies)
- The framework (FastAPI) take care of doing whatever is needed to provide the code with those needed dependencies
- Useful when
	- Have shared logic
	- Share database connection
	- Enforce security
	- Etc

## Creating Dependent
- We can create and inject dependent using the `Annotated` typing
- The dependent can take in the argument that path operation function take
```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


CommonsDep = Annotated[dict, Depends(common_parameters)]


@app.get("/items/")
async def read_items(commons: CommonsDep):
    return commons


@app.get("/users/")
async def read_users(commons: CommonsDep):
    return commons
```

### Class Dependent
- Dependent can be anything callable including `class`
- Class Dependent give us typing feature compare to function

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


class CommonQueryParams:
    def __init__(self, q: str | None = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit


@app.get("/items/")
async def read_items(commons: Annotated[CommonQueryParams, Depends(CommonQueryParams)]):
    response = {}
    if commons.q:
        response.update({"q": commons.q})
    items = fake_items_db[commons.skip : commons.skip + commons.limit]
    response.update({"items": items})
    return response
```

## Singleton
- By default, dependency referred multiple times is only called once
- We can set `use_cache` to True to disable reused
```
async def needy_dependency(fresh_value: Annotated[str, Depends(get_value, use_cache=False)]): 
return {"fresh_value": fresh_value}
```


## Dependent in Decorator
- We declare dependent in decorator when we don't need its return value
- This avoid confusion and linting error caused by unused argument
```python
from typing import Annotated

from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()


async def verify_token(x_token: Annotated[str, Header()]):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")


async def verify_key(x_key: Annotated[str, Header()]):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key


@app.get("/items/", dependencies=[Depends(verify_token), Depends(verify_key)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]
```


## Dependent with `yield`
- Used when we need extra code execution after returning the thing that we want to inject
```python
async def get_db():
    db = DBSession()
    try:
        yield db ## inject this db to the function
    finally:
        db.close()
```

# References
[[FastAPI Dependencies]]
