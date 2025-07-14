# Keeping Systems in Sync - Change Data Capture (CDC)

Ways of implementing **Change Data Capture (CDC):**

&nbsp;&nbsp; **Timestamps on rows** - *last_updated* column.

&nbsp;&nbsp; **Version numbers on rows** - *version* column.

&nbsp;&nbsp; **Status indicators on rows** - *is_dirty* column.

&nbsp;&nbsp; **Time/Version/Status on rows** - a combination of the above three.

&nbsp;&nbsp; **DB triggers**
- Disadvantages:
    - Can be fragile
    - Performance overhead

&nbsp;&nbsp; **Transaction logs** - tools that scan and interpret the DB transaction log.
- Advantages:
    - No impact on the database
    - Transactional integrity
    - Fault tolerant (because it can be asynchronous)
    - Battle tested:
        - leader-follower replication relies on it
        - DB local atomicity and durability rely on this
- Disadvantages:
    - Quite “internal”. Some databases don’t even expose it.
    - Handling schema changes.

&nbsp;&nbsp; **Event sourcing** - adapting this approach at the application level.

&nbsp;&nbsp; **Distributed transactions - Two-Phase Commit (2PC)**

- Performance cost
- Operationally fragile

<br />

## Distributed Transactions vs Append-Only + Indepotence

<br />

**Distributed transactions disadvantages**:

- Performance cost
- Operationally fragile

**Append-only indepotent approach disadvantages**:

- Total ordering of events require a single leader node which all the events have to go through
- In microservice approach, where each service has its own storage, when two events originate from different services, there is no defined order for those events