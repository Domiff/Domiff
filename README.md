# Dmitriy Levykin

**Backend Developer — Python**

I build web services and APIs: async-first applications with a clear separation
between transport, business logic and data access, covered by tests and shipped
in Docker. My focus is on predictable architecture, explicit contracts and code
that behaves the same in production as it does locally.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=redis&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat&logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)

---

## Technologies

| Area | Stack |
| ----------------- | ---------------------------------------------------------------------------- |
| **Backend**       | Python · FastAPI · Django / DRF · aiogram 3  |
| **Tasks**         | Celery · Taskiq · RabbitMQ                                          |
| **Data**          | PostgreSQL · Redis · SQLAlchemy 2 · Alembic · S3-compatible storage   |
| **Auth**          | JWT (RS256, refresh rotation) · fastapi-users · Simple JWT · cookie sessions · CSRF · RBAC |
| **Quality**       | pytest · httpx · ruff · pre-commit · structlog                 |
| **Infra**         | Docker Compose · Nginx · Gunicorn / Uvicorn · Grafana + Loki · uv    |

---

## Featured Projects

### [cafe](https://github.com/Domiff/cafe) — website and back office for a coffee shop

A full-stack FastAPI application: a public site plus an admin panel where every
piece of content is editable — menu, categories, prices, photos, landing copy,
the staff directory and the wording of every transactional email.

- **Two independent authentication contours** — JWT bearer tokens for customer API
  accounts (fastapi-users), and session cookies with role-based access for staff.
  Sections a role cannot open disappear from the sidebar entirely.
- **Transactional email** — verification, welcome and password reset — sent from a
  Taskiq worker over RabbitMQ, so a slow SMTP server never holds up a request.
  Message copy lives in the database and is edited without a deploy.
- **Two-level Redis caching** — rendered HTML per URL, invalidated on edit instead of
  waiting for a TTL, plus shared header/footer data injected via a context processor.
- **Image uploads to S3-compatible storage** — the database keeps object keys, the
  templates render full URLs.
- **Split by domain, not by file type** — `core` and `admin` hold no domain knowledge
  and lift into another project unchanged; each feature lives in one folder.

`FastAPI` · `SQLAlchemy 2 (async)` · `Alembic` · `SQLAdmin` · `fastapi-users` ·
`Taskiq` · `RabbitMQ` · `Redis` · `S3` · `Jinja2` · `PostgreSQL` · `Docker Compose`

---

### [todo-api](https://github.com/Domiff/todo-api) — task manager API

A Django REST API for task management — tasks with categories, deadlines and
completion status — reachable from the web or from Telegram, where
[todo-bot](https://github.com/Domiff/todo-bot) speaks to the same endpoints.

- **Two registration flows behind one identity model** — separate Telegram and web
  profiles, so the same person reaches the same tasks from either entry point.
- **JWT with rotation and blacklisting** — a refresh token is invalidated the moment
  it is exchanged, closing the replay window on a stolen token.
- **Async CRUD views** over tasks and urgency-based categories, documented with an
  auto-generated OpenAPI schema and Swagger UI.
- **Deployed as a full stack** — Nginx terminating SSL and serving static files in
  front of the application over a Unix socket, PostgreSQL behind it.
- **Centralized logging** — container logs collected by Promtail into Loki and read
  in Grafana, across every service in the compose file.

`Django` · `DRF` · `Simple JWT` · `drf-spectacular` · `PostgreSQL` · `Nginx` ·
`Docker Compose` · `Grafana + Loki + Promtail`

---

### [todo-bot](https://github.com/Domiff/todo-bot) — Telegram client for the task manager

A conversational front end to the same API: register, browse tasks, and create,
edit or delete them without leaving the chat.

- **Dialog-based UI** — multi-step forms for title, body, deadline and category built
  as finite state machines rather than a chain of ad-hoc message handlers.
- **FSM state in Redis**, so a user's half-finished form survives a bot restart.
- **Token lifecycle handled by the client** — the bot registers, refreshes and
  validates JWTs against the API, and re-authenticates transparently on expiry.
- **Layered structure** — each feature owns its router, handlers, windows, states and
  a service; HTTP access is confined to a single client with typed URL enums.

`aiogram 3` · `aiogram-dialog` · `Redis` · `pydantic-settings` · `Docker Compose` ·
`Grafana + Loki`

---

## Other Projects

### API Services

| Project | Description | Stack |
| ------- | ----------- | ----- |
| **[social-network](https://github.com/Domiff/social-network)** | Async social network API. RS256-signed JWTs — access token in the response body, refresh token in an httpOnly cookie that rotates on every use. Full async test suite. | FastAPI · SQLAlchemy 2 · Alembic · pytest · httpx |
| **[crypto-api](https://github.com/Domiff/crypto-api)** | Deribit index price tracker: a Celery worker ingests prices on a schedule into PostgreSQL, with a read-only API on top. | FastAPI · Celery · RabbitMQ · PostgreSQL |
| **[reputation](https://github.com/Domiff/reputation)** | Reputation system with atomic point transfers between users and a full transfer log. Thin views, rules in a service, ORM queries isolated in a repository. | DRF · Simple JWT · drf-spectacular · PostgreSQL |
| **[site-recipes-api](https://github.com/Domiff/site-recipes-api)** | Recipe service with cookie sessions stored in Redis, CSRF protection and CRUD with categories. | FastAPI · Redis · PostgreSQL · Gunicorn |
| **[questions-answers-service](https://github.com/Domiff/questions-answers-service)** | Q&A REST API with OpenAPI documentation, fully dockerized. | DRF · PostgreSQL · Docker |

### Applications and Bots

| Project | Description | Stack |
| ------- | ----------- | ----- |
| **[weather-bot](https://github.com/Domiff/weather-bot)** | Telegram bot for real-time forecasts via the OpenWeatherMap API. | aiogram 3 · aiohttp |
| **[book-shop](https://github.com/Domiff/book-shop)** | Book store with search, staff catalog management, authentication and a REST API. | Django · PostgreSQL · Docker |
| **[site-recipes-app](https://github.com/Domiff/site-recipes-app)** | Recipe application with user authentication. | Django · PostgreSQL |

---

## How I Work

I keep the transport layer disposable. A handler parses input and returns a response;
the rules live in a service and every query in a repository. In `cafe` the HTML pages
and the JSON API are separate router modules sitting on the same services — the
transport differs, the rules behind it do not.

Async is a decision about the workload, not a default. These services spend their time
waiting on sockets, so async pays for itself — and where a library is still blocking,
the boundary is explicit rather than a blocking call sitting unnoticed inside an async
handler.

Work that can be slow or fail on its own goes to a worker. Sending mail over SMTP or
pulling prices from an exchange should not decide how long a request takes, and a
retry there is cheaper than a failed response.

Cache invalidation is driven by the event that made the data stale. In `cafe` an edit
in the admin panel drops the affected page immediately; a TTL on its own would keep
serving the old version for as long as it lasts.

Auth is designed around a leaked token. Access tokens stay short-lived, refresh tokens
rotate and the spent one is blacklisted, and in the browser the refresh token lives in
an httpOnly cookie that page scripts cannot read.

Configuration comes from the environment and secrets never enter the repository. Every
project starts with one `docker compose up` and follows the same layout, so the second
one needs no explanation.

Logs are structured and collected centrally — reading them one container at a time
stops working the moment there is more than one container.

---

## Contact

- Telegram — [@TYGDYK](https://t.me/TYGDYK)
- Email — [dmlegasy@gmail.com](mailto:dmlegasy@gmail.com)
- GitHub — [Domiff](https://github.com/Domiff)
