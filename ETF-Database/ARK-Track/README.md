# Creating Tables with PostgreSQL and TimescaleDB

Track the holdings of ARK ETF funds in PostgreSQL

We write SQL statements to create our database tables.

Project from Part Time Larry: https://youtu.be/P-flYBbmCws

## Docs:

- https://use-the-index-luke.com/
- Creating Hypertables: https://docs.timescale.com/timescaledb/latest/how-to-guides/hypertables/

### 1. Create files "data" and "db"

### 2. Create "create_tables.sql" and code

### 3. Open Docker

```bash
sudo docker ps
```

```bash
sudo docker exec -it timescaledb bash
```

```bash
psql -U postgres
```

```bash
\d
```

```bash
\l
```

- Connect to database (db)

```bash
\c etfdb
```

- Execute

```bash
CREATE TABLE stock (
    id SERIAL PRIMARY KEY,
    symbol TEXT NOT NULL,
    name TEXT NOT NULL,
    exchange TEXT NOT NULL,
    is_etf BOOLEAN NOT NULL
);
```

```bash
etfdb=# \d

              List of relations
 Schema |     Name     |   Type   |  Owner
--------+--------------+----------+----------
 public | stock        | table    | postgres
 public | stock_id_seq | sequence | postgres
(2 rows)
```

- If we put "select \* from stock;" is empty

```bash
select * from stock;
```

- Insert a raw (AAPLE)

```bash
insert into stock (symbol, name, exchange, is_etf) values ('AAPL', 'Apple', 'NASDAQ', false);
```

- Insert a raw (QQQ)

```bash
insert into stock (symbol, name, exchange, is_etf) values ('QQQ', 'Nasdaq 100 ETF', 'NASDAQ', true);
```

```bash
etfdb=# select * from stock;

 id | symbol |      name      | exchange | is_etf
----+--------+----------------+----------+--------
  1 | AAPL   | Apple          | NASDAQ   | f
  2 | QQQ    | Nasdaq 100 ETF | NASDAQ   | t
(2 rows)
```

- Insert Microsoft

```bash
insert into stock (symbol, name, exchange, is_etf) values ('MSFT', 'Microsoft', 'NASDAQ', false);
```

- Insert new TABLE

```bash
CREATE TABLE etf_holding (
    etf_id INTEGER NOT NULL,
    holding_id INTEGER NOT NULL,
    dt DATE NOT NULL,
    shares NUMERIC,
    weight NUMERIC,
    PRIMARY KEY (etf_id, holding_id, dt),
    CONSTRAINT fk_etf FOREIGN KEY (etf_id) REFERENCES stock (id),
    CONSTRAINT fk_holding FOREIGN KEY (holding_id) REFERENCES stock (id)
);
```

- Insert data raw

```bash
insert into etf_holding (etf_id, holding_id, dt, shares, weight) values (2, 1, current_date, 1, 2);
```

- View table

```bash
select * from etf_holding;
```

- Insert data raw

```bash
insert into etf_holding (etf_id, holding_id, dt, shares, weight) values (2, 3, current_date, 1, 2);
```

# Alpaca API, PostgreSQL, and TimescaleDB

We use the Alpaca API to populate a PostgreSQL ETF database.

Project from Part Time Larry: https://youtu.be/RAIqlK5K7-s

## Docs:

- https://magicstack.github.io/asyncpg/current/
- https://docs.aiohttp.org/en/stable/

- Open terminal and code:

```bash
sudo docker ps
```

```bash
sudo docker start timescaledb
```

- Connect to TablePlus

- Create Alpaca account

- Create 'config.py' file and put APIs

- Open terminal and install 'alpaca_trade_api'

```bash
pip3 install alpaca_trade_api
```

- Create 'requirements.txt' file and install 'alpaca_trade_api', 'psycopg2', '...', '...'

```bash
pip install -r requirements.txt
```

- Go to TablePlus -> "SQL Query" and delete db.

```bash
delete from etf_holding;
```

```bash
delete from stock;
```

- Create 'populate_stocks.py' or 'main.py' and code. Run.

```bash
import config
import alpaca_trade_api as tradeapi
import psycopg2
import psycopg2.extras

connection = psycopg2.connect(
    host=config.DB_HOST, database=config.DB_NAME, user=config.DB_USER, password=config.DB_PASS)

cursor = connection.cursor(cursor_factory=psycopg2.extras.DictCursor)

api = tradeapi.REST(config.API_KEY, config.API_SECRET, base_url=config.API_URL)

assets = api.list_assets()

for asset in assets:
    print(f'Inserting stock {asset.name} {asset.symbol}')
    cursor.execute('''
      INSERT INTO stock (name, symbol, exchange, is_etf)
      VALUES (%s, %s, %s, false)
      ''', (asset.name, asset.symbol, asset.exchange))

connection.commit()
```

- Go to TablePlus -> "SQL Query".

```bash
select * from stock where exchange = 'NASDAQ' order by symbol desc;
```

# Tracking ARK Invest ETFs with Python and PostgreSQL

We parse the ARK ETF datasets across multiple days, load the data into PostgreSQL, and begin writing SQL queries. What stock did ARK add on January 26th, 2021? What stock did they reduce exposure to?

Project from Part Time Larry: https://youtu.be/5uW0TLHQg9w

- Download data from ARK Invest and put into 'data' file

- Go to TablePlus in "SQL Query" code:

```bash
UPDATE stock SET is_etf = TRUE
WHERE symbol IN ('ARKK', 'ARKQ', 'ARKG', 'ARKX', 'ARKF', 'ARKW', 'IZRL', 'PRNT')
```

```bash
select * from stock where is_etf = True;
```

- Create 'populate_etfs' and code:

- Run.

```bash
ARKW
ARKX
ARKF
ARKG
IZRL
PRNT
ARKK
ARKQ
```

- Go to TablePlus in "SQL Query" code:

```bash
select * from etf_holding;
```

- Specific date

```bash
select * from etf_holding where dt = '2021-01-26';
```

```bash
-- select * from stock where id = 7315;

-- select * from etf_holding where etf_id = 7315;

select * from etf_holding where etf_id = 7315 and dt= '2021-01-26';
```

- New holding (compare dates)

```bash
select holding_id
from etf_holding
where dt = '2021-01-26'
and holding_id NOT IN (select distinct(holding_id) from etf_holding where dt = '2021-01-25'
)
```
