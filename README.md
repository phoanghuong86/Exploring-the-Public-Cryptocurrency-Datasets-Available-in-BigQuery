# Exploring-the-Public-Cryptocurrency-Datasets-Available-in-BigQuery

CREATE OR REPLACE TABLE lab.51 (transaction_hash STRING) as 
SELECT transaction_id FROM `bigquery-public-data.bitcoin_blockchain.transactions` , UNNEST( outputs ) as outputs
where outputs.output_satoshis = 19499300000000




CREATE OR REPLACE TABLE lab.52 (balance NUMERIC) as
WITH double_entry_book AS (
   -- debits
   SELECT
    array_to_string(inputs.addresses, ",") as address
   , -inputs.value as value
   FROM `bigquery-public-data.crypto_bitcoin.inputs` as inputs
   UNION ALL
   -- credits
   SELECT
    array_to_string(outputs.addresses, ",") as address
   , outputs.value as value
   FROM `bigquery-public-data.crypto_bitcoin.outputs` as outputs
)
SELECT   
sum(value) as balance
FROM double_entry_book
where address = "1XPTgDRhN8RFnzniWCddobD9iKZatrvH4"
