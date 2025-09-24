
- London office is the 3rd largest (Menlo park, Seattle, London, …)
- All products covered in London
- Round 1 should be scheduled in July, but it is also ok to reconnect in August if I can't do it in July

# First loop

## Behavioral

Areas:
* People management
* Resolving conflict
* Embracing ambiguity
* Communicating effectively

All questions should be answered with a specific example, not generally.

STAR

Example questions:
* People management
	* Describe how you discovered knowledge gap in your team
	* Managing an underperformer
	* Someone in your team who grew significantly
* Conflict
	* Describe a conflict within your team that you handled
	* Describe a conflict you had with your manager
	* Describe a conflict you had cross-functionally
* Ambiguity
	* Scoping the project with unclear requirements

## Product Architecture Design

Imagine you and the interviewers are the first two engineers on the project and you are trying to figure out how to design the project.

You can also treat your interviewer as a Product Manager.

They use https://excalidraw.com/

Areas evaluated:
* Solution design
* Technical excellence
* Technical communication
* Problem navigation (clarify questions, confirm requirements, confirm direction, not making wrong assumptions and sticking to them)

Common failures:
* Poor time management
* Too high level
* Not discussing tradeoffs
* Overcomplicating design

[Jackson Gabbard](https://www.youtube.com/@jackson-gabbard)'s [episode 6](https://www.youtube.com/watch?v=ZgdS0EUmn70) is still relevant

# Next steps

Product architecture:
- Strong
	- Problem navigation
	- Strong communication
	- Methodic approach to problem solving with tradeoff discussions
	- Good understanding of requirements
- Improvement areas
	- Latency estimation was high
	- Didn't leave time to get into discussion about "undo"

Behavioral:
* Strong
	* Providing feedback
	* Mentoring seniors
	* Resolving conflict
* Improvement areas
	* Communication:
		* I don't always answer exactly to the point
		* I should've asked clarifying questions
	* Low performance management:
		* Lack of experience with letting someone go due to low performance

Celena will not be my recruiter anymore. Another recruiter will take over now.

Interview is valid for 2-3 months
I need to provide with a mininum 5 days of availability and at least a couple of hours per day

### Previous feedback
- It was good, but needs to be deeper
- Letting someone go
- People management:
	- It was good overal, but more ok than good
	- Nowadays they are more like "It is ok, but we need really good people"
	- In the next rounds, this needs to be better and deeper

## Interview rounds

5 interviews (2 technical, 3 managements)
- System design (another one)
- Coding

### Design (included feedback from the previous one)
Focus areas:
- Problem navigation (good)
- Solution design (good)
- Tehnical excellence (deep dive): I could do better here
	- Positives:
		- Structured (delayed until I have details)
	- Improvement areas:
		- Didn't think about node crash + stickiness
		- Game validation
- Technical communication (good)

### Coding (included someone else's feedback)
45 minutes, 2 algorithmic questions, expect 2 working solutions
- Problem solving: Time/space complexity estimation was accurate, recognized that recursion can solve it
- Coding: Quite quick in coding, write Java quickly, recursion, working solution
- Verification: Debugging, spotting edge cases
- Communication: Explained what they are about to do; communicate as you code

### People management
Focus areas:
- High performers
- Low performers
- Conflict resolution within the team
- Building the team
- Managing the team
- Improving the team
- Letting someone go
General guide:
- Proactivity (identify performance issues ahead of time)
- Understanding the root cause (empathy)
- Focus on seniors
- Outcomes/reflection and learnings
- Avoid "easy way out" and avoiding responsibility. E.g. no "moved them to another team"
- When managing the team, also include cross-functional stuff
	- E.g. are you aware of how other teams are performing so you can identify gaps in yours
	- Are you enabling them to work cross-functionally

### Project retrospective
What project to choose:
- Long-term project
- Project you had influence over (running rather than observing)
Focus areas:
- Setting direction
- Project execution
- Stakeholder management
- Cross-team collaboration (influence, building relationships, shared goals)
	- Not just the good stuff (we hold hands), they want to hear about the challenges
- Learnings/retrospective
- Don't be a dictator (providing autonomy)
- Business acumen
	- Linking team goals to business objectives
- Tradeoffs
- 

### Behavioral
Focus areas:
- Feedback culture and growth mindset
	- Do I proactively seek feedback
	- How do I take feedback (constructive)
- Managing ambiguity
	- Articulate ambiguity
	- Quickly pivot/adapt
	- Track the changes
- Conflicts: How I deal with the conflict
- Motivations to work
	- E.g. "Why do you get out of bad in the morning?"
	- The answer should be focused on **people***. E.g. you are passionate about people management (e.g. growing them)

## Next steps after the fool loop

**I passed all the interviews!**
VP will review, but before, we need a team of interest
	He shared my feedback with all hiring managers.
	Once we find teams of interests, I will have a call with a hiring manager.
Slight pause: Most open roles are infrastructure-heavy roles and I am product-oriented.
I need to send a CV to Gianni to share with HMs.
	If it contains something that leans towards the infrastructure, that could make things faster, but not necessary.
By next week, he will send me the teams of interest.
He will reach out to me next week.

## Team placement

**Role: Privacy and Security**
* HM
	* Yiannis P (Director)
	* Philip (HM)
* Org: Privacy and Security
* JD: No JD since the role is confidential
* I am not the only candidate


## Team placement call - Privacy & Security
### About the org

Meta is good at building products that engage users.
* You focus on 99% of users, and neglect 1%
* Over time, 1% of users starts complaining
* They complain to their representatives, who writes laws
	* GDPR
	* Cookies
	* ...
* These laws prescribe product requirements to you
	* This makes you cater for 1% of people, otherwise you pay penalty
They work with lawyers, PMs, etc. and come up with requirements, edge cases...
Then they build technologies that allow product teams
Privacy/risks org. But not responsible only for privacy laws, it is responsible for:
* Privacy laws
* Any abuse laws
* Competitions laws
* ...
You can call it "compliance" org.
They need to do this across the products. There are two extremes of doing this:
1. Provide requirements and make every product accountable for implementing it
2. Implement it centrally

### Team focus

E.g. you are not allowed to share data with 3rd parties. The goal is to identify, detect, and prevent this. Some examples of unintentionally/accidentally sharing data with 3rd parties:
* Your team calls a 3rd party API (a.g. Google Maps), sending user data
* Scraping (people using your APIs or scraping your web pages)
* Security breaches (someone hacks you)

4 teams.
* Rate limiting system
* ...

Team member skill requirements:
* Backend
* Abuse prevention

Success metrics:
* Not getting sued
* Being compliant
* Incidents are rare
* When incidents happen, you have a robust process for responding
* Stay ahead of the curve - find out about laws and regulations, and implement them before there is an incident

## Sep 22
Preliminary numbers (not official):
* Base: £139K (€160K)
* Performance bonus: 20% £27.8K (€32K)
	* Can be multiplied by individual and company performance
* Equity: 600K$ vesting over 4 years (in dollars)
	* $150Kper year (€127K)
* Equity refreshes: every year you can get an equity refresh (even for average performance)
* Relocation package: once we have an offer finalized

This would make a total yearly compensation: 

Reasons they will move from these:
* You want more money
	* Gianni is here to tell me what's realistic
* You've got a competing offer
	* Gianni can use this as a leverage
* Unvested stocks
	* Docker's stock options are a data point
	* But the finances will push back since it is not public
* What you have vesting over the next 12 months

Data points that don't work:
* Taxes
* Cost of living
* ...

**Next steps**
* We will catch up tomorrow at the same time - 10:30h
