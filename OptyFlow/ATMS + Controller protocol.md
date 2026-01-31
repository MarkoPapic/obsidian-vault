**Protocol**
- TCP (not support older infrastructure)
	- Intellight does it
	- FHWA assumes it

**Protocol characteristics**:
- Socket-based:
	- Full duplex
		- No polling: e.g. events & alerts
	- Avoids handshakes and auth every time

**Data encoding**:
- Canonical event schema (`TrafficSignalEvent`, `VulnerableRoadUserDetection`, `ConnectedVehicleState`, `SafetyAlert`, `TrafficOptimizationCommand`) like FHWA
- Param IDs like NTCIP and OIDS

**What about FHWA?**
- I don't like the idea of HTTP + JSON
- What about e.g. gRPC + ProtoBuf?
	- We are compliant with the ProtoBuf if we use it with WebSocket
	- When it comes to gRPC, we can have a second API that support gRPC
- ...

**What about security?**
* Challenge: What about agencies without public internet access (closed loop)? How do certificates/JWTs get validated in that case?

**Open questions**
- What about agencies without public internet access (closed loop)

**Dan's questions**:
- What about agencies without public internet access (closed loop)?
	- How do certificates/JWTs get validated?
- Should we control corruption/reliability?
	- Do we use TCP and rely on it
	- Or do we use UDP and control it

**Sasa's questions**:
- How would the implementation go? Separate process vs same process.
	- Dan: Ultimately, it needs to be part of ISM
	- 