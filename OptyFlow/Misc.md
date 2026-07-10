Emulators IP: `192.168.0.34`

Wireshark VM password: `StrongPassword123!`

Wireshark capture filter:
```
udp port 5001
```


Wireshark:
- Alerts: ???
- Event page for the controller: 18:36
- Status counters: 18:47

Staging DB VM IP: `192.168.122.137`
Staging VM IP: `192.168.122.246`

Prod DB VM IP: `192.168.122.136`
Prod VM IP: `192.168.122.180`

MQTT:
- Username: optyflow-user
- Password: optyflow-password
- Client ID: tmc
- IP: 20.80.74.127
- Port: 1883

# WebSocket fix

Yes, you can implement it, but not cleanly from outside golang.org/x/net/websocket.

Important distinction: golang.org/x/net/websocket is not the Go standard library. It is a Go subrepository package. Contributing there is possible, but it has a different maintenance path
than net/http.

Why A Wrapper Is Hard
The exported APIs do not expose enough frame metadata.

To reconstruct a WebSocket message correctly, you need at least:

- frame opcode: binary/text/continuation/control
- FIN bit
- payload bytes
- masking validation
- control frame handling in the middle of fragmented messages

websocket.Message.Receive hides this and returns one frame. Conn.Read is also not enough because it streams frame payloads but does not expose message boundaries. It can continue into the
next frame/message, so you cannot reliably know where one WebSocket message ends.

The required state is in unexported types like hybiFrameReader.header.Fin.

Best Path If You Want To Stay With x/net/websocket
Implement this inside a fork/copy of golang.org/x/net/websocket, then use that from libs/ws.

I would add a new API instead of changing Codec.Receive, because current behavior is documented as single-frame receive:
```
func ReceiveMessage(ws *Conn, v interface{}) error
```

or lower-level:
```
func ReadMessage(ws *Conn) (payloadType byte, data []byte, err error)
```

Implementation Shape
Inside the websocket package, the logic is roughly:
```
func ReadMessage(ws *Conn) (byte, []byte, error) {
	ws.rio.Lock()
	defer ws.rio.Unlock()

	var payloadType byte
	var out []byte
	started := false

	for {
		frame, err := ws.frameReaderFactory.NewFrameReader()
		if err != nil {
			return 0, nil, err
		}

		frame, err = ws.frameHandler.HandleFrame(frame)
		if err != nil {
			return 0, nil, err
		}
		if frame == nil {
			continue // ping/pong handled internally
		}

		hf, ok := frame.(*hybiFrameReader)
		if !ok {
			return 0, nil, ErrBadFrame
		}

		switch frame.PayloadType() {
		case TextFrame, BinaryFrame:
			if !started {
				started = true
				payloadType = frame.PayloadType()
			}
		default:
			// continuation frames are rewritten by HandleFrame to original type
			// in x/net/websocket, so normal fragmented continuation payloads
			// should arrive here as text/binary after handling.
		}

		part, err := io.ReadAll(frame)
		if err != nil {
			return 0, nil, err
		}
		out = append(out, part...)

		if trailer := frame.TrailerReader(); trailer != nil {
			_, _ = io.Copy(io.Discard, trailer)
		}

		if hf.header.Fin {
			return payloadType, out, nil
		}
	}
}
```

That is simplified. Production version should also:

- enforce max complete message size, not just per-frame size
- validate continuation sequencing strictly
- reject unexpected continuation frames
- preserve existing ping/pong/close behavior
- avoid mixing this API with Conn.Read or Codec.Receive on the same connection
- add tests for fragmented binary messages, fragmented text messages, control frames between fragments, oversized messages, and unfragmented messages

Then Codec.ReceiveMessage can be:
```
func (cd Codec) ReceiveMessage(ws *Conn, v interface{}) error {
	payloadType, data, err := ReadMessage(ws)
	if err != nil {
		return err
	}
	return cd.Unmarshal(data, payloadType, v)
}
```

And a cousumer would change:

err := websocket.Message.Receive(conn, &rawMessage)

to something like:

err := websocket.Message.ReceiveMessage(conn, &rawMessage)
