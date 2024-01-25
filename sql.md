# SQL COMMAND

## COALESCE
```
SELECT COALESCE(column1, column2, 'N/A') AS result
FROM your_table;
```

if column1 is null, it will check column2, and if both are null, it will return 'N/A'.
if column1 is not null then it return the column1

## Timezone conversion

### SQL
```
CONVERT_TZ(your_datetime_column, 'UTC', 'America/New_York') AS converted_datetime
```

### BIGQUERY
```
DATETIME(ic.changedOn, 'timeZone') AS Changed_On,
```


## ORDER BY and get the top one

### BIGQUERY
``
ARRAY_AGG(t ORDER BY changedON DESC LIMIT 1)[OFFSET(0)] AS ic`
```

```
WITH ranked_data AS (
  SELECT
    t.*
  FROM
    `your-table-name` t
)

SELECT
  ARRAY_AGG(t ORDER BY changedON DESC LIMIT 1)[OFFSET(0)] AS ic
FROM
  ranked_data t
WHERE
  row_num = 1
GROUP BY
  docId;
```

