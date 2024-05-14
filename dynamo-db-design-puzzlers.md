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

- I want to sample 1000 random items from my table, how?
  - Can scan table - iterative scan from top to bottom. Not efficient. It's in 1MB chunks
  - Can do parrallel scan - this is provided. Can define total segments and segment. Total segments can split table up tp 1m logical segments and start anywhere. Switch up the values to get good random sampling.
  - Should make sure to use limit here. Helps throttling.

## Main Dish - Puzzler

- I configured user id for my partition key because it has high cardinatlity, am I safe now?
  - Need to make sure to pick a high cardinality key for parition key
  - DeviceID works great in an IoT workload
  - Heavy users - generates more traffic than normal (think like 10000x traffic as an example).
  - Means we can get hot partitions which makes this whole thing redundant.
  - Choosing parition/sharing key is important. Means your traffic is distributed in a partiuclar way.
  - Reported that over 95% of customers the SA met was using the userID as the parition key.
  - Need to think about what you're using. Don't make it the only one you use.

## Main Dish - Single table design

- I am running on an IoT sevrice and want to provide the last 3 months of data to customer. Do I use single table or multiple tables on DynamoDB.
  - Can do single table, but nee to consider throughput (provisioned v on-demand), storage (standard v standard IA) with TTL enabled.
  - Standard IA is Standard Infrequent Access. no perf difference?
  - TTL is free. You'd have to manage it yourself in a traditional RDBMS
  - Multi table could be per month. Current month will have large write workload, will be hot
  - Remember - Access pattern matters a lot

## Main Dish - Is on demand load unlimited

- I created a table ousing on demand mode. A new large scale service will open next week, am I safe now?
  - Unlimited throughput. Base is 4000 write request units (4000 writes pr second), 12000 read request units (24000 eventually consistent reads).
  - Should consider account limits on maximum capacity.
  - Need 1M WCU/RCU, make sure you pre warm.
  - Can pre warm table to handle partition splitting better.
