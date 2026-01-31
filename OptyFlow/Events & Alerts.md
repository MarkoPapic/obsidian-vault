## Gathering events

High resolution logging standard: https://docs.lib.purdue.edu/cgi/viewcontent.cgi?article=1003&context=jtrpdata

Events are covered in **NTCIP 1103 -> A.7**
- Econolite doesn't support these MIBs - it returns "no such name"
	- Econolite has custom MIBs that return event data under: `1.3.6.1.4.1.1206.3.5.2.24`
	- There are also block objects `0x57`, `0x58`, `0x59` that return event log as a differential block object. I managed to get some data by fetching `0x05 57 01 01`
- Maxtime supports these MIBs but doesn't return any events. Maybe it needs to be configured - check NTCIP 1103 -> 6.1.3
* They have an `events` API. It only stores 10000 events in memory so you need to fetch it frequently not to loose data. However, this may be **high resolution events**, which may be an overkill.
* API endpoint: `http://192.168.1.33:1025/maxtime/api/events/query`
* You have it in Postman

## Testing


- [x] Event 1 occurrence
- [x] Event 2 Frequency
	- [x]  Happens when met
	- [x] Doesn't happen when not met
- [x] Event 6 Active
- [x] Event 6 active for 20s
	- [x] Happens when met
	- [x] Doesn't happen when not met
- [x] Event 4 AND Event 5 freq 5/1m
	- [x] Event 2 in between
	- [x] Event 2 at the end
	- [x] Doesn't happen if a minute expires (Event 2 in between)
	- [x] Doesn't happen if a minute expires (Event 2 at the end)
- [x] Event 3 AND Event 7 active
	- [x] Happens when met
	- [x] Doesn't happen when not met
- [x] Event 3 AND Event 8 active for 20s
	- [x] Happens when met
	- [x] Doesn't happen when not met
- [x] Event 1 OR Event 2 AND Event 6 active
- [x] Event 3 AND (Event 4 or Event 7 active)
- [x] (Event 5 OR Event 8 active) AND Event 9 active
