
# Flows

## Process event

1. Event happens
2. If event is enabled, log it
3. If event is disabled, ignore it

## Create alert (without escalation)

1. Create trigger
2. Create alert

## Test alert
1. Create alert
2. Event happens
3. Alert shows up in the UI
4. Reload screen: alert should be displayed

## Alert updates
1. Alert happens
2. Acknowledge alert
3. New version of the alert is sent to the UI
4. Reload screen: new version of the alert should be displayed

## Alert escalations
1. Alert happens
2. Wait for the escalation
3. Escalation shows up for the escalation recipients

## Process active event

1. ...

# ToDo

- [ ] How to handle `onFreq` and `onActive` event criteria (check ChatGPT)
	- [ ] Triggers
- [ ] Escalation workflow
- [ ] Cleanup logic
	- [ ] Retention time for events
	- [ ] Alert receipients
	- [ ] Alert delivery
	- [ ] Event frequency buckets
	- [ ] Event activity duration deadlines

## Testing

- [ ] Check if triggers generate good IDs
- [ ] Test migration
- [ ] Test baseline

# Gathering events

High resolution logging standard: https://docs.lib.purdue.edu/cgi/viewcontent.cgi?article=1003&context=jtrpdata

Intellight:
* They have an `events` API. It only stores 10000 events in memory so you need to fetch it frequently not to loose data.
* API endpoint: `http://192.168.1.33:1025/maxtime/api/events/query`
* You have it in Postman

