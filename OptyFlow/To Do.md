- [ ] Navigating from and back to the Map reloads the map, which takes a couple of seconds
- [x] Status list `Expand` doesn't go to Status
- [x] Intersection layout "DB page" flow is not intuitive. Especially `Download` button.
    - [x] Put `Intersection Layout` as the first page.
    - [x] Rename `Instersection Layout` to `Intersection Geometry`
    - [x] Agree on a better name for `Pages` 
    - [x] Agree on a better name for `Download`
- [ ] StatusDbData should be refreshed upon Download
- [ ] Entity Tree collapses on page change (e.g. when navigating from status list to map)
- [ ] Reports should run as soon as the user navigates to the report template (with default filters)
- [ ] Outline what categorizes as a fault in `Faults Summary by Intersection`
- [x] Rename `Phases ON` to `Active Phases`


**DB config drift report**:
- [ ] Test that ensures that every page type has a `PageLabel` in `tmc/domain/asc/db/constants/pageType.go`
- [ ] 

**ISM**:
- [ ] Unit tests for `tmc/domain/asc/broker/status.go`
- [x] Switch to concise topics
- [ ] Semi-expanded
	- [ ] Coordination
	- [ ] Phasing & calls
- [ ] Meaningful ring timing names

**Reporting**:
- [ ] Integration tests for reporting features
- [ ] Sort event by time in UI (maybe even when fetching)
