
# Data dictionary

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
    string userid "Store ID *store-level" 

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

## trade_r5_week dataset (weekly grouped ONLY for demo purpose)
`trade_r5_week` is the transaction data group by **ONE WEEK** of the raw transaction data (`trade_v1`). 
`trade_r5_week` is an extended dataset **based on `trade_v1`**, more data, more custom columns are added.

** the correct dataset `trade_r6` will be added.
`trade_r6` is the transaction data group by **ONE DAY** of the raw transaction data (`trade_v1`).

```mermaid
erDiagram
  trade_r5_week {
    integer count "count of trx in week (per MID dimension)"
    integer amount "sum of trx amount in week (per MID dimension)"

    integer win1week_avg_ticket "average ticket size last week"
    integer win1week_avg_ticket_Npc "mutiplied threshold of average ticket size last week"
    integer win1week_avg_ticket_Npc_pcdiff "percentage difference of the mutiplied threshold of average ticket size last week with current"

    integer win1week_count "count of trx last week"
    integer win1week_count_Npc "mutiplied threshold of count of trx last week"
    integer win1week_count_Npc_pcdiff "percentage difference of the mutiplied threshold of average ticket size last week with current"

    integer win1week_amount "total trx amount last week"
    integer win1week_amount_Npc "mutiplied threshold of total trx amount last week"
    integer win1week_avg_ticket_Npc_pcdiff "percentage difference of the mutiplied threshold of total trx amount last week with current"
  }
```

## trade_r6 dataset
`trade_r6` is the transaction data group by **ONE DAY** of the raw transaction data (`trade_v1`).
`trade_r6` is an extended dataset **based on `trade_v1`**, more data, more statistic columns in 1-month window are added.

```mermaid
erDiagram
  trade_r6 {
    datetime sysdtm_day "txn time truncated in day, e.g. 2022-05-23 00:00:00"

    integer daily_ticket_size "current day ticket size"
    integer win1mo_daily_ticket_size "average daily ticket size in last 1-month window"
    integer win1mo_daily_ticket_size_Npc "mutiplied threshold of `win1mo_daily_ticket_size`"
    integer win1mo_daily_ticket_size_Npc_pcdiff "percentage difference of win1mo_daily_ticket_size vs win1mo_daily_ticket_size_Npc"

    integer daily_gmv "current day GMV"
    integer win1mo_daily_gmv "average daily GMV in last 1-month window"
    integer win1mo_daily_gmv_Npc "mutiplied threshold of `win1mo_daily_gmv`"
    integer win1mo_daily_gmv_Npc_pcdiff "percentage difference of win1mo_daily_gmv vs win1mo_daily_gmv_Npc"

    integer dailycount "no of daily txn in current day"
    integer win1mo_dailycount "no of daily txn in last 1-month window"
    integer win1mo_dailycount_Npc "mutiplied threshold of `win1mo_dailycount`"
    integer win1mo_dailycount_Npc_pcdiff "percentage difference of win1mo_dailycount vs win1mo_dailycount_Npc_pcdiff"
    integer daily_ticket_size_allmerchant "current day ticket size of all merchants"
    integer daily_ticket_size_allmerchant_NPC "mutiplied threshold of `daily_ticket_size_allmerchant`"
    integer daily_ticket_size_allmerchant_NPC_pcdif "percentage difference of daily_ticket_size_allmerchant vs daily_ticket_size_allmerchant_NPC"
  }
```

## trade_r15 dataset
`trade_r15` is the transaction data grouped by same amount of the raw transaction data (`trade_v1`).
```mermaid
erDiagram
  trade_r15 {
    datetime sysdtm_day "txn time truncated in day, e.g. 2022-05-23 00:00:00"

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
