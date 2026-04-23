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

## Demo

Using the public OptyFlow Central

Log out of https://central-stage.optyflow.com/ (or use Incognito Window)

Update the `node1.optyflow.com` entry in the `C:\Windows\System32\drivers\etc\hosts` file:
```
20.80.74.127 node1.optyflow.com
```

Then go to https://central-stage.optyflow.com/ as usual and log in with the following user:
- Username: `nijoqilo@fxzig.com`
- Password: `DemoPassword897!`

If you decide to delete the controller and add it again, use asset ID: `0000178`

DB password: dbpassword


### Sasa MCP error

```
https://optyflowentraidextstage.ciamlogin.com/8fde787b-73f6-4ca5-b437-de93ab4c7c68/oauth2/v2.0/authorize?client_id=22adf34f-cf00-4513-80f0-89b0f4b79797&code_challenge=NWf31iVJhZG0KSmIxPk_7nrSYePvOABIUcjrl38OZ9s&code_challenge_method=S256&redirect_uri=https%3A%2F%2Fclaude.ai%2Fapi%2Fmcp%2Fauth_callback&response_type=code&state=iGtja6F-RfyrCxfZGptkmFIgdk82YOikTwfmhRnMuww
```