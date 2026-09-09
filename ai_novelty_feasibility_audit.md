# AI Novelty and Feasibility Audit

## AI Evaluation Summary

The proposed project is feasible within a 15-week semester only if it is treated as a focused systems evaluation. Building ORB-SLAM3, evaluating it on KITTI, connecting stereo output from CARLA, and reporting localization failures are realistic at approximately 8-12 hours per week. A basic closed-loop waypoint demonstration is achievable but carries integration risk. Native LiDAR-camera fusion, dense neural reconstruction, semantic mapping, ROS2 integration, Jetson validation, multi-agent SLAM, and physical-vehicle testing cannot all be completed responsibly in the same semester.

The project has limited algorithmic novelty because ORB-SLAM3, KITTI evaluation, and CARLA-based autonomous-driving demonstrations are mature and widely used. Its defensible value is a reproducible evaluation framework that combines real-world recordings with controlled simulation, explicitly prevents ground-truth leakage, records failures as well as successes, measures consumer-hardware performance, and tests whether estimated pose can close the control loop.

## Novelty Assessment

| Dimension | Assessment | Explanation |
|---|---|---|
| New SLAM algorithm | Low | The project uses ORB-SLAM3 rather than proposing a new estimator. |
| New dataset | Low | KITTI and CARLA are established sources. |
| Experimental design | Moderate | A consistent real-data/simulation protocol with controlled stressors and ground-truth isolation provides useful systems evidence. |
| Integration contribution | Moderate | Connecting stereo SLAM output to evaluation and optional closed-loop control is meaningful engineering work. |
| Research novelty | 4/10 | Insufficient for a claim of a new state-of-the-art SLAM method. |
| Course-project value | 8/10 | Strong fit if the implementation is reproducible, quantitative, and candid about failures. |

## Red-Ocean Risks

1. **ORB-SLAM3 on KITTI is oversaturated.** Simply running existing examples and showing a trajectory would be a replication exercise, not a substantial project.
2. **CARLA autonomy demos are common.** A vehicle following waypoints is not novel if simulator ground truth performs the localization.
3. **Dense 3D visualization can hide weak evaluation.** Attractive maps do not demonstrate localization accuracy or robustness.
4. **Broad sensor-fusion language can overstate the implementation.** ORB-SLAM3 does not natively perform LiDAR-camera fusion. Accumulating LiDAR scans using visual poses is not equivalent to tightly coupled fusion.
5. **Simulation-only claims can be misleading.** CARLA cannot reproduce all physical sensor, actuator, thermal, vibration, calibration, and safety constraints.

## Differentiation Strategy

- Use ground truth strictly as an evaluation signal, never as a localization or control input.
- Evaluate both KITTI recordings and CARLA scenarios with the same metric definitions.
- Report tracking availability, relocalization events, runtime, and failed runs alongside ATE and RPE.
- Change one environmental factor at a time before testing combined stress conditions.
- Run repeated trials and report distributions or mean plus standard deviation instead of one best run.
- Use the SLAM pose in the waypoint controller to show the practical effect of localization error.
- Publish configurations and scripts needed to reproduce each result on the RTX 4070 SUPER system.

## Major Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| ORB-SLAM3 build and dependency conflicts | High | High | Validate official examples first; pin versions; preserve a known-good environment. |
| CARLA-to-SLAM timestamp or calibration errors | High | High | Use synchronous mode, fixed sensor transforms, logged timestamps, and a recorded short sequence before live integration. |
| ROS2 wrapper consumes the schedule | High | High | Keep ROS2 outside the critical path; use a direct file or process adapter first. |
| Tracking fails in CARLA images | Medium | High | Tune camera motion, exposure, resolution, field of view, and stereo baseline; retain offline evaluation as the minimum deliverable. |
| Controller becomes unstable because of pose noise | Medium | Medium | Begin at low speed on a simple route; filter only if documented; preserve localization metrics independent of control. |
| GPU memory contention between CARLA and learned models | Medium | Medium | Use ORB-SLAM3 as the baseline, reduce CARLA quality/resolution, and run learned comparisons offline. |
| Scope expansion into LiDAR or semantic mapping | High | High | Require all core milestones to pass by Week 10 before accepting any extension. |
| Cherry-picked results | Medium | High | Predeclare scenarios and metrics, retain failed runs, and report repeated trials. |

## Claims That Are Supported

- The software pipeline can evaluate stereo visual SLAM on recorded real-road data and simulated driving data.
- The project can quantify accuracy, runtime, tracking reliability, and environmental degradation on the available desktop.
- If completed, the closed-loop experiment can show that the estimated pose is sufficient for a limited simulated waypoint task.

## Claims That Are Not Supported

- The system is ready for deployment on a real vehicle.
- The RTX 4070 SUPER results predict Jetson performance.
- ORB-SLAM3 performs native LiDAR-camera fusion.
- CARLA performance proves safety or robustness on public roads.
- The project establishes a new state-of-the-art SLAM method.

## Scope Recommendation

Freeze the core scope at stereo ORB-SLAM3, KITTI, CARLA, trajectory evaluation, and controlled-condition testing. Treat the waypoint controller as the target demonstration and all LiDAR, ROS2, neural reconstruction, semantic mapping, and embedded deployment work as optional extensions. This provides enough technical depth while keeping the project finishable by a full-time student.

