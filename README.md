# ONNESHA (Onnesha)

<<<<<<< HEAD
**অন্বেষা** is a coordinated UAV search-and-rescue system that helps responders locate people and animals, map disaster areas, and assess access routes. Its two-layer scan combines broad-area radiometric long-wave infrared (LWIR) presence screening with high-resolution RGB mapping and comparison against a supplied reference map. Cooperative UAVs share results over an airborne mesh, with a gateway and relay aircraft extending connectivity to a van or remote command post.
=======
**ONNESHA** is a coordinated UAV search-and-rescue system that helps responders locate people and animals in disaster zones and remote terrain. It combines radiometric long-wave infrared (LWIR) sensing, visible imagery, onboard AI, cooperative drone searches, and an airborne communications network.
>>>>>>> 740453803c2157d87e910ae9700280c69a919819

> **Project scope:** Civilian search and rescue, including locating lost hikers and tourists. This plan does not cover military targeting or locating people as adversaries.

## Project goals

- Search wide areas more quickly by dividing coverage among multiple UAVs.
- Detect candidate human and animal heat signatures from radiometric LWIR data.
- Use high-resolution RGB imagery to help operators review candidate detections.
- Report estimated target coordinates with an explicit location uncertainty.
- Show responders which areas have been searched, which remain, and where coverage may be obstructed.
- Extend communications beyond the ground station using airborne mesh relays and, where available, satellite backhaul.
- Generate a high-resolution RGB map of the designated area and compare it with a supplied reference map to flag visible changes to roads, bridges, and access routes.

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

## Two-layer scan workflow

The mission uses two complementary scan layers. The operator can launch both layers over the whole area or run the broad scan first and direct detailed mapping toward priority areas, candidate sightings, or suspected infrastructure damage.

### Layer 1: broad-area presence scan

- Search UAVs fly at a selected mid or high altitude appropriate to the terrain, thermal camera footprint, minimum target pixel requirement, and operating limits.
- Radiometric LWIR frames are analyzed onboard to flag possible human or animal heat signatures.
- The system georeferences scan coverage and detections, then updates the ground-station map with searched, unsearched, obstructed, and uncertain areas.
- Candidate sightings are sent with time, confidence, estimated coordinates, uncertainty area, and a thermal crop.
- Where equipped, a synchronized microphone array listens for calls for help, screams, and whistles. Because rotor and wind noise may mask voices, high-altitude acoustic coverage is experimental and must be validated separately from LWIR coverage.
- A candidate or low-confidence region can trigger a lower-altitude or alternate-angle pass for closer inspection.

### Layer 2: high-resolution RGB mapping and change assessment

- RGB-equipped UAVs fly planned mapping passes over the selected area to create a high-resolution, georeferenced map or orthomosaic.
- The operator supplies a reference map or prior imagery for the same area, with its source and date recorded.
- The system aligns current imagery with the reference map, then detects and highlights visible changes in roads, bridges, crossings, trails, and other responder access routes.
- Each flagged change includes a map location, imagery evidence, confidence, and review status. The dashboard distinguishes likely blocked or damaged routes from areas where image quality or alignment is insufficient.
- Repeat RGB mapping can show changes over time, such as new flooding, debris, or a route becoming inaccessible.

The RGB map can be generated for the full mission area, but coverage depends on flight time, image overlap, resolution, visibility, and data-processing capacity. A route flagged as visually unchanged is not guaranteed safe; the map supports responder assessment and should be checked against current ground reports.

### Acoustic follow-up mode

An acoustic event can trigger a targeted verification pass. This is a follow-up mode in addition to the two planned scan layers: an eligible UAV moves to a safe, lower-altitude listening position, gathers repeated bearings, and uses LWIR/RGB views where they can see the source. The draft 15–30 m AGL range is a test envelope, not a fixed operating altitude. Rotor speed must remain within the aircraft's stable flight limits; a multirotor must not reduce rotor speed below what safe flight requires. If a speaker is fitted, the operator can approve a short rescue message and listen for a response.

Acoustics may help identify a voice when foliage, darkness, or partial obstruction limits cameras, but sound does not reliably penetrate dense canopy or rubble. Detection range varies with source loudness, terrain, vegetation, wind, aircraft noise, and source depth. Acoustic alerts supplement visual and thermal search; they do not certify that hidden spaces are clear.

## Sensors and their roles

| Sensor or data source | What it does |
|---|---|
| **Radiometric LWIR thermal camera** | Captures per-pixel thermal measurements and identifies candidate heat signatures, including in low visible light. Radiometric frames and calibration information are retained for analysis. |
| **High-resolution RGB camera** | Supports candidate review and captures overlapping georeferenced imagery for high-resolution mapping. Current imagery is compared with a supplied reference map to flag visible changes to roads, bridges, crossings, trails, and responder access routes. |
| **GNSS receiver** | Provides aircraft position and time used in mapping and target geolocation. |
| **IMU and compass** | Measure aircraft orientation and motion so the system can estimate where a camera pixel falls on the ground. |
| **Altimeter** | Provides height information used to estimate camera footprint and target position. |
| **Obstacle and terrain sensing** | Supports safer navigation around trees, structures, and terrain, subject to the capabilities of the selected airframe and sensor package. |
| **Motor telemetry and audio reference microphones (optional)** | Provides rotor RPM for adaptive filtering; reference microphones near propulsion arms can capture correlated aircraft noise for experimental cancellation. |
| **Synchronized microphone array (optional)** | A 4–8 channel digital array captures calls for help, shouting, screams, and whistles. A compact planar or 3D layout can estimate sound direction; channel synchronization and installation calibration are required. These are prototype design targets, not a selected hardware specification. Initial studies can compare a circular array with roughly 10–15 cm radius against a compact 3D layout. Treat array size as a prototype choice to validate against payload weight, prop wash, noise, and localization performance. Synchronized 16–48 kHz sampling with sub-microsecond inter-channel timing is a design target to evaluate, not a guaranteed requirement for every implementation. Use weather protection and vibration isolation, then measure wind and airframe noise; foam or fur windscreens need aerodynamic and acoustic testing before flight use. |
| **Environmental sensing (optional)** | Records relevant conditions such as temperature, humidity, and wind to inform flight limits and interpretation of thermal performance. |
| **Radio-link telemetry** | Measures mesh and backhaul health to help position relays and identify disconnected aircraft. |
| **External map and incident data** | Supplies boundaries, terrain, flood information, hazards, last-known positions, and responder locations where available. |

A target coordinate is an estimate, not an exact point. It is calculated from the image location, camera calibration and alignment, aircraft position and attitude, and altitude. Reports should include an uncertainty area and the quality of the positioning data.

## AI and data pipeline

1. **Acquire and synchronize:** Capture LWIR and RGB frames with timestamps, aircraft pose, altitude, camera orientation, and sensor status.
2. **Check data quality:** Flag blur, obstructed views, calibration issues, weak GNSS, and thermal conditions that may make detection unreliable.
3. **Find thermal candidates:** Identify possible people or animals using thermal contrast, shape, apparent size, and surrounding context.
4. **Track across frames:** Check whether each candidate persists or moves across consecutive frames to reduce transient noise and reflection-related false alarms.
5. **Suppress aircraft noise:** Use motor RPM or ESC telemetry to track blade-pass tones and harmonics for adaptive notch filtering. Evaluate reference microphones and adaptive beamforming/noise cancellation against real flight recordings; suppression must not remove human vocal signals.
6. **Detect vocal events:** Run voice-activity detection and an onboard audio classifier for distress calls, screams, and whistles, with negative classes for wind, water, wildlife, and rotor noise. This detects event types; it does not identify speakers or transcribe conversations.
7. **Estimate sound direction:** Use synchronized channels and a calibrated array with a method such as SRP-PHAT, MUSIC, or validated beamforming to estimate azimuth/elevation and confidence. Report an angular bearing cone when range is unknown.
8. **Georeference the bearing:** Rotate the body-frame direction into the global navigation frame using synchronized IMU/compass pose, then intersect a valid downward ray with terrain elevation data when geometry supports it. Otherwise retain a bearing cone. Combine bearings from separated safe positions for triangulation and propagate GNSS, attitude, array-calibration, and angular errors into the uncertainty region.
9. **Associate visible imagery:** Link a candidate to its corresponding RGB crop when that view is usable.
10. **Fuse candidate evidence:** Associate detections by time and overlapping uncertainty regions. Keep acoustic bearing, thermal candidate, and RGB evidence separate in the report so operators can judge corroboration without treating one sensor as automatic confirmation.
11. **Send compact alerts:** Prioritize candidate coordinates or bearing cone, uncertainty, time, aircraft ID, confidence, and event class. Keep full-resolution imagery and audio onboard unless requested or bandwidth permits. A short, approximately 3-second compressed audio preview may be attached when intelligible and within the link budget; keep raw multi-channel audio onboard by default.
12. **Merge fleet reports:** Identify likely duplicate sightings from overlapping passes while preserving each contributing observation.
13. **Request review or another pass:** Let an operator classify a candidate as confirmed, rejected, or needing verification; dispatch a second UAV when appropriate.
14. **Build and compare maps:** Stitch georeferenced RGB images into a high-resolution map, align it with the supplied reference map, and detect visible changes to roads, bridges, and access routes. Flag uncertain alignment or image quality for operator review.
15. **Evaluate and improve:** Preserve reviewed examples and mission conditions for offline model assessment and retraining.

AI output is a **candidate alert**, not a final rescue decision. Thermal reflections, wet surfaces, environmental conditions, acoustic noise, and occlusion can cause errors. Rotor and wind noise can overwhelm a drone microphone; acoustic range and localization accuracy must be measured on the actual airframe across flight modes. Sound may be detectable through openings or partial obstruction, but there is no guarantee of detection beneath rubble or dense canopy. A non-detection from any sensor does not prove that an area is empty. Operators review alerts and can request another pass.

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
- Show Layer 1 LWIR coverage and Layer 2 RGB mapping coverage separately, including searched, unsearched, obstructed, and low-confidence areas.
- Display the supplied reference map, current high-resolution RGB map, change overlays, flight sectors, routes, hazards, flood boundaries, terrain, launch points, and responder locations as map layers.
- Show coverage time, altitude, sensor, resolution, and data quality for each map cell.
- Provide a swipe or side-by-side comparison of reference and current imagery, with reviewable road and bridge change alerts.

### Fleet management

- Show each UAV's position, role, route, battery, sensor state, and communications health.
- Assign, pause, resume, and reassign search sectors and priorities.
- Show mesh topology, relay paths, gateway status, and stale or missing telemetry.
- Alert operators about weak links, low battery, sensor problems, and lost aircraft communications.

### Detection review and responder handoff

- Display candidate locations with confidence, uncertainty area, timestamp, aircraft ID, and thermal/RGB previews.
- Show acoustic alerts as distinct map markers with event class (for example, HELP_CALL, WHISTLE, or SCREAM), confidence, time, aircraft, bearing cone or uncertainty region, and the observation history.
- Let an authorized operator play a short compressed audio preview; keep raw multi-channel recordings onboard by default and apply a defined retention policy.
- Support operator-approved two-way rescue messages from a fitted speaker, with response audio treated as a new candidate event.
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

Dense foliage can block the camera's view of the forest floor. LWIR cannot see through solid leaves or branches. Acoustic events may help prioritize a canopy-obstructed sector when a voice or whistle reaches the array, but foliage and rotor noise can also attenuate or mask the sound. The map must therefore mark canopy-obstructed ground as **not fully searched**, and a thermal non-detection must not be treated as proof that nobody is present. Safe lower passes, different angles, ground teams, or other search methods may be needed.

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
2. **Acoustic bench and flight prototype:** Record synchronized array data across rotor speeds and wind conditions; evaluate RPM-informed filtering, adaptive cancellation, vocal-event classification, and bearing estimates against surveyed sound sources.
3. **Mission map:** Add boundary drawing, sector planning, and searched/unsearched coverage visualization.
4. **Two-search-UAV trial:** Test cooperative coverage, overlap, duplicate sightings, and reassignment.
5. **Gateway and mesh trial:** Forward fleet alerts to the ground station and test lost-link storage and recovery.
6. **Relay and satellite trial:** Extend range in remote terrain and measure useful throughput, delay, and availability.
7. **Field validation:** Test known targets in open terrain, flood-like conditions, and forest environments; compare location estimates to surveyed positions.
8. **Operational pilot:** Train operators and responders, document procedures, and measure mission performance.

## Measures of success

- Fraction of the designated area covered with usable sensor data.
- Time to cover an area and time from candidate detection to operator alert.
- Candidate detection and false-alarm rates under recorded conditions.
- Acoustic event detection, false-alarm rate, bearing error, and useful range across rotor speed, wind, terrain, wildlife, and source-distance conditions.
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
- Microphone-array design, rotor-noise mitigation, array calibration, audio retention, and privacy policy.
- Edge-computer capability and model deployment approach.
- Local aviation, privacy, data-retention, and emergency-response requirements.

## References

- [ITU-T F.749.18: Framework and requirements for emergency services using civilian UAVs](https://www.itu.int/epublications/publication/itu-t-f-749-18-2024-06-framework-and-requirements-for-emergency-services-using-civilian-unmanned-aerial-vehicles)
- [NASA: Search and Rescue under the Forest Canopy using Multiple UAS](https://ntrs.nasa.gov/citations/20200002819)
- [Estimating ground surface visibility on thermal images from drone wildlife surveys in forests](https://www.sciencedirect.com/science/article/pii/S1574954123004089)
- [Design of UAV-Embedded Microphone Array System for Sound Source Localization in Outdoor Environments](https://www.mdpi.com/1424-8220/17/11/2535)
- [Acoustic event detection for drone search and rescue with rotor-noise suppression](https://doi.org/10.1016/j.dsp.2024.104881)
- [Waseda University: drone microphone-array research for locating voices and whistles in disaster rubble](https://www.waseda.jp/inst/research/news-en/56369)
