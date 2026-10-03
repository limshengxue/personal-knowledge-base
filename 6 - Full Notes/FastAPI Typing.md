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
	- `Optional` means the variable can be null like `name: Optional[str, None]` means the variable can be `str` or null
	- But FastAPI encourage using the union like `name : str | None` as it is more intuitive and can avoid mistaken as the variable is optional
- Pydantic model - data type declared as a class with attributes


# References
[[2 - Source Materials/Articles/FastAPI Typing|FastAPI Typing]]