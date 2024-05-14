# Start

- Need patterns of resilience to handle issues and balancing that with cost and complexity
- how to make system resilient with aurora
- Resilience - ability to recover from depency disruptions, acquire and release compte resources, mitigate transitent issues
- supported by availability and disaster recovery

# Availability

- Proportion of time a worklaod is available for use during a time period
- Adding redundancy is best way to ensure this. Avoids single point of failure

# Disaster Recvery (DR)

- Techniques to recover from a one off destructive event. RTO - Recovery Time Objective max length of oe impact, RPO - Recoverty Point Objective

- Don't need to provision storage, does it automatically up to 128TB

- One AZ fails - no impact on storage
- Writes make 6 copies, and wait for 4 acknowledgements to come back to say it's done
- Reads - as long as there's three copies across AZ's you're good

- Just write log records - just instructions on how to change the database (high level idea). That's what gets written.
- Storage understands these records, so it knows what to do with it
- They're applied to DB page, which is then read as normal.
- Called coalescing

- Backups are stored in S3.

- Restore

  - Brings back latest version of pages
  - Brings back all the logs up to the latest time, and squashes them together.
  - Handles rolling back any incoming transactions
  - Happens in storage and parallel

- Clones
  - Pointers all the way down
  - Means cloning can happen **super fast**
  - When writing to clone, break pointer and write to clone storage. Then can read directly.
  - When writing to original, clone old page for clone, and create new page for original.
  - new writes work as normal

# Everything Fails, all the time

# Multi AZ

- Great way to failover when something goes wrong
- Primary keeps replicas in sync, and can fail over if something goes wrong
- Easy to spin up Read replica, and we just say how much to send to it.
- Replica isn't perfectly in sync, but it's usually within milliseconds
- Can add up to 15 replicas

# Mixed cluster?

- Can add replicas which are different sizes or even serverless
- Can specify the order to failover to support this. Called Tiers.
- Tier 0 is the first, exhaust all then move to 1, etc
