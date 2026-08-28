# Library Documentation References

Quick reference links for preferred Python libraries.

## Core Libraries

| Library | Purpose | Documentation |
|---------|---------|---------------|
| Click | Argument parsing / CLI | https://click.palletsprojects.com/ |
| Arrow | Date/time handling | https://arrow.readthedocs.io/ |
| SQLAlchemy | Database ORM | https://docs.sqlalchemy.org/ |
| Alembic | Database migrations | https://alembic.sqlalchemy.org/ |

## Async Web Frameworks

| Library | Best For | Documentation |
|---------|----------|---------------|
| FastAPI | APIs with auto OpenAPI docs, type validation | https://fastapi.tiangolo.com/ |
| Quart | Async Flask alternative, traditional web apps | https://quart.palletsprojects.com/ |
| AioHttp | High-performance services, lower-level control | https://docs.aiohttp.org/ |

## Choosing a Web Framework

- **FastAPI** — Choose when building APIs, especially with Pydantic models. Automatic OpenAPI/Swagger docs. Best developer experience for REST APIs.
- **Quart** — Choose when porting Flask apps to async, or when you want Flask-like simplicity with async support. Good for traditional server-rendered apps.
- **AioHttp** — Choose when you need maximum control over the HTTP layer, high concurrency, or are building a service that acts as both client and server.
