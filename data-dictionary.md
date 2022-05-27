
# Data dictinoary

## trade_v1 dataset
`trade_1` is the raw transaction data (`trade_v1`). Every row represents a single transaction.

```mermaid
erDiagram
  merchant {
   string nickname "merchant name" 
   string mid "merchant id"

    string risk_level_initial
    string risk_level_ongoing

    string business_hour "Business hour of the store"

    string mcc "mcc code"
    string mcc_name "name of mcc"
    string mcc_group "group short name of mcc"

    string sid "Store ID *store-level"
    string user "Store ID *store-level"

    string source "online / offline *store-level"
  }
```

```mermaid
erDiagram
  transaction {
    integer txamt "amount in decimal"
    integer amount "amount"
    timestamp sysdtm "transaction time"
    string wallet_region "wallet region of Alipay / WeChat pay"

    string syssn "transaction ID"
    string txn_type "collection|refund"

    string wallet "name of wallet, e.g. alipay"
    string busicd "wallet code"
    string busicd4 "first 4 digit of wallet code"

    boolean business_hour "transaction in business hour"
    string customer_id "customer id"

    string status "0=pending 1=success 2=fail"

    string udid "device ID|Shopify|Qiantai"
    string cardcd "customer identifier per wallet per walllet org"
  }
```

## trade_r2 dataset
`trade_r2` is the transaction data group by **ONE MINUTE** of the raw transaction data (`trade_v1`). 
`trade_r2` is an extended dataset **based on `trade_v1`**, more data, more custom columns are added.

```mermaid
erDiagram
  trade_r2 {
    integer win10m_amtgt100k_count "count of trx amount greater than 100k in 10-minute window"
    integer win10m_amtgt50k_count "count of trx amount greater than 50k in 10-minute window"

    integer win5m_count "count of trx in 5-minute window"

    datetime sysdtm_min "transaction time in minute"
  }
```

# Analyse idea

```mermaid
graph TD
    d[(Dataset HK)] --> dataset[(Dataset)]
    d2[(Dataset BJ)] --> dataset
    d3[(Dataset supporting data)] --> dataset

    subgraph Quicksight platform
      dataset --> qs["Analyse - Quicksight"]

      qs --> tl[Visual: top list]
      qs --> bd[Visual: break down Visual]
      qs --> others[Visual: Other analyse insight]

      tl -->|1. select filter, e.g. SID/MID, time| bd
      bd -.-> |"2. try out different threshold (e.g. 170%)"| bd

      bd -.-> adjust[\3. adjust filters/]
      adjust -.-> bd
      adjust -.-> tl
    end
```
