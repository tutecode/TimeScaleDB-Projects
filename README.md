# TimeScaleDB-Projects
 Ark Invest ETFs Tracker

## What is Time Series Data?

- Tracks change over time, records have a timestamp and observations
- Stock price, CPU and memory usage, sensor data, ad views, COVID-19
- Can be captured at regular intervals (metrics) or irregular intervals (events)
- Immutability, ordered, append-only (INSERTs)
- Events whose value increases when you add time field (example: logins)
- Uses: pattern recognition, forecasting (trends, seasonality), anomaly detection

## Why it is important?

- 260% higher insert performance, up to 54x faster queries, and simpler implementation when using TimeScaleDB vs MongoDB for time-series data.
    - https://blog.timescale.com/blog/how-to-store-time-series-data-mongodb-vs-timescaledb-postgresql-a73939734016/
- Better Data Model, Query Language, and Reliability
    - https://blog.timescale.com/blog/timescaledb-vs-influxdb-for-time-series-data-timescale-influx-sql-nosql-36489299877/#:~:text=In%20our%20benchmark%2C%20TimescaleDB%20demonstrates,InfluxDB%20for%20high%2Dend%20scenarios. 
- Embraces SQL, not a SQL-Like language 
- One database for both relational and time-series data
- Built on top of PostgreSQL, open source for 25 years

## Why you might consider TimescaleDB?

- Fast ingestion of time-series data
- Time-oriented Analytics functions
- Continuous Aggregates
- Data retention policies
- Compression and Jobs
- Availability of PostgreSQL tools and ecosystem

## Why PostgreSQL?

- Battle-tested, scalable, fast, store anything
- Power of SQL, Aggregate, JOIN, on existing business data, triggers, relations
- Tooling - SQLAlchemy, Django ORM
- PostgreSQL extensions
- Can store JSON documents (eg. API response)
- Fully open-source, not owned by publicly traded company
- Used by many of the most successful companies.

## Why PostgreSQL?

- Get TimeScaleDB up and running with docker
- Populate a few tables with stock price data, we'll use ARK funds
- Use analytics functions like first(), last(), histogram(), time_bucket(), locf()
- Use features like continuous aggregates and retention policies
- Build a few simple FastAPI (+Tradekit) endpoints to serve our data as an API
- Use SQL to calculate some indicators and find chart patterns in data
- Use PostgreSQL stored procedures to create user-defined functions
- Plotly Dash, Data Visualization
- Crypto Edition of all of this