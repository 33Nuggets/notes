# InfluxDB
InfluxDB is an open-source time series database developed by the company InfluxData. It is written in the Go programming language for storage and retrieval of time series data in fields such as operations monitoring, application metrics, Internet of Things sensor data, and real-time analytics.
https://www.influxdata.com/products/

## Quick Start with InfluxDB


```python
# initialize client
url = ''
token = ''
org = ''

client = influxdb_client.InfluxDBClient(
    url=url,
    token=token,
    org=org,
    ssl_ca_cert=certifi.where()  # <-- required as a fix to SSLError
)


bucket = 'bucket1'
# write records
write_api = client.write_api(write_options=SYNCHRONOUS)
   
for value in range(5):
  point = (
    Point("measurement1")
    .tag("tagname1", "tagvalue1")
    .field("field1", value)
  )
  write_api.write(bucket=bucket, org=org, record=point)
  time.sleep(1) # separate points by 1 second


# write large amount of records
    col_timestamp = 'datetime'
    cols_tag = ['stock_abbr', 'stock_name'] # <- tags are string type only
    cols_field = ['price', 'high', 'low']
    df = pd.read_csv('../data.csv')

    df = df[[col_timestamp] + cols_tag + cols_field]

    with client.write_api(write_options=SYNCHRONOUS) as write_api:
        write_api.write(
            bucket=bucket,
            record=df,
            org=org,
            # data_frame
            data_frame_measurement_name='table_name',
            data_frame_timesteamp_column=col_timestamp,
            data_frame_tag_columns=cols_tag,
            # remaining columns are identified as fields
        )


# query
# sends a Flux query
query = """from(bucket: "demo")
    |> range(start: -5d)
    |> filter(fn: (r) => r._measurement == "table_name" and r.stock_abbr == "AAPL")
    |> pivot(rowKey:["_time"], columnKey: ["_field"], valueColumn: "_value")
"""
#

df = query_api.query_data_frame(query, org=org)


# delete records
delete_api.delete(
    start=start, # <- start of time range
    stop=stop,   # <- stop of time range
    predicate='_measurement="AAPL"',
    bucket=bucket,
    org=org
)

```