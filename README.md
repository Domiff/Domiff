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
| **Backend**       | Python · FastAPI · Django / DRF · Strawberry (GraphQL) · aiogram 3  |
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

### [blog](https://github.com/Domiff/blog) — blog API behind a GraphQL schema

A FastAPI service where REST covers only authentication and the whole domain —
posts, comments and profiles — is exposed through a single GraphQL schema.

- **DataLoaders against N+1** — related fields such as `Post.author`, `Profile.posts` and
  the comment counts batch every key requested at one level of the query into a single
  `IN (...)` or `GROUP BY` statement, so the number of SQL queries stays flat as the
  result grows.
- **Ownership checked per mutation** — a post or comment can only be edited, published or
  deleted by the profile that owns it, while queries stay public.
- **Tokens that can actually be revoked** — RS256 JWTs with a 15-minute access token and a
  30-day refresh, plus a Redis blocklist, so a logout takes effect at once instead of
  waiting out the expiry.
- **An explicit error boundary** — application errors carry a code in `extensions.code`,
  unexpected ones are masked and left only in the logs, and any request with errors rolls
  back its transaction.
- **Domain modules own their GraphQL layer** — `auth`, `profile` and `blog` each keep their
  models, repository, types, mutations, loaders and admin views together.

`FastAPI` · `Strawberry GraphQL` · `AuthX` · `JWT (RS256)` · `SQLAlchemy 2 (async)` ·
`Alembic` · `PostgreSQL` · `Redis` · `SQLAdmin`

---

### [realtime-messenger](https://github.com/Domiff/realtime-messenger) — messenger backend with live delivery

A FastAPI backend for chats: membership with roles, message history over REST and live
delivery over WebSockets, with a SQLAdmin back office on top.

- **Delivery through Redis pub/sub** — every frame is persisted as a message and published
  to a channel shared by the chat's members, and each member's socket forwards it on. No
  connection is tied to the process that accepted it, so the app scales past one worker.
- **Roles and membership as domain rules** — `owner`, `admin` or `member` per chat, chats
  that are private or group, and a second join answered with `409` rather than a duplicate
  row.
- **The WebSocket handshake carries the token in the query string** — browsers cannot set
  headers there, so the token is read from the URL and the socket joins the chat and
  subscribes on connect.
- **Rate limits tuned per endpoint** — 3 logins a minute against guessing, but a looser
  limit on refresh, because clients call it themselves and several open tabs would trip a
  tight one at once.
- **Every auth failure answers the same `401`** — a bad token, an unknown email and a wrong
  password are indistinguishable, so the API cannot be used to enumerate accounts. The
  refresh token stays in an httpOnly, secure, `samesite=strict` cookie and rotates on use.
- **Tests need no services** — the suite runs against in-memory SQLite built from the
  SQLAlchemy metadata, with payloads from Faker factories.

`FastAPI` · `WebSockets` · `Redis pub/sub` · `SQLAlchemy 2 (async)` · `Alembic` ·
`fastapi-guard` · `SQLAdmin` · `S3` · `PostgreSQL` · `pytest`

---

## Other Projects

### API Services

| Project | Description | Stack |
| ------- | ----------- | ----- |
| **[todo-api](https://github.com/Domiff/todo-api)** | Task manager API: Simple JWT with rotation and a token blacklist, separate web and Telegram registration flows behind one identity model, async CRUD over tasks and urgency-based categories, Swagger docs. Deployed behind Nginx over a Unix socket, with container logs collected by Promtail into Loki. | Django · DRF · Simple JWT · PostgreSQL · Nginx · Docker Compose · Grafana + Loki |
| **[crypto-api](https://github.com/Domiff/crypto-api)** | Deribit index price tracker: a Celery worker ingests prices on a schedule into PostgreSQL, with a read-only API on top. | FastAPI · Celery · RabbitMQ · PostgreSQL |
| **[reputation](https://github.com/Domiff/reputation)** | Reputation system with atomic point transfers between users and a full transfer log. Thin views, rules in a service, ORM queries isolated in a repository. | DRF · Simple JWT · drf-spectacular · PostgreSQL |
| **[site-recipes-api](https://github.com/Domiff/site-recipes-api)** | Recipe service with cookie sessions stored in Redis, CSRF protection and CRUD with categories. | FastAPI · Redis · PostgreSQL · Gunicorn |
| **[questions-answers-service](https://github.com/Domiff/questions-answers-service)** | Q&A REST API with OpenAPI documentation, fully dockerized. | DRF · PostgreSQL · Docker |

### Applications

| Project | Description | Stack |
| ------- | ----------- | ----- |
| **[book-shop](https://github.com/Domiff/book-shop)** | Book store with search, staff catalog management, authentication and a REST API. | Django · PostgreSQL · Docker |
| **[site-recipes-app](https://github.com/Domiff/site-recipes-app)** | Recipe application with user authentication. | Django · PostgreSQL |

### Bots

| Project | Description | Stack |
| ------- | ----------- | ----- |
| **[todo-bot](https://github.com/Domiff/todo-bot)** | Telegram client for [todo-api](https://github.com/Domiff/todo-api): multi-step forms built as finite state machines rather than a chain of message handlers, FSM state kept in Redis so a half-filled form survives a restart, and JWTs refreshed by the bot itself. | aiogram 3 · aiogram-dialog · Redis · Docker Compose |
| **[weather-bot](https://github.com/Domiff/weather-bot)** | Telegram bot for real-time forecasts via the OpenWeatherMap API. | aiogram 3 · aiohttp |
| **[tarot-cards](https://github.com/Domiff/tarot-cards)** | Telegram bot for the Rider–Waite–Smith tarot deck, with every card description, daily reading and history chapter authored in a separate admin panel. The bot and the admin run as independent processes sharing only the database layer; admin forms validate text against Telegram's 4096-character message limit. | aiogram 3 · SQLAdmin · SQLAlchemy 2 · Alembic · Redis · PostgreSQL |

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
