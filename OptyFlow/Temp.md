
## Frontend

### Current _seen_ flow

1. Components call `sdk.markAlertSeen()` or `sdk.markAlertsSeen()`
2. SDK updates alerts in IndexedDB
3. SDK emits the `ALERTS_SEEN` event
4. [AlertContext](/home/marko/Code/OptyFlow/atms-client-web/src/alerts/context.tsx) subscribes to `ALERTS_SEEN` and marks alerts as _seen_ in the context

### Desired _seen_ flow

1. Components call `sdk.markAlertSeen()` or `sdk.markAlertsSeen()`
2. SDK updates alerts on the server
3. Everything else gets taken care of automatically due to `SHOULD_SYNC_ALERTS` and `syncAlerts()`

Required model changes:
- [x] ProtoBuf message needs a `seen` indicator
- [x] Model needs a `seen` indicator
- [ ] Remove `markSeen` and similar methods from the model

