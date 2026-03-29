
I want to allow end users to communicate with the `tmc` server through the `cloudws` server in the same way they would communicate directly with the `tmc`. The `cloudws` service would act as a proxy in some way. At the moment, there is some communication between `tmc` and `cloudws` that is not related to this end-user "proxying" and that communication needs to be preserved - the "proxying" should be built on top of it.

For clarity and simplicity, let's introduce the following terms:
- TMC: The `tmc` service
- Cloud: The `cloudws` service
- Client: client applications (e.g. a web browser application)

## Current State

All communication between a client and the TMC happens over the WebSocket protocol. Also, all communication between TMC and Cloud happens over the WebSocket protocol.

The following ways of communication between a client and the TMC are supported at the moment:
- Request-response communication: clients send a WebSocket message to TMC and wait for the response
- Client-initiated fire-and-forget messages: clients send a WebSocket message to TMC and don't expect a response
- TMC-initiated fire-and-forget messages: TMC sends (pushes) a WebSocket message to a client
	- Sending of all these fire-and-forget messages is centralized in `tmc/clients/monitor.go`

The following ways of communication between the TMC and the Cloud are supported at the moment:
- Request-response communication: TMC sends a WebSocket message to the Cloud and waits for the response
- TMC-initiated fire-and-forget

Relevant code:
- `libs/comms/message.go`: Message formats
- `libs/comms/response.go`: Helpers for constructing response messages
- `libs/ws/connection.go`: WS connection implementation
- `tmc/infra/network/application/client/websocket.go`: Client WS handler on the TMC side
- `tmc/infra/network/application/cloud/websocket.go`: Cloud WS handler on the TMC side
- `cloudws/infra/ws/websocket.go`: WS handler on the Cloud side
- `tmc/clients/monitor.go`: All fire-and-forget (push) messages from TMC to clients are sent from here.

## Desired State

Beside the existing communication functionality, I want to support the following ways of communication:
- Request-response communication from a client to the TMC through Cloud
	- Request flow: `client -> Cloud -> TMC`
	- Response flow: `TMC -> Cloud -> client`
- TMC-initiated fire-and-forget messages to the client through Cloud:
	- Flow: `TMC -> Cloud -> client`

## Proposal

Since the existing functionality supports the following bi-directional communications:
- Clients <-> TMC
- TMC <-> Cloud

we can assume that we have the following
- Persistent WebSocket connection between a client and Cloud
	- Cloud can extract the user ID from the auth token
- Persistent WebSocket connection between Cloud and TMC

We just need to build on top of the existing protocols.

### Request-response communication from a client to the TMC through Cloud

Flow:
1. Cloud receives a message from a client
2. Cloud creates a message for TMC which consists of the following:
	 - Original client message
	 - User ID
 3. Cloud sends a message to TMC
 4. TMC Decodes the message and synthetically constructs a connection instance that implements the `ClientConnection` defined in `tmc/domain/client/connection.go`
 5. TMC processes the message using the existing handlers defined in `tmc/clients/api.go` via `tmc/clients/handlers.go`
 6. When Cloud gets a response from TMC, it forwards a response to the same client connection

### TMC-initiated fire-and-forget messages to the client through Cloud

Since TMC sends these fire-and-forget messages exclusively from `tmc/clients/monitor.go`, we can have `Monitor` have a reference to the Cloud WebSocket connection. Then whenever `Monitor`sends a message to clients, it also sends it to the Cloud connection.
Besides the original message, this TMC -> Cloud message will also need to specify the intended recipients of the message, e.g.:
- "all": A special indicator that the message should be broadcasted to all clients
- List of user IDs

Then Cloud can select the recipients from its connection registry and forward the message to them.

## Prompt

Let me know what do you think of the above plan and how it can be improved.