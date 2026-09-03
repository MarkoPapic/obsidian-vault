- Foundational:
	- Should we ditch the RGB status? This is applicable to "Corridor overview", "Status & control", "Map"
- Corridor geometry
	- Rename to "Corridor configuration" or "Corridor editor"
- Corridor overview
	- What does healty/degraded/critical mean? Do we want to remove it?
- Status & Control
	- Should we only flag plan missmatch
		- What if they are in different plans but have the same splits/cycle/...?
		- What if they are in the same plan but have different splits/cycle/...?
	- Coordination % per corridor is too coarse
		- We can't mark the corridor as green just because all the intersections are in _a_ coordination
	- Rename "Coordination Health" to "Coordination Overview"
	- During night, agencies may want to put the corridor in programmed flash or free. This would mean that the intersections are not in coordination 100% of time and we would mark it as only 50% of time coordinated even though that is expected.
	- Status list:
		- What corridor-specific information can we display in the list?
- Change history
	- What should we display here?

## To Discuss with Josh

- What corridor-specific information can we display in the corridor status list?
- Map view: What should we display here?
- Does he want a summary (healthy/degraded/crictical corridor)? If yes, what does a "healthy" corridor mean?
- How to handle the fact that an intersection can belong to multiple corridors
