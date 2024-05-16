# MongoDB example

1. Summary data: 

- Collection data:
  
  ```js
   {
    "_id": {
      "$oid": "6645708ed05a9a011ce73e89"
    },
    "timestamp": {
      "$date": {
        "$numberLong": "1715067240000"
      }
    },
    "metadata": {
      "callid": "30e0724f-3075-421a-94d8-83a4189daf34",
      "application": "autocall",
      "domain": "autocall.tpcoms.vn"
    },
    "callee": "0936369494",
    "caller": "02873030665",
    "duration": {
      "$numberInt": "21"
    },
    "billsec": {
      "$numberInt": "14"
    },
    "status": "answered"
  }
  ```
- Output: 

  ```js
  {
    "busy": 1,
    "not-available": 2,
    "answered": 27,
    "total": 31,
    "total_duration": 800,
    "total_billsec": 335,
    "no-answered": 1
  }
  ```

- pipeline:

  ```js
  [
    {
      $match:
        /**
         * query: The query in MQL.
         */
        {
          timestamp: {
            $gte: new Date("2024-05-07 14:00:00"),
            $lte: new Date("2024-05-07 15:00:00"),
          },
        },
    },
    {
      $facet: {
        total: [
          {
            $group: {
              _id: null,
              totalDoc: {
                $sum: 1,
              },
              totalDuration: {
                $sum: "$duration",
              },
              totalBillsec: {
                $sum: "$billsec",
              },
            },
          },
        ],
        status: [
          {
            $group: {
              _id: "$status",
              total: {
                $sum: 1,
              },
            },
          },
        ],
      },
    },
    {
      $project: {
        status: {
          $arrayToObject: {
            $map: {
              input: "$status",
              as: "item",
              in: {
                k: "$$item._id",
                v: "$$item.total",
              },
            },
          },
        },
        total: {
          $first: "$total.totalDoc",
        },
        total_duration: {
          $first: "$total.totalDuration",
        },
        total_billsec: {
          $first: "$total.totalBillsec",
        },
      },
    },
    {
      $replaceRoot: {
        newRoot: {
          $mergeObjects: [
            {
              total: "$total",
              total_duration: "$total_duration",
              total_billsec: "$total_billsec",
            },
            "$status",
          ],
        },
      },
    },
  ]
  ```
