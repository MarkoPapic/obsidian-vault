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

### Installation script feedback

* We should have a "requirements done"
* DB installation should be part of the instructions
	* Once  you are done, you click "Complete" and it takes you to the Central installation
- After the installation is complete, have a "Open OptyFlow Central" button which takes you to `central.optyflow.com`
- 