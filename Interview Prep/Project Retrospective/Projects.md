
# Build Cloud

## Themes
* [[#Why?]]
* [[#Setting direction]]
* [[#Team building]] 
* [[#Project execution]]
* [[#Phased rollout]]
* [[#Stakeholder management / XFN]] and [[#Challenges]]
* [[#Results]]

## Results
* more than 500K builds per month
	* ~300 builds/s, peak 1K concurrent builds
	* Considering that 500K builds is clustered around weekdays, working hours
* 40K MAU
* 3K orgs

## Why?
* **Business acumen**: Parts of company strategy were:
	* multi-product revenue
	* expand into cloud industry and offer cloud-based products
* **Problem space**: An average developer spends 1h per day waiting for builds to finish
* **Build cloud**:
	* Powerful infrastructure specialized for builds
	* Shared caching
	* DD integration

## Setting direction
* **Business objectives (OKRs)**
	* Multi-product ARR
* **Success metrics**: Partnered with product manager
	* North star: Number of cached builds: 80% builds with at least 1 cached layer
	* Health metrics:
		* Cold start: Up to 3 seconds
		* Isolated compute
		* Availability: 99.99%
		* Reliability: Number of successful builds: 99.99%
		* Incident response: acknowledge within 15 minutes (on-call)
* **Roadmap**

## Team building
* Skills I needed
* Team composition
* Reflection:
	* The team executed great, the software scales successfully to 500K builds per month.
	* They lacked skills in untrusted compute on-demand provisioning

## Project execution
* **Clear goals**
* **Technical strategy**: sprinkle shows of success (500K builds/month)
* **Driving delivery & quality**
	* Planning and prioritization - focus on outcomes, not outputs (providing autonomy)
	* Execution discipline: Weekly planning, standups, retros, demos.
	* Short feedback loop: Demos, design docs, dashboards
* **Driving success metrics**
	* Looker analytics: Reviewing with PM and PMM weekly
		* Builds, orgs, subscription type, cached layers...
		* Top customers
		* Usage patterns
		* Usage within org (e.g. champion + onboarded other members)
		* Revenue
	* Grafana (OTel) + BugSnag + Swarmia: Weekly operations review
		* Successful builds
		* Availability + Reliability
		* DORA
	* Alerting: Slack alerts
	* CSESC
	* Slack channels with biggest customers / design partners
* **Removing blockers**
	* Billing team potentially not on track for overages API
	* BYOC: Found companies in the pipeline to get insights on what they want
* **Identify gaps / course-correct?**
	* Stability code yellow (reliability metric, CSESC)
* **Tools**:
	* Google sheet: Roadmap and resourcing plan
	* Jira: Backlog
	* Looker: Data analytics, funnel
	* Grafana + OTel: Engineering observability, SLOs
	* Swarmia: DORA
	* Jira dashboard for CSESC

## Phased rollout
* C0
* EAP (exit criteria)
* GA

## Stakeholder management / XFN
* I Identified the stakeholders and proactively established a relationship with them:
	* Upwards: VP of product, VP of engineering
	* Product + design (obviously)
	* PMM for GTM
	* Support
	* Sales
	* Legal
	* Security
	* Technical writers
	* Finances (budget for on-call)
	* Other teams (Core Build, Billing)
* Reached out and clarified what they care about
	* E.g. Support needs external/internal documentation, escalation paths, troubleshooting guides...
	* E.g. legal needs to review our data flows
	* E.g. security needs us to be SOC2 etc. compliant
* Bi-weekly cross-functional syncs
	* Product, design, PMM, support, sales, legal, security
		* Share updates
		* Identify dependencies
		* Identify and raise risks
	* Depending on the needs and the stage of the project, I communicate with some of them more frequently
* Frequent status updates: Avoid surprises and last-minute bad news

### Situations
* Consumption-based pricing + RevRec challenges
	* I identified that the current pricing model is not ideal for our new product
	* Sat down with the PM and we came up with a Billing UX proposal for DBC
	* Reached out to the Billing team to put it on their roadmap and included them in our weekly XFN syncs
	* Helped the team understand the needs and context and established a joint working group
	* Defined clear APIs/ownership
	* RevRec challanges (see *Conflicts/Challenges/Missalignment*)
* Multi-region (product, cost, legal, security)
* Untrusted compute joint working group
* Funnel improvements:
	* Identified leaky funnel
	* Attended EAP customer calls and UX research calls
	* Facilitated a workshop with PM and designer and came up with onboarding improvements
* Regularly participated in EAP customer calls and UX research calls. PM and designer appreciated the engineering perspective. E.g. answering the question "Why can't we just build this ourselves using Buildkit `remote` driver?"
* Billing team code red
* Sales pipeline: What' s coming ahead

### Conflicts/Challenges/Missalignment
* Billing system not designed for consumption-based pricing and RevRec cannot recognize revenue this way
	* I scheduled a meeting to understand their needs and limitations
	* Involved PM and drove alignment between 3 parties
	* Suggested a "buckets of minutes" SKU
		* Tradeoff, we don't have post paid overages for GA
	* They can support it in 6 months, given this becomes a priority for them
	* I escalated to my manager and he prioritized this with their manager
* Untrusted compute joint working group
* Buildkit maintainer win-win
* Andrei and pinning Entity Tree
* Company on-call is weekly 12-hour shifts but my team is not geographically distributed and other engineers cannot support our product yet

## Ambiguities and shift in direction
* Multiregion
* BYOC (Single tenancy)
* Pivoting to CI
* Largest customers (>60% revenue) has 10 builds in parallel. Focus on stability and scalability. (code yellow)

## Tradeoffs: business vs operations vs development
* Dynamic provisioning
* BYOC (single tenancy)
* Untrusted compute isolation
* Private network access
* GPU painted door

## Learnings, mistakes, retrospective
* Stability code yellow (retrospective!)
* Billing team code red (retrospective!)

## Challenges
* Billing system not designed for consumption-based pricing and RevRec cannot recognize revenue this way - see *Stakeholder management*
* Buildkit cannot scale horizontally
* Context transfer optimization

## Top-down & bottoms-up management styles – when to apply each one
* **Top-down**: Manager sets goals, priorities, and approach; useful when clarity, speed, or alignment is critical. Effective in:
	* Crisis situations (e.g., outage, security vulnerability).
    - Company-wide strategic pivots where consistency is key.
    - Early-stage projects with no clarity.
* **Bottoms-up:** Team contributes ideas, owns solutions, and drives innovation; useful for creativity, buy-in, and scaling leadership. Effective in:
	* Mature, high-performing teams who thrive with autonomy.
	* Innovation-heavy or ambiguous problem spaces.
	* Building long-term team ownership and engagement.
