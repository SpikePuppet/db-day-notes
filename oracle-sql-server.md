# Intro

- This covers RDS with DB2, Oracle, SQL Server
- Usual stuff about how moving to a managed database is good
- Can BYO license or license included model

# RDS Custom

- Feature of RDS which gives same capabilities of RDS, with stuff like elevated permissions and access to the OS.
- deployed in own account. Full access to the database system.
- Immense flexibility to what you want to do.
- ElastiCache - serverless offering
- Serverless can create a cache in under a minute

# ElastiCache Serverless

- Can build on a single paramtere
- Works on one endpoint, with a series of proxies which handle directing traffic to the correct node.

# Memory DB

- similar to elasticache - primary and replicas
- command passed to multi ax transaction log (proprietary). Ack'd when transaction is written.
- Data is replicated via transaction log and consumed by replicas
- GUaranteed delivery to replicas
