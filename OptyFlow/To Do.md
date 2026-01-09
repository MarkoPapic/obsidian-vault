
## Now
- [x] Alert instances list
- [x] System settings UI
- [x] Enable alert
- [x]  System settings
	- [x] `v6.go` that matches the baseline (system settings + errors)
	- [x] don't forget the `error_severity` type
- [x] Migration that populates user emails
- [x] Rebase `main` onto the frontend branch
- [ ] 

## Later
- [x] Get event types endpoint
- [x] Enable/disable event type
- [x] Events screen on the UI
- [ ] Feature flags fix
- [ ] Test everything one more time (with signals)
- [ ] Test migration
- [ ] On frequency should "reset" after the trigger has happened. Currently if "5 in 1 minute" we fire an alert on 6, 7, 8, etc. 
- [ ] On duration should also reset (e.g. if "active for 5" triggered it, 6th minute should not trigger it)
- [ ] Boolean algebra for triggers
- [ ] Fix bug where seen alerts reappear after reload
- [ ] Set CASCADE to set `null` for all FKs to `event_log` so that it doesn't prevent the cleanup job
- [ ] Set CASCADE to other new tables where needed
- [ ] Events & alerts cleanup job
	- [ ] `event_active_duration_deadlines`
	- [ ] `event_frequencies`
	- [ ] `alert_instances`
	- [ ] `alert_recipients`
	- [ ] `alert_delivery_attempts`
- [ ] Errors
- [ ] Entity filters
- [ ] Reuse escalation policies for multiple alerts (DB schema supports this)


## Missing things
- [ ] Event polling
- [ ] Email delivery
- [ ] SMS delivery
- [ ] Alert card additional information and styling
- [ ] Entity filters configuration for triggers

