# Clickhouse Compose

Some testing with clickhouse 


## Clickhouse

### BTC Dataset

#### Create table and insert data
```
CREATE TABLE btc.blockchain_btc_blocks
(
    `hash` String,
    `version` Int64,
    `mediantime` DateTime64(9),
    `nonce` Int64,
    `bits` String,
    `difficulty` Float64,
    `chainwork` String,
    `previousblockhash` String,
    `size` Int64,
    `weight` Int64,
    `coinbase_param` String,
    `number` Int64,
    `transaction_count` Int64,
    `merkle_root` String,
    `stripped_size` Int64,
    `timestamp` DateTime64(9),
    `date` String,
    `last_modified` DateTime64(9)
)
ENGINE = MergeTree
ORDER BY (number, hash)
SETTINGS index_granularity = 8192
```

```
create database btc
use btc
```

#### Import from Bucket
```
 insert into blockchain_btc_blocks select * FROM s3('https://aws-public-blockchain.s3.us-east-2.amazonaws.com/v1.0/btc/blocks/date=2023-07-24/*', 'Parquet') ;
```

#### Import from local files
```
aws s3 sync --no-sign-request 's3://aws-public-blockchain/v1.0/btc/blocks/' . 
```

```
INSERT INTO blockchain_btc_blocks
FROM INFILE '../btc_parquet/date=20*/*' SETTINGS input_format_parquet_allow_missing_columns = 1  FORMAT Parquet ;
```


## Jupyter Lab

Tail logs to get the url with token to login

### References 

* https://clickhouse.com/docs/en/integrations/jupysql
* https://jupysql.ploomber.io/en/latest/quick-start.html
* https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html

## SuperSet

We are using a simple image from `https://github.com/amancevice/docker-superset` and not the microservice version from apache superset itself.

After first start you need to run `docker exec -it superset superset-init`
