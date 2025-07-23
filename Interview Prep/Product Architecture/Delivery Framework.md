
1. **Clarify requirements** (~5m)
	1. Functional requirements
		- Ask about user persona
	2. Non-functional requirements
		- Quantify (ask for specific numbers)!
		- Per use case. E.g. 'low latency' is far less meaningful than 'low latency search'
		- [Down the road] Calculate req/s, MB/s, storage requirements... ([estimation cheatsheet](https://www.hellointerview.com/blog/mastering-estimation))
2. **Core entities** (~2m)
	- What are the core entities that your API will exchange and your system will persist?
	- Examples: user, tweet, follow
3. **API / System interface / Contracts** (~2m)
	- `POST /v1/tweet`
		- `body: { "text": string }`
	- `GET /v1/tweet/:tweetId -> Tweet`
	- `POST /v1/follow/:userId
	- GET /v1/feed -> Tweet[]
	- Nice touches:
		- versioning
		- request ID
4. **High level design** (~10-15m)
	- Don't overthink this!
5. **Deep dives** (~10m)
	- Ensure all functional requirements are covered
	- Address edge cases (e.g. hash collisions in URL shortener)
	- Bottlenecks
	- Scalability, cache, sharding...
	- Observability
	- Security
	- Cost
	- **Give your interviewer room to ask questions and probe your design**
	- ...

![[Screenshot 2025-07-23 at 14.06.54.png]]

## Cheatsheet

- [Key technologies](https://www.hellointerview.com/learn/system-design/in-a-hurry/key-technologies)
- [Back of the envelope calculations](https://www.hellointerview.com/blog/mastering-estimation)
- [Common patterns](https://www.hellointerview.com/learn/system-design/in-a-hurry/patterns)
- 