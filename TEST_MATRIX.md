# Child-Safe Rendering Finality — Adversarial Test Matrix

This matrix is derived from `draft-das-child-safe-rendering-finality-03` and the implementation logic in `IMPLEMENTATION_REFERENCE.md`.

| ID | Scenario | Expected result |
|---|---|---|
| T01 | Valid adult candidate + current bounded context + matching sink | Permit only scoped effects |
| T02 | Candidate missing required field | Reject, non-renderable |
| T03 | Candidate contains unknown load-bearing field under strict schema | Reject |
| T04 | Invalid `act_type` | Reject |
| T05 | Invalid content digest syntax | Reject |
| T06 | Eligibility `NOT_ELIGIBLE` | Deny |
| T07 | Eligibility `UNKNOWN` | Deny / revalidate; no effect |
| T08 | Eligibility `EXPIRED` | Deny / revalidate; no effect |
| T09 | Eligibility `REVOKED` | Deny |
| T10 | Parental policy `DENY` | Deny |
| T11 | Platform policy `DENY` | Deny |
| T12 | Stale policy epoch | Deny / revalidate |
| T13 | Stale revocation epoch | Deny / revalidate |
| T14 | Replayed nonce | Deny |
| T15 | Content digest changed after authority | Deny |
| T16 | Recipient changed after authority | Deny |
| T17 | Device changed after authority | Deny |
| T18 | Application changed after authority | Deny |
| T19 | Sink changed after authority | Deny |
| T20 | Display authority presented to cast sink | Deny |
| T21 | Display authority presented to mirror sink | Deny |
| T22 | Authority expired | Deny |
| T23 | Authority authenticator invalid | Deny |
| T24 | Authority absent | Deny |
| T25 | Authority malformed | Deny |
| T26 | `SINGLE_USE` first valid presentation | Permit once |
| T27 | `SINGLE_USE` second presentation | Deny |
| T28 | `FRAME_WINDOW` within frame bound | Permit if all other state current |
| T29 | `FRAME_WINDOW` exceeds `max_frame_count` | Stop / deny |
| T30 | `FRAME_WINDOW` exceeds `max_duration_ms` | Stop / deny |
| T31 | `SESSION_BOUND` reused in different session | Deny |
| T32 | Minor-registered device + adult-only content | Hard deny |
| T33 | Minor device + adult account logged in | Still deny |
| T34 | Minor device + valid OVER_18 service credential | Still deny |
| T35 | Adult-registered device without adult verification | No authority |
| T36 | Adult verification creates bounded viewing context | Eligible to proceed to PED |
| T37 | Temporary Under-18 mode activates during adult context | Invalidate/suspend/supersede adult authority |
| T38 | Adult account token persists during Temporary Under-18 mode | Does not restore adult authority |
| T39 | Temporary Under-18 timer expires | Does not silently restore adult rendering |
| T40 | Protected adult re-authentication after handover succeeds | New bounded context may be created |
| T41 | Protected adult re-authentication fails | Remain non-renderable |
| T42 | Device lock invalidates configured context | Old authority unusable |
| T43 | Profile change invalidates configured context | Old authority unusable |
| T44 | Application materially backgrounds / terminates | Revalidate where profile requires |
| T45 | Output changes from local display to cast | New/specific authority required |
| T46 | Output changes from local display to mirror | New/specific authority required |
| T47 | Output changes to XR | Validate XR output and sink |
| T48 | Device-attestation epoch changes | Revalidate / deny stale authority |
| T49 | Policy changes while content buffered | Old authority cannot override new policy |
| T50 | Revocation changes while content buffered | Old authority cannot override revocation |
| T51 | Cached restricted content on device without current authority | Remains non-renderable |
| T52 | Locally AI-generated restricted content | Requires applicable classification/authority |
| T53 | AI transformation changes final digest | Old content-bound authority fails |
| T54 | Policy explicitly permits low-risk transformation inheritance | Permit only if profile rules and bindings remain satisfied |
| T55 | High-risk generative transformation | Require new policy evaluation / authority |
| T56 | Key-release failure | No usable key enters unprotected app context |
| T57 | Decoder bypass attempt | No visible output through protected claim path |
| T58 | Unprotected GPU/compositor surface bypass attempt | Must fail for a hardware/TEE finality claim |
| T59 | Audio requested without `PLAY_AUDIO` | No audio effect |
| T60 | External display attached when policy requires separate authority | Deny / revalidate |
| T61 | Screen capture active when policy forbids it | Deny / suspend |
| T62 | VPN/proxy changes upstream path | Does not create finality authority |
| T63 | Alternate DNS path | Does not create finality authority |
| T64 | Authorization service offline but valid bounded lease exists | Continue only if policy permits and lease remains valid |
| T65 | Authorization service offline and high-risk authority unavailable | Fail closed |
| T66 | Offline lease expires | Stop and require renewal |
| T67 | Restored device snapshot contains old authority | Replay/epoch protections reject |
| T68 | Old parental policy snapshot restored | Current epoch prevents override |
| T69 | Wrong sink runtime ID | Deny |
| T70 | Device attestation `INVALID` where required | Deny |
| T71 | Device attestation `UNKNOWN` in high-assurance profile | No silent rendering |
| T72 | All validations pass but validation evidence not committed | Do not issue authority |
| T73 | Boolean ALLOW supplied without finality authority | No protected effect |
| T74 | Authority has valid signature but wrong effect scope | Deny requested effect |
| T75 | Authority is fresh but revoked | Deny |
| T76 | Immediate physical handoff with no detectable event | Documented limitation; bounded lease remains the residual risk |
| T77 | High-risk revalidation event fires while playing | Pause/blank/mute until valid renewed authority |
| T78 | Physical external camera records display | Outside digital finality threat claim |
