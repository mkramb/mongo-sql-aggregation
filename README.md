# Mongo SQL Aggregation

### Input with custom DSL definition

```
SELECT
  first_name,
  last_name
FROM
  users
WHERE 
  joined_at >= ISODate(‘2020-01-30’) AND
  joined_at <= ISODate(‘2022-02-30’) AND   number_order > 10
ORDER BY
  num_orders DESC
```

### To generate following Mongo aggregation pipeline

```
db.users.aggregate([
  {
    $match: {
      "joined_at": {
        $gte: new ISODate( "2020-01-30"),
        $lte: new ISODate( "2020-02-30")
      }
  },
    num_orders: {
      $gt: 10
    }
  },
  {
    $sort: {
    "number_order" : -1
    }
  },
  {
    $project: {
    _id: 0,
      "first_name": 1,
      "last_name": 1
    }
  }
])
```