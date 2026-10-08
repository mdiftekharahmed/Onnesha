# Existing Projects Relevant to Onnesha

**Research note and project comparison**  
**Reviewed:** 9 October 2026  
**Scope:** Publicly documented research projects, operational mapping services, datasets, and technical papers relevant to Onnesha's multi-UAV search, acoustic localization, forest exploration, and infrastructure assessment goals.

## Executive summary

The attached comparison identifies real work in several useful areas, but some descriptions merge demonstrated results, project goals, and proposed Onnesha features. Based on the public primary sources reviewed here, no single system was found that documents all four capabilities together: fleet-level two-layer UAV search; onboard acoustic victim localization; high-resolution RGB mapping; and automated reference-map comparison for access-route changes.

The closest precedents are complementary:

- **Fraunhofer FKIE LUCY** is the closest match for a drone-mounted microphone array intended to detect and direction-find cries, shouts, and other victim sounds. Its public page describes experimental systems and evaluation; it does not establish a finished operational product or the exact accuracy numbers in the pasted text.
- **SHERPA** is a strong precedent for heterogeneous, hierarchical rescue robotics: broad-area aerial platforms, smaller rotary-wing UAVs, a ground rover, and a human operator. It does not document the exact automatic thermal-acoustic macro-to-micro workflow attributed to it.
- **NASA/MIT forest-canopy search** is a strong precedent for collaborative UAV exploration and mapping beneath a tree canopy, including GPS-denied operation and central collaborative SLAM. It is not a thermal/acoustic person detector and does not use the claimed high-altitude-to-low-altitude cueing architecture.
- **Copernicus Emergency Management Service (CEMS) Rapid Mapping** is an operational reference for pre-event reference maps, event delineation, and damage-grading products. It is an on-demand mapping service, primarily based on satellite and other available data, rather than a tactical onboard UAV change-detection pipeline.
- **AFTERMAP** is a recent UAV imagery dataset for building-damage instance segmentation. It does not label road or bridge condition and is not a paired pre/post map-change benchmark. Its public repository describes restricted dataset access.
- **CURSOR** is a closed EU search-and-rescue project combining aerial systems, ground robots, geophones, gas sensing, communications, and a common operational picture. It is a broad integration precedent, but its acoustic sensors were primarily ground-deployed rather than a drone-borne voice-localization array.
- **RescueNet** is a closer imagery match for disaster roads: its UAV labels include `Road-Clear` and `Road-Blocked`, as well as water, vehicles, trees, and building damage. It is semantic segmentation of post-disaster imagery, not reference-map change detection.

The main design lesson is to combine these precedents as separate subsystems and validate their interfaces. Acoustic detection should be treated as an experimental cueing channel until rotor-noise performance, bearing accuracy, range, and false alarms have been measured on Onnesha's actual aircraft.

## How to read this report

The descriptions below separate **what sources say exists or was tested** from **what a project aims to do** and from **what the pasted comparison attributes without clear evidence**. “No public code found” means no code link was identified in the primary project pages reviewed; it does not prove that code does not exist elsewhere.

Project status and access details reflect the linked pages as reviewed on 9 October 2026. A research result should not be assumed to be a certified, field-ready product.

## 1. Acoustic drone localization and human-voice detection

### 1.1 Fraunhofer FKIE LUCY

**Full name:** Listening system Using a Crow’s nest arraY.  
**Organization:** Fraunhofer Institute for Communication, Information Processing and Ergonomics FKIE, Germany.  
**Status:** Research and experimental evaluation; Fraunhofer describes LUCY as under development and reports test configurations.

LUCY uses an irregular three-dimensional arrangement of MEMS microphones carried by a drone to estimate the direction or location of sounds such as calls for help and impulsive shouts. Fraunhofer reports successful direction-finding and pinpointing tests on an experimental 32-microphone system and says a modular 64-microphone demonstrator is being built. The public project page presents combining LUCY with camera/thermal sensors and using multiple drones as possible extensions, rather than as a completed fleet system.

**Relevance to Onnesha:** The closest identified precedent for the proposed airborne acoustic sensor. It supports exploring volumetric arrays, direction finding, noise suppression, modular payloads, and fusion with thermal/visual sensors.

**Limits and corrections:** The public page does not establish the pasted text’s specific LUCY pipeline: it does not specify a deployed distress-sound neural network, exact SRP-PHAT/MUSIC implementation, a 15–30 m operating altitude, or 2–5 m horizontal accuracy. Treat those as design hypotheses requiring separate evidence and trials. LUCY’s public description emphasizes direction finding; range and ground coordinates require additional geometry or repeated observations.

**Links:** [Fraunhofer FKIE LUCY page](https://www.fkie.fraunhofer.de/en/departments/sdf/lucy.html) · [Fraunhofer overview: “A drone with ears”](https://www.fraunhofer.de/en/press/research-news/2023/december-2023/a-drone-with-ears.html)

### 1.2 “Southampton Acoustic Drone” claim

The pasted text attributes a drone-borne acoustic array for avalanche victims to the University of Southampton and gives a 15–30 m altitude and 2–5 m horizontal-accuracy result. I could not verify a University of Southampton project matching that name or those results in the official university materials located for this review. The Southampton pages found describe microphone-array and audio research, but not the specified airborne avalanche-rescue system or those figures.

This item should be cited as **unverified**, not presented as a confirmed project or benchmark, until an original paper, lab page, thesis, or project record is supplied. It may be a conflation with other drone-audition research, including the studies below.

**Links:** [Southampton: Next Generation Recording Technology](https://www.southampton.ac.uk/vaae/research/projects/vaae-next-generation-recording-technology.page) · [Southampton microphone-array research](https://www.southampton.ac.uk/study/postgraduate-research/projects/cutting-edge-microphone-arrays-for-improving-smart-device)

### 1.3 Drone-embedded microphone-array research

These papers are useful technical precedents even though they are not complete multi-UAV disaster-response products.

- **Hoshiba et al., “Design of UAV-Embedded Microphone Array System for Sound Source Localization in Outdoor Environments” (2017).** Describes an airborne array and evaluates direction localization with drone noise and outdoor voice/whistle sources. It demonstrates the core processing problem; it does not validate the pasted 2–5 m result at 15–30 m altitude as a general specification. [Paper](https://www.mdpi.com/1424-8220/17/11/2535) · [Open full text](https://pmc.ncbi.nlm.nih.gov/articles/PMC5713044/)
- **DOANet (2020).** Studies an eight-channel cube-shaped array, drone ego-noise, and angle estimation, with methods including GCC-PHAT and neural direction-of-arrival estimation. It is a signal-processing research contribution, not an integrated fleet product. [Journal article](https://link.springer.com/article/10.1186/s13636-020-00184-2)
- **“Drone Audition: Sound Source Localization Using On-Board Microphones” (2022).** Uses an irregular array embedded in a drone and studies DOA estimation under severe motor/propeller noise, including low signal-to-drone-noise conditions. [Repository record and abstract](https://openresearch-repository.anu.edu.au/items/89d987f1-2d0f-4df5-87ef-0d0fbc49f623)
- **Rotor-noise suppression for drone SAR acoustic events (2025).** Proposes BLSTM-MVDR beamforming and evaluates voice-event detection across directions, distances, and signal-to-noise conditions. It reinforces that aircraft noise is central to the problem. [Paper DOI](https://doi.org/10.1016/j.dsp.2024.104881)
- **Sky-Ear (2026 preprint).** Proposes a circular array and two-stage “Sentinel/Responder” processing with multi-observation localization. It reports simulation experiments; it should not be described as field-validated hardware. [arXiv preprint](https://arxiv.org/abs/2604.12455)
- **DroneAudioSet (2025 preprint/dataset).** Provides annotated drone-audio recordings across drone types, throttle settings, microphone layouts, and environments. It can help bootstrap noise-aware models, but Onnesha still needs representative local recordings of voices, whistles, water, wildlife, wind, and its own aircraft. [Dataset](https://huggingface.co/datasets/ahlab-drone-project/DroneAudioSet) · [Paper](https://arxiv.org/abs/2510.15383)

### Acoustic-design implications for Onnesha

1. Keep acoustic detection separate from coordinate estimation. A microphone array commonly estimates a bearing first; a single bearing generally does not supply range. Estimate a ground location only when geometry supports it, or combine observations from separated known positions. Preserve an angular cone or uncertainty region when range is unknown.
2. Record synchronized channels and flight telemetry, including motor RPM/ESC data where available. Evaluate RPM-informed filtering, reference-microphone cancellation, beamforming, and learned noise suppression on actual flight recordings. Suppression can also erase the target signal, so test detection and localization after filtering.
3. Treat microphone layout, channel count, sample rate, synchronization, wind protection, and isolation as **prototype design variables**, not proven requirements. Measure array response, mass, aerodynamic noise, and localization error on the airframe.
4. Do not assume “low-RPM hover” is a safe or quiet operating mode. Multirotors need sufficient thrust to remain controllable. Define a safe listening maneuver and measure its acoustic benefit before operational use.
5. Store raw audio locally under a clear retention policy. Send a compact event tag, time, aircraft ID, bearing/uncertainty, and a short preview only when the link budget and privacy rules allow. Provide operator review before dispatching responders.

## 2. Hierarchical and forest multi-UAV search

### 2.1 EU FP7 SHERPA

**Full name:** Smart collaboration between Humans and ground-aErial Robots for imProving rescuing activities in Alpine environments.  
**Program/status:** EU Seventh Framework Programme; CORDIS lists the project as closed, 1 February 2013 to 31 March 2017.

SHERPA explored a heterogeneous alpine SAR team. Its system concepts used a human rescuer (“busy genius”), a ground rover (“intelligent donkey”), small rotary-wing UAVs (“trained wasps”), and larger aerial platforms (“patrolling hawks”). Small UAVs could operate near the rover and provide maneuverable local observation; larger platforms could patrol a broader area and provide an overview. The rover also served as a mobile support/docking platform for the small UAVs.

**Relevance to Onnesha:** A precedent for role specialization, human-supervised autonomy, mixed endurance/size classes, local re-tasking, and operators working from a shared mission system.

**Limits and corrections:** SHERPA supports heterogeneous multi-robot collaboration, but does not establish the exact workflow in the pasted text—automatic high-altitude thermal detection directly cueing quadrotors to verify victims beneath snow or canopy. It is better described as a layered team architecture and collaborative robotics research. No passive airborne microphone localization or automated road/bridge reference-map change detection is documented in the sources reviewed.

**Links:** [CORDIS project record/results](https://cordis.europa.eu/project/id/600958/results) · [CORDIS project description](https://cordis.europa.eu/project/id/600958) · [SHERPA architecture paper record](https://cris.unibo.it/handle/11585/257483)

### 2.2 NASA/MIT: Search and Rescue under the Forest Canopy using Multiple UAVs

**Organizations:** MIT and NASA Langley Research Center.  
**Publication:** International Journal of Robotics Research, 2020; reports simulation and collaborative exploration missions at NASA Langley.

This work addresses multi-UAV exploration and mapping under forest canopy, where GNSS and visual place recognition are difficult. Each UAV performs onboard sensing, local state estimation, and frontier-based exploration. When communications are available, vehicles send compressed tree-based submaps to a central ground station for collaborative SLAM. The system uses tree configurations as landmarks and cycle-consistent matching to reduce incorrect loop closures and improve map fusion.

**Relevance to Onnesha:** A strong precedent for canopy-aware operations, GPS-denied mapping, compact submap sharing, cooperative exploration, and a ground station fusing fleet observations.

**Limits and corrections:** This work is about exploration/localization/mapping, not a demonstrated acoustic or thermal human detector. The paper describes a central ground station for computationally intensive collaborative SLAM, so it should not be summarized as fully decentralized mapping. It does not establish the macro-altitude/low-altitude cueing sequence in the pasted text.

**Links:** [NASA report and paper](https://ntrs.nasa.gov/citations/20200002819) · [MIT CSAIL project page](https://www.csail.mit.edu/research/search-and-rescue-under-tree-canopy) · [MIT summary](https://www.csail.mit.edu/news/fleets-drones-could-aid-searches-lost-hikers) · [arXiv paper](https://arxiv.org/abs/1908.10541)

### 2.3 Related sub-canopy mapping research

NASA’s sub-canopy UAS work targets forest-fuel mapping rather than victim search. It combines LiDAR, RGB, GNSS/INS, mapping, and trajectory planning for flight beneath canopy. This is relevant to safe sub-canopy navigation and map-building, but is not a search-and-rescue detection system. [NASA Earth Science and Technology Office project](https://esto.nasa.gov/firetech/subcanopy-uas-development/)

## 3. Post-disaster mapping, reference maps, and infrastructure assessment

### 3.1 Copernicus Emergency Management Service Rapid Mapping

CEMS is an operational EU service providing geospatial information for emergency response. Rapid Mapping products include:

- **Reference maps:** pre-event background features such as transport networks, settlements, and infrastructure.
- **Delineation maps:** geographic event extent, such as flooded or burned areas.
- **Grading maps:** damage assessment based on pre-event and post-event information.

Products are generated from satellite imagery and other available geospatial data. Delivery can take hours or days depending on activation, acquisition, and product needs. Manuals explain product content, production methods, and quality control.

**Relevance to Onnesha:** A reference for map layers, pre-event baselines, event extent, grading, metadata, delivery, and quality control. The dashboard can separate a reference layer, current observations, event extent, and assessed damage/access status.

**Limits and corrections:** CEMS is not generally an onboard, real-time edge pipeline running over an airborne mesh. The public material does not support the claim that it automatically flags each severed bridge or blocked road from a tactical UAV orthomosaic. Individual route condition is a separate image-analysis and human-review task; event maps are not a substitute for on-scene verification.

**Links:** [Rapid Mapping Manual](https://mapping.emergency.copernicus.eu/about/rapid-mapping-manual/) · [CEMS overview](https://emergency.copernicus.eu/about/) · [Technical manual PDF](https://emergency.copernicus.eu/mapping/sites/default/files/files/JRCTechnicalReport_2020_Manual%20for%20Rapid%20Mapping%20Products_final.pdf) · [EU legal description of products](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32018D0620)

### 3.2 AFTERMAP

**Full name:** A FEMA-aligned dataset for damaged-structure segmentation in UAV imagery.  
**Status:** Recent 2026 publication and associated GitHub repository; the repository says the complete dataset is not redistributed there and access may be requested for non-commercial academic work.

The paper describes 1,926 high-resolution UAV images with 8,363 annotated polygon instances across five classes: destroyed, major damage, minor damage, tarp, and no damage. It evaluates instance-segmentation models for post-disaster **building** damage.

**Relevance to Onnesha:** A reference for building-damage labels and FEMA-oriented assessment on close-range UAV imagery. It may inform a structural-damage layer if access and licensing permit.

**Limits and corrections:** AFTERMAP does not label road passability or bridge integrity and is not a paired pre-event/post-event change-detection benchmark. The paper notes GPS, altitude, and camera metadata are generally missing from source videos, limiting direct geospatial mapping. Full dataset access is restricted/request-based, so it is not a turnkey open training dataset. The pasted comparison overstates it as an open framework for road/bridge scoring.

**Links:** [AFTERMAP paper](https://www.mdpi.com/2075-5309/16/15/2943) · [GitHub repository](https://github.com/sultankennesaw-Civil/AFTERMAP-Dataset)

### 3.3 RescueNet

RescueNet is a high-resolution UAV semantic-segmentation dataset collected after Hurricane Michael. Its 4,494 images have classes including water, building damage, vehicles, trees, pools, **Road-Clear**, and **Road-Blocked**.

**Relevance to Onnesha:** A close match to the proposed RGB road-access layer. It offers training examples for clear versus blocked roads and disaster context from UAV imagery.

**Limits:** It provides post-disaster semantic labels, not an automated comparison with the same location’s pre-event reference map. It cannot certify bridge safety, structural capacity, or current passability; image conditions and local validation still matter.

**Links:** [RescueNet paper](https://arxiv.org/abs/2202.12361) · [GitHub repository](https://github.com/BinaLab/RescueNet-A-High-Resolution-Post-Disaster-UAV-Dataset-for-Semantic-Segmentation)

### 3.4 xBD / xView2

xBD is a satellite-imagery dataset with paired pre- and post-disaster imagery and building-level damage labels. It supports building damage assessment and change-oriented humanitarian response research.

**Relevance to Onnesha:** Useful reference for paired-image change workflows, geospatial metadata, disaster-diverse evaluation, and building-damage classes.

**Limits:** It is satellite imagery and building-focused. It does not supply UAV acoustic data, road-passability labels, or a bridge-condition model. Models trained at satellite scale need careful validation on UAV orthomosaics.

**Links:** [xBD paper](https://arxiv.org/abs/1911.09296) · [xView2 dataset](https://xview2.org/dataset) · [Microsoft reference implementation](https://github.com/microsoft/building-damage-assessment-cnn-siamese)

### Infrastructure-assessment implications for Onnesha

- Treat road, bridge, and route observations as **visible condition indicators**, not engineering load ratings. RGB imagery may flag a missing span, floodwater, debris, collapse, or visible obstruction; it cannot certify a bridge as safe for a particular vehicle.
- Align current imagery with the reference basemap and report registration confidence. A mismatched reference can create false change alerts.
- Separate classes such as `ROAD_CLEAR`, `ROAD_BLOCKED`, `FLOODED`, `BRIDGE_VISIBLE_DAMAGE`, and `UNKNOWN`; use `UNKNOWN` for shadows, occlusion, blur, water, or poor alignment.
- Use RescueNet-like segmentation as a starting point, then collect local imagery and labels for local roads, flood conditions, and camera geometry.
- Preserve original frames, georeferencing metadata, map-source date, and operator review state so responders can inspect alert evidence.

## 4. Integrated disaster-response consortia

### 4.1 EU H2020 CURSOR

**Full name:** Coordinated Use of miniaturized Robotic equipment and advanced Sensors for search and rescue OpeRations.  
**Status:** EU Horizon 2020 project closed 28 February 2023 after final demonstrations.

CURSOR developed a Search and Rescue Kit for collapsed-building response. Its published system included several drone roles (tethered mothership, ground-penetrating-radar drone, situational-awareness drone, transport drone, and modelling drones), SMURF ground robots, gas/VOC sensing, geophones, field communications, a common operational picture, and operator tools. The EU report describes aerial surveillance, photos, HD video, thermal imagery, and 3D modelling. Its geophone system was deployed on or near the ground; SMURF robots were intended to enter debris.

**Relevance to Onnesha:** The strongest broad systems-integration precedent in the pasted list. It informs role-based drones, command-center information fusion, air/ground collaboration, map products, relay roles, and sharing alerts across responders.

**Limits and corrections:** CURSOR’s acoustic sensing was not a drone-mounted microphone array doing voice direction finding. CORDIS describes ground-based geophones and a ground robot sniffer; it does not document the proposed airborne audio-localization subsystem. The report says “field communication solution”; don’t call it an airborne mesh without a source for that topology.

**Links:** [CORDIS final reporting](https://cordis.europa.eu/project/id/832790/reporting) · [Official project site](https://www.cursor-project.eu/) · [Large-scale field-test description](https://www.cursor-project.eu/large-scale-field-test-in-afidnes-greece-november-20-25-2022/)

## 5. Additional supporting project

### UAV-Rescue (Fraunhofer-led German/Austrian research)

UAV-Rescue combines radar, LiDAR, AI, and autonomous/semi-autonomous aerial exploration for hazardous indoor or debris environments. Public pages describe 3D situational mapping, hazard identification, person detection under difficult visibility, and radar-based vital-sign sensing research. Fraunhofer lists a project duration of 2021–2023; public pages summarize research rather than offer an end-user product or public codebase.

**Why it matters:** Relevant to sensor fusion and human detection under visual occlusion, especially inside structures or debris. Its radar/LiDAR focus differs from Onnesha’s wide-area LWIR/RGB/acoustic approach.

**Links:** [Fraunhofer FHR project page](https://www.fhr.fraunhofer.de/en/projects/03_industrial_applications/uav-rescue-uav-borne-sensor-systems-for-ai-based-support-of-rescue-missions.html) · [Fraunhofer EMI overview](https://www.emi.fraunhofer.de/de/geschaeftsfelder/sicherheit/forschung/uav-rescue-lebensrettung-aus-der-luft.html)

## 6. Comparative matrix

| Project / resource | Broad / multi-tier search | Airborne acoustic localization | Forest / sub-canopy work | Infrastructure mapping | Communications / operator system | Public artifact / maturity |
|---|---|---|---|---|---|---|
| **Fraunhofer LUCY** | Not a documented fleet architecture | **Core research:** microphone array and sound direction finding | Not the main scenario | No road/bridge change maps | Payload/sensor work; no integrated mesh product found | Experimental configurations under evaluation; official project page |
| **Southampton Acoustic Drone claim** | Unverified | Named system and quoted performance not verified | Not established | None identified | None identified | Southampton sources found concern general microphone-array research |
| **Hoshiba / DOANet / Drone Audition** | Single-UAV technical studies | **Core research:** array processing and DOA under ego-noise | Occlusion/night are motivations, not fleet forest trials | None | Research processing | Papers, no full operational fleet product |
| **SHERPA** | **Heterogeneous roles** and broad/local aerial platforms | No airborne voice array found | Alpine/rough terrain, not canopy mapping | No automated road/bridge change pipeline found | Human/ground/aerial collaboration | Closed EU project; CORDIS and publications |
| **NASA/MIT forest-canopy UAVs** | Cooperative exploration; not the claimed macro/micro tiers | No acoustic victim detection in cited system | **Core research:** GPS-denied canopy mapping/exploration | Forest map, not disaster-route assessment | Compressed submaps to central collaborative-SLAM station | Peer-reviewed paper and NASA record |
| **Copernicus EMS Rapid Mapping** | Satellite/aerial products, not a UAV fleet | None | None | **Operational event extent and grading maps**, reference products | Formal service and product delivery | Operational service/manuals; turnaround varies |
| **AFTERMAP** | No fleet search | None | None | Building-damage segmentation only | Dataset/model benchmark | 2026 paper; repository says request-based access |
| **RescueNet** | No fleet search | None | None | **UAV road-clear/road-blocked and disaster-scene segmentation** | Dataset/research code | 4,494-image dataset and public repository |
| **xBD / xView2** | No UAV fleet search | None | None | **Paired satellite building-damage assessment** | Dataset/research implementations | Research dataset and code resources |
| **CURSOR** | **Multi-role drone fleet plus ground robots** | Ground geophones; not a UAV microphone array | Not forest-focused | Aerial situational models and 3D mapping | **Common operational picture, field communications, tethered mothership** | Closed H2020 project; reports/demonstrations |
| **UAV-Rescue** | Semi-autonomous indoor exploration | No mic-array focus identified | Indoor/debris environments | 3D environment/hazard mapping, not route-map change | Human responder support | Fraunhofer research project |
| **Onnesha (proposed)** | Broad LWIR scan + full-area RGB map + targeted follow-up | Proposed array detection and bearing localization | Proposed canopy-aware search and acoustic cueing; unvalidated | Proposed reference/current RGB access-route flags | Proposed mesh, gateway/relay UAVs, satellite, van dashboard | Plan only; must be built and field-tested |

## 7. What the comparison means for Onnesha

### 7.1 The system is an integration project

The reviewed work provides precedents for each pillar: LUCY and drone-audition papers for microphones; SHERPA and NASA/MIT for multi-robot or under-canopy exploration; CEMS for incident-map products; RescueNet/xBD/AFTERMAP for image labels and damage assessment; CURSOR for an integrated responder-facing system. Onnesha’s main engineering challenge is connecting these functions in one reliable mission loop with shared geospatial metadata, confidence/uncertainty, data prioritization, and human review.

### 7.2 Practical architecture sequence

1. **Macro scan:** Use LWIR and onboard candidate detection for broad coverage; map the usable footprint and mark occluded/low-quality cells.
2. **RGB map pass:** Capture overlapping high-resolution imagery over the operational area; generate a georeferenced mosaic and compare it with a dated reference. Make route-condition outputs confidence-rated and reviewable.
3. **Acoustic cueing:** Use the array as a candidate cue where optical/thermal visibility is poor. Validate range and false alarms over rotor speed, wind, water, canopy, source direction, and distance.
4. **Targeted verification:** Dispatch an eligible drone to a safe lower-altitude position, gather repeated bearings, and combine sound direction with visible/thermal evidence where available. Preserve an uncertainty region if range cannot be solved.
5. **Responder handoff:** Deliver compact alerts over mesh to the gateway/relay and van dashboard. Show evidence, freshness, data quality, confidence, and uncertainty; keep raw media local unless needed.

This treats the two planned area-scan layers (LWIR presence coverage and RGB map/change coverage) separately from an acoustic follow-up maneuver. It avoids making “two layers” mean both “high/low altitude” and “thermal/RGB”; those are different operational dimensions.

### 7.3 Recommended evidence and validation plan

- Benchmark acoustic detection and bearing on the actual aircraft, first on a bench and then in controlled flight. Record microphone channels, motor telemetry, wind, source position, and surveyed ground truth.
- Report class-wise precision/recall and false alarms for voice, scream, whistle, wind, rotor, water, and wildlife. Report angular error and ground-position error separately.
- Use repeated observations or separated UAV locations for triangulation. Test whether DEM intersection is geometrically meaningful and publish uncertainty.
- Validate canopy flight/mapping separately from thermal person detection. Use known ground targets, canopy-visibility labels, GNSS-denied cases, and independent safety procedures.
- Evaluate map alignment with checkpoints, then test road/bridge classes on held-out incident areas and local imagery. For bridges, output “visible damage/obstruction suspected,” not “safe/unsafe,” unless a qualified engineering assessment supports that conclusion.
- Measure end-to-end alert latency and useful throughput on mesh, gateway, and satellite paths. Keep image/audio bandwidth assumptions explicit.
- Maintain operator review records and a way to mark results uncertain or unusable.

## 8. Research gap observed

Among the public sources reviewed, the gap is not that no one has attempted drone audio, canopy autonomy, mapping, or rescue dashboards. The less commonly documented combination is one field-validated system that:

1. performs broad-area radiometric LWIR presence screening and records coverage quality;
2. runs calibrated airborne acoustic event detection/geolocation under its own rotor noise;
3. produces a georeferenced high-resolution RGB map and compares it to a dated reference for route changes;
4. coordinates search and relay UAV roles when the ground station is outside radio range; and
5. gives responders a common operating picture with evidence and human review.

This is a bounded finding from the sources reviewed, not a claim that no private, commercial, unpublished, or otherwise unindexed system has these features.

## References and project links

### Primary project pages and services

- [Fraunhofer FKIE LUCY](https://www.fkie.fraunhofer.de/en/departments/sdf/lucy.html)
- [SHERPA — CORDIS record](https://cordis.europa.eu/project/id/600958) · [results](https://cordis.europa.eu/project/id/600958/results)
- [NASA — Search and Rescue under Forest Canopy](https://ntrs.nasa.gov/citations/20200002819) · [MIT CSAIL project](https://www.csail.mit.edu/research/search-and-rescue-under-tree-canopy)
- [Copernicus EMS Rapid Mapping Manual](https://mapping.emergency.copernicus.eu/about/rapid-mapping-manual/)
- [AFTERMAP paper](https://www.mdpi.com/2075-5309/16/15/2943) · [repository](https://github.com/sultankennesaw-Civil/AFTERMAP-Dataset)
- [CURSOR — CORDIS final report](https://cordis.europa.eu/project/id/832790/reporting) · [official site](https://www.cursor-project.eu/)
- [Fraunhofer UAV-Rescue](https://www.fhr.fraunhofer.de/en/projects/03_industrial_applications/uav-rescue-uav-borne-sensor-systems-for-ai-based-support-of-rescue-missions.html)

### Technical papers, datasets, and code

- [Hoshiba et al. — UAV microphone-array localization](https://www.mdpi.com/1424-8220/17/11/2535)
- [DOANet — drone sound-source localization](https://link.springer.com/article/10.1186/s13636-020-00184-2)
- [Drone Audition — on-board microphone localization](https://openresearch-repository.anu.edu.au/items/89d987f1-2d0f-4df5-87ef-0d0fbc49f623)
- [Rotor-noise suppression for drone SAR audio](https://doi.org/10.1016/j.dsp.2024.104881)
- [Sky-Ear preprint](https://arxiv.org/abs/2604.12455)
- [DroneAudioSet paper](https://arxiv.org/abs/2510.15383) · [dataset](https://huggingface.co/datasets/ahlab-drone-project/DroneAudioSet)
- [RescueNet paper](https://arxiv.org/abs/2202.12361) · [repository](https://github.com/BinaLab/RescueNet-A-High-Resolution-Post-Disaster-UAV-Dataset-for-Semantic-Segmentation)
- [xBD paper](https://arxiv.org/abs/1911.09296) · [xView2 data](https://xview2.org/dataset) · [Microsoft model repository](https://github.com/microsoft/building-damage-assessment-cnn-siamese)
- [UAV road-damage dataset](https://zenodo.org/records/11473582)
