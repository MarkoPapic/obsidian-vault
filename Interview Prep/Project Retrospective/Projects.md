
## Build Cloud

### Setting direction
* **Business acumen**: Parts of company strategy were:
	* multi-product revenue
	* expand into cloud industry and offer cloud-based products
* **Problem space**: An average developer spends 1h per day waiting for builds to finish
* **Build cloud**:
	* Powerful infrastructure specialized for builds
	* Shared caching
	* DD integration
* **Built the team**
	* We needed people with SaaS experience
	* We needed people with low level Buildkit and Engine experience
* **Setting success metrics**: Partnered with product manager
	* North star: Number of cached builds: 80% builds with at least 1 cached layer
	* Health metrics:
		* Cold start: Up to 3 seconds
		* Isolated compute
		* Availability: 99.99%
		* Reliability: Number of successful builds: 99.99%
		* Incident response: acknowledge within 15 minutes (on-call)
	* Business metrics: Our OKRs are tied into company OKRs
		* E.g. multi-product ARR
* **Driving success metrics**
	* Looker analytics: Reviewing with PM and PMM weekly
		* Builds, orgs, cached layers...
		* Top customers
		* Usage within org (e.g. champion + onboarded other members)
		* Revenue
	* Grafana (OTel) + BugSnag + Swarmia: Weekly operations review
		* Successful builds
		* Availability + Reliability
		* DORA
	* Alerting: Slack alerts
* **Tools**:
	* Google sheet: Roadmap and resourcing plan
	* Jira: Backlog
	* Looker: Data analytics, funnel
	* Grafana + OTel: Engineering observability, SLOs
	* Swarmia: DORA
	* Jira dashboard for CSESC

### Project execution
* **Identifying gaps**
	* See *Setting direction -> Driving success metrics*
	* CSESC
	* Slack channels with biggest customers / design partners
* **Tools**:
	* Google sheet: Roadmap and resourcing plan
	* Jira: Backlog
	* Looker: Data analytics, funnel
	* Grafana + OTel: Engineering observability, SLOs
	* Swarmia: DORA
	* Jira dashboard for CSESC
* **Course-correct**:
	* Pivoting to CI
	* Largest customers (>60% revenue) has 10 builds in parallel. Focus on stability and scalability. (code yellow)

## Focus areas

- [ ] Architecture and technical stuff
- [ ] Data (MAU, metrics, scale)
- [x] Setting direction:
	- [x] How do you set and drive success metrics?
	- [x] How do you align your projects to the objectives of the organization at large?
	- [x] Business acumen: Linking team goals to business objectives
- [ ] Project Execution:
	- [x] How do you identify and address gaps?
	- [x] What tools and/or systems have you utilized or built to ensure quality work across complex problems?
	- [x] How often do you course-correct?
	- [ ] How do you manage your team to ensure execution?
	- Note from Gianni: Providing autonomy, not being a dictator
- [ ] Stakeholder management
	- [ ] When have you had to influence and negotiate with stakeholders and peers?
	- [ ] How do you build strong partnerships across the organization?
	- [ ] How do you foster engagement?
	- Suggestions:
		- ???
		- TODO: Ask ChatGPT for an example of stakeholder management
- [ ] Cross-team collaboration (influence, building relationships, shared goals)
	- [ ] Not just the good stuff (we hold hands), they want to hear about the challenges
	- Suggestions:
		- BuildKit
		- consumption based pricing
		- support enablement
- [ ] Learnings, mistakes, retrospective
	- Suggestions:
		- Private network access
		- DBC release postponed (see *Leadership & Behavioral*)
- [ ] Tradeoffs: Understanding design trade-offs
	- Suggestions:
		- BYOC (single tenancy), private network access
		- Dynamic provisioning
- [ ] Business vs operations vs development considerations
- [ ] Ambiguities and shift in direction.
- [ ] Do you understand top-down & bottoms-up management styles – when to apply each one?
- [ ] Ability to clearly communicate thoughts

## General suggestions
* Funnel improvements: collaboration with product, design, and data team
* On-call process
* The first team with consumption-based pricing
	* We needed metering
	* We needed a good billing UI
* Challenges:
	* Buildkit cannot scale horizontally, so our team needed to do this with their guidance
	* Dynamic provisioning with untrusted compute
* Incident response