Based on functional requirements outlined in `docs/functional_requirements/events_alerts.md`, update this component so that it loads a list of alert configurations which correspond to `AlertConfigurationThumbnails` ProtoBuff message defined in`src/sdk/messages/alertConfiguration.ts` via WebSocket. The WebSocket request will require you to:
- Create a corresponding function in `src/sdk/SDK.ts`
- Define a `GET_ALERT_CONFIGURATIONS` `1354` outgoing message type  in `src/sdk/tmc/constants/messageTypes.ts`

You can see design in the attached screenshot.

Things you should ignore for now:
* Search capability
* Pagination

