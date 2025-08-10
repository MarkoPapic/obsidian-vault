## Customer portal

Prod deployment:
* ~~`install-token-hmac-secret` secret in staging and prod~~
* ~~DB migrations in staging and prod~~
* ~~Clear out old JWKS~~
* ~~Add a public key for OTel to JWKS~~
* ~~Move Terraform state to Azure Storage~~
* ~~Update config in `/datacenter` UI (new connection parameters)~~
* ~~CustomerPortal deployment script and env (referenced by Terraform)~~
* ~~CustomerPortal deployment pipeline~~
* ~~CustomerPortal staging deploy~~
* ~~Add user invitations to agencies - stage~~
* ~~CustomerPortal UI staging deploy~~
* ~~CustomerPortal prod deploy~~
* ~~CustomerPortal UI prod deploy~~
* ~~TMC prod installation script update~~

### Installation script

Write a shell script that expects the following variables to be set and does the following with them:
- `OF_INSTALLATION_TOKEN`: Assign the value to a new `INSTALLATION_TOKEN` variable. If unset, prompt the user to enter it.
- `OF_AGENCY_ID`: Assign the value to a new `AGENCY_ID` variable. If unset, prompt the user to enter it.
- `OF_NODE_NUM`: Assign the value to a new `NODE_NUM` variable. If unset, prompt the user to enter it.
- `OF_DB_PASSWORD`: Assign the value to a new `DB_PASSWORD` variable. If unset, prompt the user to enter it.
- `OF_DB_NAME`: Assign the value to a new `DB_NAME` variable. If unset, set to `optyflow`.
- `OF_DB_USER`: Assign the value to a new `DB_USER` variable. If unset, set to `optyflowuser`.
- `OF_DB_PORT`: Assign the value to a new `DB_PORT` variable. If unset, set to `5432`

Declare `HOST` variable and set it to `node<OF_NODE_NUM>.optyflow.com`

If `OF_AGENCY_KEY` is set:
- Assign the value to a new `AGENCY_KEY` variable.
Else
 - Call a `generate_key_pair` function that accepts `OF_NODE_ID` as an argument. Leave the implementation of the function for later.

At the end, the script should output (`echo`) the following values:
- `INSTALLATION_TOKEN`
- `AGENCY_ID`
- `NODE_NUM`
- `DB_PASSWORD`
- `DB_NAME`
- `DB_USER`
- `DB_PORT`
- `HOST`

