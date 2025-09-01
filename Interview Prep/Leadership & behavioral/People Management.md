| Question                           | Answer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Underperformer - positive outcome  | [[#Sergio TCC tech stack]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Underperformer -layoff             | [[#Michalis - laid off]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 1:1s                               | [[#1-1s]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 1:1s approach                      | co-owning the agenda; open-ended questions, mixing topics                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Evaluate a healthy/unhealthy team? | - **Impact & Outcomes**: Does the team consistently deliver business value?<br>- **Technical Excellence**: Are engineering practices (code quality, reliability, innovation) strong?<br>- **Collaboration & Culture**: Does the team function well together and across functions?<br>- **Growth & Resilience**: Are individuals learning, advancing, and staying engaged?<br>- **Autonomy & Clarity**: Can the team operate independently with clear priorities and accountability?<br><br>Specific example: **stability code yellow** |
| Course-corrected                   | stability code yellow                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Building a team                    | Example: DBC<br>**1. Product strategy**: Fast builds<br>**2. Technical roadmap**: shared cache, untrusted compute, isolation, on-demand provisioning, low cold start, high availability<br>**3. Skills**: SaaS, low-level BuildKit, infra<br>**4. Composition**: Diverse team, learning opportunities (BuildKit maintainers, dynamic provisioning)<br>**5. Hiring**: see [[Approach#Hiring and team building]]<br>**6. Reflect and evaluate**: we needed more isolation + long running compute experience (Rodny, task force)          |
| Grow a senior 1                    | [[#Silvin promotion]]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

## Answers

### Sergio TCC tech stack
- Docker acquired AtomicJar
- Sergio's impact lowered over time. The output was there, but the outcomes were not impactful. He started focusing his effort on tasks that are not high priority.
- Staff Engineers play a pivotal role in technical leadership, and disengagement at that level can affect both team momentum and morale.
- I talked to Sergio in our 1:1 as soon as I noticed this
	- I wanted to understand his perspective without jumping to conclusions
	- I assumed best intentions: he had previously been a strong contributor and a great leader
- Root cause: tech stack, architecture, no staging
- While his concerns were valid, I explained that:
	- These things are common in the software industry - companies acquire other companies
	- Staff Software Engineers are expected to lead through them, not avoid them. His role required him to help bridge the gap between the two systems, not opt out of the hard parts.
	- Making the new codebase work with our existing codebase is crucial for our product strategy.
-  We aligned on the above expectations and together came up with an improvement plan
	- Sergio pair with Kiril
	- I secured a short-term help from Infra
- Sergio committed to leading the effort and viewed it as an opportunity to influence the architecture instead of being frustrated by it.
- Over the following months, I checked in frequently and helped with minor moments of friction
- Sergio became re-engaged, and took ownership of the integration work. Not only he made it work, but also suggested and implemented some ideas for future-proofing (e.g. time-series database for insights)
- His impact returned to the expected Staff Engineer level and the knowledge he applied was allowed other engineers to learn from it
- My learnings:
	- Even senior/staff engineers can get hesitant to take ownership and make changes, and feel as the situation was imposed top-down
	- It is important to act early and encourage the bottoms-up approach - suggesting changes and taking ownership and accountability for them

### Michalis - laid off
He underperformed in the following ways:
- Tasks he worked on took too long - way longer than expected, given the complexity of the tasks.
- He didn't provide any visibility or transparency into his work.
- He frequently missed meetings without notice. Then he retroactively says that he had to pick up a kid from school.
- When I manage to get a hold of him to discuss the progress of his tasks, he says that it is taking that long because he is doing thorough research before implementing a solution.
- When he finally delivers, the solution is usually low quality, which is contradictory to the amount of research he claims he has been doing.
- He showed no proactivity. When out of work, he would wait for me to explicitly give him something to work on.
In our 1:1 meetings, I brought these things up and tried to get to the root cause together with him, without jumping to conclusions. He was cooperative in these conversations and we came up with a 30-day performance improvement plan. Key elements included:
- Improving visibility:
	- Better Jira hygiene - I provided some examples
	- Standups - sharing more information
	- Writing design docs - sharing his intentions with the rest of the team
- Calendar hygiene: I understand if he has to pick up a kid from school, but I advised him to block these time slots in the calendar ahead of time so that the rest of the team is aware that he will be unavailable
- Tasks are taking too long due to the apparent research he is doing: Writing design docs (as mentioned above) was supposed to address this, too, giving the rest of the team visibility into his intentions, as well as allowing them to give feedback.
- Ownership and proactivity: Being proactive about choosing priority tasks or reaching out tome when unsure.
- Quality improvement: I also arranged pair programming sessions with more experienced engineers, with the intention of improving the quality of his deliverables.

He committed to the above improvement plan, and I documented it and shared it with him.

I documented the plan and tracked progress regularly. While there was some temporary improvement, the same patterns returned. Tasks dragged, visibility dropped, and quality remained low.

In the meantime, we had a formal, company-wide performance review where I also shared feedback with him, indicating that his performance is below expectations. In this formal review, his peers also shared similar reviews about him.

After 60 days with minimal improvement, I consulted HR. We extended the PIP with more intensive support and I kept close contact with HR to ensure fairness and remove potential bias.

After more than 90+ days of support and no meaningful, sustained improvement, I made the decision to let him go. Otherwise, his continuous underperforming would hurt:
- Delivery velocity
- Team morale and coordination

### 1-1s
- **Execution and delivery** (not status updates)
    - Are they clear on priorities and expectations?
	- Are they aware of how their work ties into the goals?
	- Any blockers?
	    - Cross-functional dependencies I could resolve?
		- Something I could escalate?
- **Career growth** (not every time)
    - Review development plan
	- Are they on a good path towards the next role - referencing the competence framework
	- Are their current projects helping them grow (challenging, new skills, ...)
- **Coaching and feedback**
    - Share feedback
	- Ask what feedback they have for me
- **Well-being and morale**
    - Do they feel their work is meaningful and impactful
	- Do they feel they have a safe environment and autonomy to innovate
	- Any stress, burnout, etc.
- **Recent changes**
    - Share latest changes
	- Discuss recent changes
	- How do they feel about the changes
- **Team dynamics and culture**
    - Any shoutouts?
	- Any conflicts?
	- Do they feel confident to share ideas/concerns/...?
- **Ideas / improvement areas**
    - 
- **How can I help?**

### Silvin promotion
Silvin is a great IC but not getting promoted for 7 years
- **Identify**: Influence too narrow. Lack of XT/XFN impact.
- Establish goals and plan:
	- Goal: become a Staff Engineer
	- Went over the competence framework
		- Mapped strengths and improvement areas
		- Aligned on them
- **Plan**: Stretch goal - Own a billing service
	- Requires a lot of XFN collaboration with the PM, Billing team, RevRec team
	- Requires ownership and accountability
	- Opportunity to mentor other engineers
	- We set expected outcomes, not outputs: autonomy
- **Coaching and support**: weekly follow-ups in our 1:1s and other meetings
	- XFN enablement: Ensuring prioritization and engineering resources from the Billing team
	- Coached him on delegating to the Billing team
	- Coached him on defining ownership and strong API contracts
	- Coached him on proactively reaching to the Billing team so that they can put it on the roadmap
	- Coached him on framing proposals in terms of outcomes and business value, not just technical design
	- Invited him to present at a meeting with VPs of prod/eng. Coached him on how to frame it in terms of outcomes, not outputs.
- **Evaluate and tweak**: We review the outcomes weekly
	- Customer value
	- Delivery velocity
	- Engineering metrics
- **Result**:
	- More people are aware of his influence
	- He got some public shoutouts/kudos
	- VP of eng supported his promotion
	- He got promoted

### TODO
- [x]  [[people_management_interview.pdf]] 
- [x] [[Interview notes]]
- [x] Gianni's email
- [x] TODO: Celena's PDFs (with 4-5 focus areas)
- [ ]  Someone you helped grow (senior) **this is the most important part of the interview**
	- [ ] Structure and process is very important
	- [ ] Come up with multiple examples and go deep and detailed
	- [ ] Try to come up with a situation that follows your coaching framework
	- [ ] Ideas:
		- [ ] Don't skip the obvious: E.g. PIP, Lattice growth/align section...
		- [ ] Providing with opportunities to make impact
		- [ ] Challenging tasks / stretch goals
		- [ ] Giving them something to own (for senior/staff+)
		- [ ] See *Approach -> Managing high performers*
	- [ ] Example ideas:
		- [x] Silvin being a great senior but not getting promoted for 5 years (at the moment I joined)
		- [ ] Sergio
			- [ ] E.g. "i suggested to him to become a Buildkit maintainer"
- [ ] Managing high performers
	- [ ] See *Approach -> Managing high performers*
- [ ] Promoting someone
- [x] Managing low performers
- [x] Letting someone go
- [ ] Conflict resolution within the team
- [x] Building a team
- [x] How do you evaluate a good team?
- [x] Detecting and improving an unhealthy team
- [ ] Specific examples for 1:1s (e.g. what if they respond that their work is not meaningful / burnout/ ...)
- [ ]  How do you hold yourself and/or others accountable for managing team performance?
- [ ] How do you recognize your team’s successes and empower them to celebrate one another’s achievements?
- [ ] How do you foster an environment of continuous learning?
- [ ] Are you aware of how other teams are performing so you can identify gaps in yours
- [ ] Situation where you adapted your approach
- [ ] What was the last bit of feedback you received?
- [ ] Example of feedback that you have delivered that was really impactful?
- [ ] 