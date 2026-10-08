# ONNESHA (Onnesha)

**ONNESHA** is a coordinated UAV search-and-rescue system that helps responders locate people and animals in disaster zones and remote terrain. It combines radiometric long-wave infrared (LWIR) sensing, visible imagery, onboard AI, cooperative drone searches, and an airborne communications network.

> **Project scope:** Civilian search and rescue, including locating lost hikers and tourists. This plan does not cover military targeting or locating people as adversaries.

## Project goals

- Search wide areas more quickly by dividing coverage among multiple UAVs.
- Detect candidate human and animal heat signatures from radiometric LWIR data.
- Use high-resolution RGB imagery to help operators review candidate detections.
- Report estimated target coordinates with an explicit location uncertainty.
- Show responders which areas have been searched, which remain, and where coverage may be obstructed.
- Extend communications beyond the ground station using airborne mesh relays and, where available, satellite backhaul.
- Support responders with flood, terrain, and hazard information in addition to sightings.

## System overview

```text
Search and verification UAVs
             ⇅
       Airborne mesh
             ⇅
Gateway / relay UAVs ───── Satellite backhaul (optional)
             ⇅                         ⇅
        Ground station in response van / remote command post
```

UAV roles are mission assignments rather than separate aircraft types. A UAV may scan, verify a candidate, or serve as a gateway or relay when its equipment and battery allow. If the gateway becomes unavailable, an eligible UAV should be able to take over. Every aircraft retains its local flight plan and safety behavior if communications are interrupted.

## Sensors and their roles

| Sensor or data source | What it does |
|---|---|
| **Radiometric LWIR thermal camera** | Captures per-pixel thermal measurements and identifies candidate heat signatures, including in low visible light. Radiometric frames and calibration information are retained for analysis. |
| **High-resolution RGB camera** | Provides visible detail to help an operator assess candidate detections, trails, clearings, obstacles, and scene context when visibility and lighting permit. |
| **GNSS receiver** | Provides aircraft position and time used in mapping and target geolocation. |
| **IMU and compass** | Measure aircraft orientation and motion so the system can estimate where a camera pixel falls on the ground. |
| **Altimeter** | Provides height information used to estimate camera footprint and target position. |
| **Obstacle and terrain sensing** | Supports safer navigation around trees, structures, and terrain, subject to the capabilities of the selected airframe and sensor package. |
| **Environmental sensing (optional)** | Records relevant conditions such as temperature, humidity, and wind to inform flight limits and interpretation of thermal performance. |
| **Radio-link telemetry** | Measures mesh and backhaul health to help position relays and identify disconnected aircraft. |
| **External map and incident data** | Supplies boundaries, terrain, flood information, hazards, last-known positions, and responder locations where available. |

A target coordinate is an estimate, not an exact point. It is calculated from the image location, camera calibration and alignment, aircraft position and attitude, and altitude. Reports should include an uncertainty area and the quality of the positioning data.

## AI and data pipeline

1. **Acquire and synchronize:** Capture LWIR and RGB frames with timestamps, aircraft pose, altitude, camera orientation, and sensor status.
2. **Check data quality:** Flag blur, obstructed views, calibration issues, weak GNSS, and thermal conditions that may make detection unreliable.
3. **Find thermal candidates:** Identify possible people or animals using thermal contrast, shape, apparent size, and surrounding context.
4. **Track across frames:** Check whether each candidate persists or moves across consecutive frames to reduce transient noise and reflection-related false alarms.
5. **Associate visible imagery:** Link a candidate to its corresponding RGB crop when that view is usable.
6. **Estimate confidence and location:** Produce a confidence score, a location estimate, and an uncertainty area using camera geometry and aircraft navigation data.
7. **Send compact alerts:** Prioritize candidate coordinates, uncertainty, time, aircraft ID, confidence, and small image crops. Keep full-resolution imagery onboard unless requested or bandwidth permits.
8. **Merge fleet reports:** Identify likely duplicate sightings from overlapping passes while preserving each contributing observation.
9. **Request review or another pass:** Let an operator classify a candidate as confirmed, rejected, or needing verification; dispatch a second UAV when appropriate.
10. **Evaluate and improve:** Preserve reviewed examples and mission conditions for offline model assessment and retraining.

AI output is a **candidate sighting**, not a final rescue decision. Thermal reflections, wet surfaces, environmental conditions, and occlusion can cause errors. An operator reviews detections and can request another pass.

## Search and mission behavior

- Draw or import a search boundary and mark hazards, restricted areas, launch locations, and responder positions.
- Divide the area into sectors based on camera footprint, altitude, flight endurance, weather, terrain, and communications.
- Plan overlapping flight lines and record the time and quality of coverage for each map cell.
- Mark obstructed or low-quality coverage as incomplete instead of treating a flyover as a successful search.
- Continue scanning assigned areas while another aircraft investigates a candidate where fleet capacity allows.
- Reassign unfinished sectors if an aircraft returns early, loses a sensor, or leaves the mission.
- Revisit high-priority sightings, hazards, and changing flood conditions.
- Reserve battery for return, landing, or other configured safety actions.

## Ground station

The ground station runs in the response van when accessible, or at a remote command post. Its primary view is a shared map of the entire incident area.

### Map and coverage

- Draw, edit, or import the search boundary.
- Show searched, unsearched, obstructed, and low-confidence coverage areas.
- Display flight sectors, routes, hazards, flood boundaries, terrain, launch points, and responder locations as map layers.
- Show coverage time, altitude, and data quality for each searched cell.

### Fleet management

- Show each UAV's position, role, route, battery, sensor state, and communications health.
- Assign, pause, resume, and reassign search sectors and priorities.
- Show mesh topology, relay paths, gateway status, and stale or missing telemetry.
- Alert operators about weak links, low battery, sensor problems, and lost aircraft communications.

### Detection review and responder handoff

- Display candidate locations with confidence, uncertainty area, timestamp, aircraft ID, and thermal/RGB previews.
- Let operators mark sightings as confirmed, rejected, or requiring further review.
- Dispatch a verification or monitoring pass.
- Share approved location updates and approach information with response teams.
- Export incident maps and reports with observation history and operator decisions.

The dashboard should clearly distinguish current information from cached or stale information during a link outage.

## Communications and extended range

- Search UAVs share mission updates, telemetry, coverage state, and detection alerts over an airborne mesh.
- A **gateway UAV** bridges the mesh to the van or command post.
- One or more **relay UAVs** can extend the network when distance or terrain blocks a direct link to the gateway.
- A gateway or long-endurance relay may use satellite backhaul where service and equipment are available.
- The network routes around a weak or failed node where possible; an eligible UAV can assume gateway duties.
- Aircraft store data locally during outages and synchronize when connectivity returns.
- Alerts and safety telemetry take priority over full-resolution video; imagery is transferred selectively.
- Flight stabilization, mission limits, and failsafe actions remain onboard and do not depend on the mesh or satellite link.

Satellite service is an optional backhaul path. Coverage, bandwidth, delay, antenna requirements, power, and payload weight need field validation for the selected service.

## Jungle and lost-person search mode

For civilian searches for missing hikers or tourists, the mission planner can use last-known position, intended route, elapsed time, reports, trails, water sources, clearings, shelters, and terrain to prioritize sectors. It can plan alternate viewing directions, identify canopy gaps, and send another aircraft to inspect uncertain candidates.

Dense foliage can block the camera's view of the forest floor. LWIR cannot see through solid leaves or branches. The map must therefore mark canopy-obstructed ground as **not fully searched**, and a thermal non-detection must not be treated as proof that nobody is present. Safe lower passes, different angles, ground teams, or other search methods may be needed.

## Additional rescue-support features

- Map flood extent and compare repeat passes to identify changes.
- Flag visible hazards such as blocked roads, damaged crossings, fire, debris, or unstable structures.
- Monitor a confirmed location while responders travel to it.
- Provide an aerial overview to improve incident situational awareness.
- Use an optional speaker or spotlight to attract attention or communicate a simple responder-approved message, subject to payload and operating constraints.
- Maintain an incident log of detections, routes, hazards, network status, and operator decisions.

## Safety and operating principles

- Keep a human operator responsible for mission oversight and detection confirmation.
- Configure geofences, altitude and area limits, battery reserves, lost-link behavior, and return or landing procedures.
- Do not make safe flight depend on mesh connectivity, satellite backhaul, or remote AI availability.
- Plan for GNSS degradation, sensor failure, poor weather, relay loss, and gateway replacement.
- Show uncertainty, sensor quality, and data age alongside every operational alert.
- Coordinate flights with local aviation rules, emergency services, and incident command.

## Development roadmap

1. **Single-UAV sensing prototype:** Capture calibrated LWIR and RGB data, run candidate detection, and display location uncertainty.
2. **Mission map:** Add boundary drawing, sector planning, and searched/unsearched coverage visualization.
3. **Two-search-UAV trial:** Test cooperative coverage, overlap, duplicate sightings, and reassignment.
4. **Gateway and mesh trial:** Forward fleet alerts to the ground station and test lost-link storage and recovery.
5. **Relay and satellite trial:** Extend range in remote terrain and measure useful throughput, delay, and availability.
6. **Field validation:** Test known targets in open terrain, flood-like conditions, and forest environments; compare location estimates to surveyed positions.
7. **Operational pilot:** Train operators and responders, document procedures, and measure mission performance.

## Measures of success

- Fraction of the designated area covered with usable sensor data.
- Time to cover an area and time from candidate detection to operator alert.
- Candidate detection and false-alarm rates under recorded conditions.
- Target location error and reported uncertainty calibration.
- Mesh availability, data-delivery delay, and recovery after outages.
- Safe mission completion and return rate.
- Percentage of forest area correctly marked as visible versus canopy-obstructed.

## Open design decisions

- Airframe and payload capacity for search, gateway, and relay roles.
- Fleet size, battery endurance, and required sector overlap.
- Radio bands, mesh protocol, encryption, and supported backhaul options.
- Satellite service and antenna placement, if required.
- RGB and thermal camera alignment, stabilization, and calibration method.
- Edge-computer capability and model deployment approach.
- Local aviation, privacy, data-retention, and emergency-response requirements.

## References

- [ITU-T F.749.18: Framework and requirements for emergency services using civilian UAVs](https://www.itu.int/epublications/publication/itu-t-f-749-18-2024-06-framework-and-requirements-for-emergency-services-using-civilian-unmanned-aerial-vehicles)
- [NASA: Search and Rescue under the Forest Canopy using Multiple UAS](https://ntrs.nasa.gov/citations/20200002819)
- [Estimating ground surface visibility on thermal images from drone wildlife surveys in forests](https://www.sciencedirect.com/science/article/pii/S1574954123004089)
