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

## Doc

Context & Problem Statement
- todo

Requirements:
- Performance
- Security
- Discoverability & industry standards (think of a name for this)

Assumptions:
- We use TCP for transport layer

Protocols:
- Raw TCP
	- Full duplex
	- Requires custom implementation of everything.
	- Not as canonical - no industry standards on APIs. We would need to have a proprietary API.
- WebSocket
	- Full duplex
	- No endpoint discoverability
	- Minimal overhead
	- Not as canonical - no industry standards on APIs. We would need to have a proprietary API.
	- No industry standard authz. We would need to implement non-canonical mechanism.
- MQQT
	- How would we achieve request-response?
	- Small overhead (good)
	- Requires additional infrastructure component
	- We need custom patterns to "simulate" sync use cases
	- Supports industry standard authz
- gRPC
	- Requires HTTP
	- Supports industry standard authz
- HTTP
	- Unidirectional
	- Text based
	- Supports industry standard authz

Encoding:
- JSON
	- A lot of overhead
- ProtoBuf
- ...

**ChatGPT prompt**:
- It is blunt: refine it and articulate it better. But still keep it brief.
- Review it:
	-  Is this a useful document for starting an asynchronous brainstorming?
	- Am I missing something?
	- Did I put anything incorrect?
- Suggest additional protocols and/or data formats