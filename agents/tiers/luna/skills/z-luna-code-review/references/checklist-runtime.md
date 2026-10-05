# Runtime APIs and lifecycle

Use for camera/input, physics, particles, networking, asynchronous code, events or pooling. Match the exact
overload and export configuration before reporting a runtime defect.

| Area | Inspect and verify |
| --- | --- |
| Physics and navigation | V7 hints 634–635 (`ProjectDiagnostics_LP_CharacterController_*`) explicitly warns that not all CharacterController features are implemented. Flag affected usage with that scope; verify behavior in the target export. For Rigidbody, raycasts, SphereCast, 2D movement and NavMesh, obtain exact API/version evidence rather than banning all physics or asserting support from a symbol. |
| Particles and pool reuse | Inspect `Clear(bool)`, `Stop` overloads, child systems and Runtime Analysis exclusions. Reproduce spawn → clear/stop → reuse. Do not assert `Clear$1` is always absent or prescribe Stop as a guaranteed substitute without testing equivalent behavior. |
| Camera/input conversion | For ScreenToWorldPoint, verify depth from camera, projection mode, canvas/iframe scaling and coordinate origin. Reproduce at target aspect ratios. Projecting every candidate to screen space changes picking semantics and cost; it is not a universal fix. |
| Async, tasks and loading | Vendor `config.json` force-includes Task and TaskCompletionSource, which establishes retention intent, not every overload's behavior. Verify async/await, UniTask and AsyncOperation separately against the installed compiler/runtime. Do not assume worker-thread execution or reject async solely by keyword. |
| Outbound HTTP | Check API implementation, CORS and the actual ad-network offline/self-contained policy. A transpiled UnityWebRequest call is not proof that the network request is permitted or available. |

## Lifetime checks

Choose subscriptions according to the intended lifetime: OnEnable/OnDisable for enabled lifetime;
Awake/OnDestroy for object lifetime. Do not move subscriptions wholesale because Luna allegedly loses them on
pause. Establish that mechanism with a pause/resume reproducer first; object-lifetime handlers can still fire
while a pooled object is disabled.

Inspect paired registration/release and repeated initialization. Test disable/enable, pool reuse, pause/resume
and destruction as applicable. General leaks belong to generic review unless a Luna-specific failure is
evidenced.

For an interface referring to a Unity object, distinguish destroyed Unity-object semantics from a managed
null. Check the actual target and expected lifetime; do not impose an `as Object` check on arbitrary non-Unity
implementations.

## Evidence boundary

V7 hints 913–916 (`RuntimeAnalysisTab_*_Hint`) documents removal of unused methods and modules and instructs
playthrough coverage. It does not prove a particular overload is removed in this project. Inspect project
exclusions and collect a focused exported-playable reproduction when these leads matter. Without one, report
the relevant check as unverified rather than a compatibility defect.
