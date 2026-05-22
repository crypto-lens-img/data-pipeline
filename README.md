# 📡 Data Pipeline

Microservicio de ingesta de datos en tiempo real para CryptoLens.

## Responsabilidad

Único servicio que se comunica con fuentes externas. Recopila precios, noticias y sentimiento, calcula indicadores técnicos y publica eventos en Kafka.

## Fuentes de datos

| Fuente | Método | Datos |
|---|---|---|
| Binance WebSocket | WebSocket persistente | Precios en tiempo real |
| Binance REST API | HTTP | Datos históricos OHLCV |
| CryptoPanic API | HTTP | Noticias crypto |
| CoinDesk | Scraping | Noticias adicionales |
| Reddit PRAW | Librería oficial | Sentimiento r/Bitcoin |

## Stack

- `asyncio` + `httpx` — peticiones asíncronas
- `websockets` — conexión persistente con Binance
- `pandas` + `pandas-ta` — transformación e indicadores técnicos
- `SQLAlchemy` — ORM para PostgreSQL + TimescaleDB
- `kafka-python` — publicación de eventos

## Kafka Topics

```
prices     → {symbol, open, high, low, close, volume, timestamp}
news       → {source, title, url, published_at}
sentiment  → {subreddit, title, score, posted_at}
```

## Configuración

```bash
cp .env.example .env
# Edita .env con tus credenciales
docker compose up data-pipeline
```

## Parte de CryptoLens

[crypto-lens-img](https://github.com/crypto-lens-img) · [ml-engine](https://github.com/crypto-lens-img/ml-engine) · [llm-service](https://github.com/crypto-lens-img/llm-service) · [api-gateway](https://github.com/crypto-lens-img/api-gateway)
