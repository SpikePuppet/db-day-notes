# Introduction

- Interesting puzzler questions that the SA's face each day with customers
- This is cool - more an interactive style talk. Get to chew on the pizzle

# Starter Puzzlers

- See 10K requests per second, but consuming 20000 WCUs, Why?

  - Not due to being over 1kb write
  - Could indexes do soemthing here?
    - Dynamo DB has GSI (Global Secondary Index) - separate table with partition key and sort key. hence why it consumes resources separately
    - LSI - local seconary index. Putting data into base table. Does consume WSU and RSU.
  - Consuming 20 because one WSU for base table, one for LSI.
  - Use GSI instead of LSI in most cases - LSI is great for strongly consistent read.

- ## I want to sample 1000 random items from my table, how?
