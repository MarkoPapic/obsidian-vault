We currently have a `corridor_links` Postgres table that is used for corridors. However, I would like to have a first-class citizen `links` table that is also used by corridors but outlives corridors.

A link should connect two instances of `locations` table.

A link should have (among others), the same properties as `corridor_links`:
- `distance_metres numeric(12, 3) not null check (distance_metres > 0)`
- `speed_previous_to_current_kph numeric(8, 3) not null check (speed_previous_to_current_kph > 0)`
- `speed_current_to_previous_kph numeric(8, 3) not null check (speed_current_to_previous_kph > 0)`
- `distance_is_estimated boolean not null`

So a corridor is an ordered collection of links.

Replace `corridor_links` with `links`. There are some caveats:
- We use `controllers` for corridors, not `locations`. You will need to retrieve a location from the controller and vice versa. Retrieving a location from the controller is a straightforward `controllers -> entities -> locations` path. However, retrieving a controller from a location doesn't technically give you a unique controller. However, you can just get the first one, because in practice, only one controller will be assigned to a location.
- At some point, we will expose endpoints for creating links explicitly. But at the moment, links can only get created as part of corridor creation. So, when a corridor gets created, you should upsert the links.
- When a link gets updated through a corridor editor, it should be updated in the `links` table too.
- When a corridor gets deleted, don't delete the corresponding links.
- I don't think you need to modify the public API or ProtoBuf schemas for this. This seems like an internal change. But I may be wrong.

Don't worry about backwards compatibility with the existing corridors and `corridor_links`. The whole corridors epic is under a feature flag, so there is no data in production yet. So feel free to delete any existing corridors or `corridor_links`.
