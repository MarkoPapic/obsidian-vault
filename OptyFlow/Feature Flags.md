**Backend**
- [x] Come up with naming conventions
- [x] In code, configure `RefreshOptions` so that it doesn't refresh too often. Default is 30 seconds.
- [x] Define a common `FeatureFlagContext` which can be used by mutiple feature flag filters. It should contain: userId, env, agency, version, tmcVersion, uiVersion,  ...
- [ ] `cloudws` endpoints:
	- [ ] Client check (`cloudws` infers information from the token)
	- [ ] TMC check (TMC needs to send information such as `userId` and `agency`)