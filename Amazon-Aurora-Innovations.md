# General

- This was the deep dive session
- Apparently delivers 5x throughput of mysql and 3x of postgres
- Managed Service - obv
- Secure, Encryption in transit and rest + multiple auth options
- Architecture
  - Usually, everything is stored in one monolithic stack (SQL, transaction, cache etc)
  - Aurora decouples storage and comute
  - Purpose build log structured distributed storage
  - Storage volume striped acros hundres of storage nodes
  - Just writes log streams - that's kind of neat!
  - Aurora writes data into 10 GiB (?) protection groups, up to 128
  - Uses quorum model for writes and reads (4/6 for writes, 3/6 for reads in recovery)
  - Maintains write if AZ fails, maintains read is AZ + 1 storage nod fails
  - Does gossiping - copies data to healthy nodes
  - Up to 16 DB instances/nodes in a regional cluster over multiple AZs
  - One primary writer
  - Any node can be promoted to a writer if something goes wrong
  - Failover tier determines failover candidate - lower = better
  - Storage log structured - doesn't flush datapages or full blocks to storage
  - Storage node - incoming logs get put on the queue, which then is durably logged to a hot log an then it's acknowledged, which is then put into an update page and coalecses into a page

# Clones

- Fast clones? - Clones are just creating pointers to the originals (clone to original). Writes happen on independent storage, aswell as new pages
- Updates on the original are pushed into the clone storage.
- Only pay for the storage you use for clones! Neat!

# Global Databases

- Primary region - replication server, other regions have replication agents which handle bringing in any writes to the new environment. Can spin up read replicas in other regions aswell.
- Can do managed failover

# New storage?

- Charges
  - Reads hitting cache has no cost
  - in mem - 8k/16k 0.000001 cent (or something like that, stupid cheap)
  - Replication happens for free, just pay for inital write (write log, 4@1k)
  - I/O optimised prdicatbale price for all workloads
  - Best for I/O intensive workloads. >25% of Aurora spend is I/0 - that's where it's better
    - Is a cluster storage configureation
    - Can go from standard to optimised once a month
    - Postgres 13 and up, MySQL 3.0.3.1

# pg_vector extension

- VEctor was available for a while in pg, but not for machine learning
- Extension for storage, indexing, searching and metadata with choice of distance. Basically manage using vectors for ML/AU

# Aurora Serverless - Managability

- Great for not managing the database yourself
- Instant scaling - scale in place in ms by adding more comp resources
- scaling occurs in fine grained capccity adjustments (0.5 ACUs)
- No impact due to scaling even when running big workloads

# Manageability - Zero ETl

- Aurora to Redshift
- Enhanced binlog

# Optimised reads

- When you create an instance an EBS store gets associated with it for ephemeral storage
- Tiered caching with optimised memory (if you use disk based compute instances)
