**Backend**
- [x] Come up with naming conventions
- [x] In code, configure `RefreshOptions` so that it doesn't refresh too often. Default is 30 seconds.
- [x] Define a common `FeatureFlagContext` which can be used by mutiple feature flag filters. It should contain: userId, env, agency, version, tmcVersion, uiVersion,  ...

To Do
- [x] Add the exported Azure App Configuration to Git and document the import process in the "first time setup"
- [ ] Add logs/spans around `cloudws` feature flag evaluation so you can troubleshoot it in staging

**Testing**
- [x] `Test.EnvironmentInclusion1`: Include `dev` and it should be enabled
- [x] `Test.EnvironmentInclusion2`: Don't include `dev` and it should NOT be enabled
- [x] `Test.EnvironmentExclusion1`: Exclude `dev` and it should NOT be enabled
- [x] `Test.Disabled`: Include `dev` but leave the FF disabled. It should NOT be enabled.
- [x] `Test.TargetIncluded`: Include Marko's user ID and it should be enabled.
- [x] `Test.TargetNotIncluded`: Include a dummy user ID and it should NOT be enabled.