# DynamoDB-GSI-Based-Search-API
DynamoDB GSI-Based Search API is a serverless backend service built on AWS that enables fast, scalable search and range queries on transactional data without relying on expensive table scans.

The project demonstrates real-world DynamoDB data modeling, Global Secondary Index (GSI) design, and AWS Lambda–based APIs to efficiently query records by date, month, and date ranges.

This project is designed with performance, cost efficiency, and scalability in mind and stays within the AWS Free Tier.

## 🏗️ Architecture

```
Client / API Consumer
        |
        |  HTTP Requests
        v
Amazon API Gateway
        |
        v
AWS Lambda (Node.js)
        |
        v
Amazon DynamoDB
   ├── Primary Table
   └── Global Secondary Index (GSI)
```
## Services Used
* AWS Lambda – Serverless compute for business logic
* Amazon API Gateway – RESTful API interface
* Amazon DynamoDB – NoSQL data store
* Global Secondary Index (GSI) – Optimized search and range queries

## 🗄️ Data Model
DynamoDB Table: search-data
Primary Key
| Attribute         | Type   | Description                     |
| ----------------- | ------ | ------------------------------- |
| `transactionID`   | String | Unique transaction identifier   |
| `date` (Sort Key) | String | Transaction date (`YYYY-MM-DD`) |


Additional Attributes

* customerName
* customerType
* fosName
* mobile
* aid
* time
* yearMonth (YYYY-MM)

## 📈 Global Secondary Index (GSI)
GSI Name: yearMonth-date-index

| Key Type      | Attribute   | Purpose                    |
| ------------- | ----------- | -------------------------- |
| Partition Key | `yearMonth` | Groups data by month       |
| Sort Key      | `date`      | Enables date range queries |

Why This GSI?

* Enables monthly queries
* Supports date-range searches
* Eliminates full table scans
* Scales efficiently with large datasets

## 🚀 API Endpoints
### 1️⃣ Query All Records for a Month
Request : ``` GET /demo?yearMonth=2026-01 ```

##### Description : Returns all records for the given month using the GSI.


### 2️⃣ Query Records by Date Range (Recommended)
Request : ```GET /demo?yearMonth=2026-01&from=2026-01-01&to=2026-01-07```

##### Description : Fetches records between two dates using a GSI range query (BETWEEN).


### 3️⃣ Get Record by Transaction ID
Request : ```GET /demo?transactionID=2026-01-07-9618924786-MOHAMMED-SOHAIL```

##### Description : Fetches a single transaction using the primary key.



## 🧠 Query Strategy

| Operation        | DynamoDB Method         | Reason               |
| ---------------- | ----------------------- | -------------------- |
| Get by ID        | `GetItem`               | Fastest, key-based   |
| Query by month   | `Query (GSI)`           | Indexed access       |
| Date range query | `Query (GSI + BETWEEN)` | Efficient range scan |
| Full table read  | `Scan` (admin only)     | Avoid in production  |

⚠️ Scan is avoided in production paths due to cost and performance impact.

## 🧩 Implementation Highlights

* Uses AWS SDK v3 (@aws-sdk/lib-dynamodb)
* Modular Lambda code with multiple files
* Safe updates using UpdateCommand
* Pagination handled for large datasets
* Defensive checks for missing query parameters
* ISO date formats for predictable sorting

## 🔐 Security & Cost Considerations

Security
* IAM role scoped to required DynamoDB actions
* No hard-coded credentials
* API Gateway handles request routing
  
Cost
* Designed to stay within AWS Free Tier
* Avoids table scans in user-facing APIs
* GSIs used only where necessary

## 🛠️ Local Development Notes

* Runtime: Node.js 18+
* Module system: ES Modules (.mjs)
* Uses import / export consistently
* No external dependencies beyond AWS SDK

## 📌 Future Enhancements
* Pagination support in API responses
* Authentication (API key / Cognito)
* Additional GSIs for agent-based queries
* Sorting order toggle (ASC / DESC)
* CloudWatch metrics & alarms
* Infrastructure as Code (Terraform / CloudFormation)


## ✅ Key Learnings

* DynamoDB access patterns must be designed before implementation
* GSIs are essential for scalable search queries
* Range queries only work on sort keys
* Serverless architecture simplifies scalability and cost control


## 🏁 Conclusion
This project demonstrates production-ready DynamoDB design, serverless backend architecture, and AWS best practices for building scalable, cost-efficient APIs.
		
