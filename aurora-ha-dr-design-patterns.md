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
