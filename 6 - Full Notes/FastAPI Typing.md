2025-11-02 10:35

Tags: [[fastapi]]

# FastAPI Typing
## Motivation
- FastAPI encourage type hint with 2 motivations
	- Provide auto-completion in editor to know what attributes/methods are available
	- Provide error hinting for type related error
- FastAPI leverage type declarations for
	- **Define requirements**: from request path parameters, query parameters, headers, bodies, dependencies, etc.
	- **Convert data**: from the request to the required type.
	- **Validate data**: coming from each request:
	    - Generating **automatic errors** returned to the client when the data is invalid.
	- **Document** the API using OpenAPI:
	    - which is then used by the automatic interactive documentation user interfaces.

## Common Types
- Simple type like `int` `str`
- Generic type like `list` `tuple` `dict`
	- `Optional` means the variable can be null like `name: Optional[str]` means the variable can be `str` or null
	- But FastAPI encourage using the union like `name : str | None` as it is more intuitive and can avoid mistaken as the variable is optional
- Pydantic model - data type declared as a class with attributes


## Nullable vs Optional Input
- `Optional[str]` and `str | None` allow the value `None`.
- A default such as `= None` determines whether the caller can omit the argument; nullable typing alone does not make input optional.
- Plain Python annotations do not perform runtime validation by themselves; FastAPI and Pydantic interpret them.

# References
[[2 - Source Materials/Articles/FastAPI Typing|FastAPI Typing]]
[Python Optional](https://docs.python.org/3/library/typing.html#typing.Optional)
