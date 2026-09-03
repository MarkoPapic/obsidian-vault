## Dan Questions

- [x] Do we need `binary_blob` in `GetSetCommand`?
	- Yes, because of `Set` commands
- [x] Time doesn't get sent as delta?
	- Time is part of the `extended`, which is always sent as full
	- Time is not part of `basic`


## ISM DB decoding

**Page**: Each page has the same set of parameters. They allow you to easily swap a whole page timing configuration. It is a synonym for "plan"

**Sheet**: It is just a pagination for the UI.

Coordination doesn't use pages but sheets. But it also supports patterns.

Relevant things for decoding pages
- Manual
- `ism.h`

### Implementation plan

- [x] Define Go structs (and interface)
	- Inputs: manual, ISMTablesStructure.txt, and ism.h
- [ ] Verify Go structs with eyes
- [x] Extract expected data from PDFs to text
- [ ] Verify expected data
- [ ] Write unit tests based on the expected data
- [ ] Implement decoder

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

If you decide to delete the controller and add it again, use asset ID: `178`

DB password: dbpassword
