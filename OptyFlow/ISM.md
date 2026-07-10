## Dan Questions

- [ ] Do we need `binary_blob` in `GetSetCommand`?
	- [ ] Yes, because of `Set` commands
- [ ] Time doesn't get sent as delta?
	- [ ] Time is part of the `extended`, which is always sent as full
	- [ ] Time is not part of `basic`


## ISM DB layout

**Page**: Each page has the same set of parameters. They allow you to easily swap a whole page timing configuration. It is a synonym for "plan"

**Sheet**: It is just a pagination for the UI.

Coordination doesn't use pages but sheets. But it also supports patterns.

Relevant things for decoding pages
- Manual
- `ism.h`

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