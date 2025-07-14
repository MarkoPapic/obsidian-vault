# Requirements Clarification

At the beginning of system design interview, you should clarify the following:
* **users**
* **functional requirements**
* **non-functional requirements**
* **cost**

**Users**
* Who is going to use this system?
* How are they going to use it?

**Functional requirements**:
* What are the functionalities (inputs, outputs)?

**Non-functional requirements**
* Load:
    * How many reads per second?
    * How many concurrent reads?
    * How many writes per second?
    * How many concurrent writes?
    * Request/response size
    * Read/write ratio
    * Is the load steady, or should we expect spikes?
* Performance:
    * Expected write-to-read delay?
    * Expected p99 latency for reads?
    * Expected p99 latency for writes?
    * ... TODO
* Availability - Should the system be resistant to hardware/network failures?
* Latency - In whate regions accross the world should the system be available?

