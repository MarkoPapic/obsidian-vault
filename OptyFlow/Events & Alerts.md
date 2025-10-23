
# DB Schema

**event_categories**
* id
* name: varchar(1024)

**event_types**
* id: guid
* name: varchar(1024)
* description: varchar(4096)
* category_id
* enabled: boolean
* actions: jsonb
	* Whether to log
	* ...
* retention_days

**events**
* id
* ocurred_at TIMESTAMP DEFAULT now (),
* stopped_at: Makes sense for events that can be active for some time
* device_id: so we can filter by controller id,
* user_id: used if audience is 'affected_user',
* is_active: We need this to figure out how long an even has been active
* data: jsonb

**alerts**
* id
* name
* description
* trigger_id
* enabled: boolean

**triggers**
* id

**trigger_event_criteria** (binds `triggers` and `event_types` many-many)
* event_type_id
* trigger_id
* behavior: jsonb

**alert_audience_group**
* id
* alert_id
* audience: jsonb
* severity
* communication_channels: jsonb
* entity_filter: jsonb

**alert_escalation_policies**
* id
* alert_id

**alert_escalation_steps**
* id
* policy_id
* audience: jsonb
* severity
* acknowledge_tolerance
* resolve_tolerance

**alert_instances**
* id: int so we can use it as a cursor for "I've seen all alert instances up to this id"
* alert_id
* trigger_event_criteria
* state
* current_escalation_step
* unack_deadline
* unres_deadline
* data
	* e.g. controller ID, detector ID, etc.
* metadata: jsonb
	* timeline
	* ...

**recipients**
* id
* alert_instance_id
* user_id
* seen (optional)
* match_source: `base` | `escalation`
* escalation_step

**alert_delivery**
* id
* alert_instance_id
* channel
* status: used by the cron to ensure atomicity
* last_attempt
* error

TODO:
- [ ] V4
- [ ] Triggers for composite keys
- [ ] Update baseline
- [ ] Indexes
	- [ ] `alert_instances`: `unack_deadline`
	- [ ] `alert_instances`: `unres_deadline`
- [ ] We probably need separate tables for offline alerts
- [ ] How to do escalations?

# Gathering events

High resolution logging standard: https://docs.lib.purdue.edu/cgi/viewcontent.cgi?article=1003&context=jtrpdata

Intellight:
* They have an `events` API. It only stores 10000 events in memory so you need to fetch it frequently not to loose data.
* API endpoint: `http://192.168.1.33:1025/maxtime/api/events/query`
* You have it in Postman

