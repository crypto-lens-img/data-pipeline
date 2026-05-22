# Data Pipeline Service

## Responsabilidad
Unico servicio que se comunica con fuentes externas de datos.
Recopila, limpia y publica datos en Kafka.
Nunca expone endpoints HTTP, solo produce eventos.

## Fuentes de datos
- Binance WebSocket: precios en tiempo real (OHLCV)
- Binance REST API: datos historicos para entrenar el modelo
- CryptoPanic API: noticias crypto agregadas
- CoinDesk: scraping con BeautifulSoup4
- Reddit PRAW: sentimiento de r/Bitcoin y r/CryptoCurrency

## Kafka Topics que produce
- prices: {symbol, open, high, low, close, volume, timestamp}
- news: {source, title, url, published_at}
- sentiment: {subreddit, title, score, posted_at}

## Base de datos - Tablas que gestiona
- crypto_prices (TimescaleDB hypertable)
- crypto_indicators (TimescaleDB hypertable)
- news
- reddit_posts

## Stack
- asyncio + httpx: peticiones asincronas
- websockets: conexion persistente con Binance
- PRAW: libreria oficial Reddit
- BeautifulSoup4: scraping CoinDesk
- pandas + pandas-ta: transformacion de datos e indicadores tecnicos
- SQLAlchemy: ORM para escribir en PostgreSQL
- kafka-python: publicar eventos en Kafka

## Estructura de carpetas esperada
data-pipeline/
  src/
    sources/
      binance_ws.py
      binance_rest.py
      cryptopanic.py
      coindesk.py
      reddit.py
    processors/
      indicators.py
    publishers/
      kafka_producer.py
    db/
      models.py
      session.py
  main.py
  requirements.txt
  Dockerfile
  .env.example

## Variables de entorno necesarias
- BINANCE_API_KEY
- CRYPTOPANIC_API_KEY
- REDDIT_CLIENT_ID
- REDDIT_CLIENT_SECRET
- DATABASE_URL
- KAFKA_BOOTSTRAP_SERVERS
