# TDS+ Competition
*Dec 16, 2023, Kenny Gorman*
## Background
Time Distance Speed Rally (sometimes called Regularity Rally) events have existed for many years, and the format is tried and true. These events aren’t scored on a pure speed over time mechanism like conventional racing events - but rather by computing time plus or minus based on a desired time over a deterministic route. Competitors get penalty points for being either too late or too early to a specific point on a route (called a time check or TC). A given route may have many TCs, and their location is unknown to the competitor in advance. Competitors follow a route book like regular rally stages and navigate the route (called a regularity). At the end of the route, the competitor with the least points wins that stage. Events can have multiple stages over multiple days. Events can be onroad, offroad, or a mix. Vehicles typically aren’t classed as they aren’t the primary factor of success.

This rally style is suited nicely for production vehicles and public roads. Because of the relatively low speeds, safety requirements, event route closure, and vehicle requirements are low. Events can be run with a driver/co-driver pair.

## TDS+ Events
TDS+ is a new term designed to capture the essence of the TDS format but take it to a new level by adding modern technology. With modern GPS systems, it is possible to create new levels of competition and reduce the administrative burden of the event to near zero. This allows events to be low-cost, have a low administrative burden, and have a grassroots feel. A loosely coupled group of competitors could easily self-organize and run an event over a few days without having pre-positioned marshals on the course.

TDS+ is an example of a new breed of GIS-based competition paradigms like Strava for cycling, running/etc.

TDS+ events have the following characteristics:

- A Competition Director must be elected. The competition director is responsible for route creation and thus is not valid to compete in the event.
- Competitors must have a device capable of recording a GPS track for the day's route. Gaia GPS apps or Garmin GPS devices would be suitable. These apps could run on the navigator's phone or more dedicated GPS hardware. They must be able to record the route and produce a .gpx file of the day's route.
- Incomplete or missing .gpx files are scored at the maximum points of all stage competitors.
- Route books are generated similarly to TDS events, but all TCs are unknown to competitors during the event.
- The competitor must provide the .gpx file to the Competition Director at the end of a route. The Competition Director will then randomly choose TCs for the route and apply them to all scores, computing penalties (points) for each competitor.
- Each stage/route has a winner; the overall winner is the competitor with the lowest points for the event.
- Providing a paper route book at the start, keeping the TCs unknown for the event, and then post-event calculation of scores should almost eliminate the possibility of cheating.

## Computing Scores
Results are tallied after a route is run. A competitor provides the .gpx file from their run, and it’s then compared to a set of TCs created after all competitors are clear of the course.

A TC consists of a latitude/longitude tuple and an integer to indicate its TC number. The lat/lon pair should exist on top of the route.

`{ _tc: 1, lat:38.573315,  lon: -109.549843}`

A computer program iterates over each .gpx file, calculating the distance to each point using the Haversine Formula. The points closest to the TC point are chosen, and that point's GPS time is recorded as the official time the competitor passed the TC. This time is then compared to the optimal time for points calculation.

`<rtept lat="42.444773" lon="-71.108882">
 <ele>62.788800</ele>
 <time>2001-06-02T03:27:05Z</time>
 <name>6153</name>
 <desc><![CDATA[6153]]></desc>
 <sym>Dot</sym>
 <type><![CDATA[Intersection]]></type>
</rtept>`

In the GPX file, the `<time>` field is used for the official time based on the closest point to a TC using the `<rtept lat/lon>` fields.

There could be a slight drift in position based on the resolution of the GPS trace. This favors high-resolution GPS electronics but is likely tiny using modern GPS equipment. Competitors should set their GPS hardware to log as accurately and often as possible. Using some Euclidean distance scheme, a more accurate formula that detected the nearest point to the intersection of a line of two points could be envisioned.


