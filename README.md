# Hi, I'm Dmitriy 👋

**Python backend developer** building REST APIs and Telegram bots. I care about clean
architecture (router → service → repository), async-first code, tests, and shipping
projects that run in Docker, not just on my machine.

## 🛠 Tech Stack

| | |
| ------------ | ------------------------------------------------------------ |
| **Backend**  | Python · FastAPI · Django / DRF · aiogram 3 · Celery          |
| **Data**     | PostgreSQL · Redis · SQLAlchemy 2 (async) · Alembic · RabbitMQ |
| **Auth**     | JWT (RS256, refresh rotation) · cookie sessions · CSRF · argon2/bcrypt |
| **Quality**  | pytest (async) · httpx · ruff · pre-commit · structlog        |
| **Infra**    | Docker Compose · Nginx · Gunicorn/Uvicorn · Grafana + Loki    |
| **Frontend** | Vue 3 · TypeScript · Pinia · Vuetify                          |

## 🚀 Featured Projects

### APIs
- **[social-network](https://github.com/Domiff/social-network)** — async social network API:
  FastAPI, JWT (RS256) with refresh token rotation, SQLAlchemy 2, Alembic, structured
  logging, full async test suite (pytest + httpx + Faker).
- **[site-recipes-api](https://github.com/Domiff/site-recipes-api)** — recipe service with
  cookie sessions in Redis, CSRF protection, recipe CRUD with categories; SQLite/Postgres,
  Docker, Gunicorn.
- **[crypto-api](https://github.com/Domiff/crypto-api)** — Deribit index price tracker:
  Celery worker polls prices every minute into PostgreSQL (RabbitMQ broker), read-only
  FastAPI on top. Vue 3 client: [crypto-api-frontend](https://github.com/Domiff/crypto-api-frontend).
- **[todo-api](https://github.com/Domiff/todo-api)** — Django 6 + DRF task manager: Simple JWT
  with token blacklist, registration via web **and** Telegram, Swagger docs, deployed with
  Docker Compose + Nginx, logs shipped to Grafana/Loki.
  Vue 3 client: [todo-frontend](https://github.com/Domiff/todo-frontend).
- **[questions-answers-service](https://github.com/Domiff/questions-answers-service)** — Q&A
  REST API on DRF + PostgreSQL with OpenAPI docs, fully dockerized.

### Telegram Bots
- **[todo-bot](https://github.com/Domiff/todo-bot)** — task management in a dialog-based UI
  (aiogram 3 + aiogram-dialog).
- **[weather-bot](https://github.com/Domiff/weather-bot)** — real-time forecasts via
  OpenWeatherMap (aiogram 3 + aiohttp).

### Web Apps
- **[book-shop-app](https://github.com/Domiff/book-shop-app)** — Django book store: search,
  staff catalog management, auth, REST API, Docker.
- **[site-recipes-app](https://github.com/Domiff/site-recipes-app)** — Django recipe app with
  user authentication.

## 📫 Contact

- Telegram: [@TYGDYK](https://t.me/TYGDYK)
- Email: [dmlegasy@gmail.com](mailto:dmlegasy@gmail.com)

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
