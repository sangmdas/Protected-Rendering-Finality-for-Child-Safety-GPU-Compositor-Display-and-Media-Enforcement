# Child-Safe Rendering Execution Finality

## Hardware-Rooted Enforcement for Age-Restricted Content on Android, iOS, and Other Platforms

### Vendor-neutral implementation reference for protected child-safety rendering across mobile, desktop, XR, media, and AI-enabled devices.

> **Core principle:** Permission to deliver content is not permission to render it.

This repository translates **`draft-das-child-safe-rendering-finality-03`** into an implementation-oriented engineering package for platform, operating-system, GPU, compositor, media-pipeline, confidential-computing, browser, AI, and child-safety engineers.

The architecture addresses a specific technical gap: an upstream age check, parental-control rule, account state, content label, server-side policy decision, or application-level `DENY` can be correct while restricted content still becomes perceptible later through a different software, media, GPU, display, audio, cast, mirror, XR, cached, local, or AI-generated path.

The repository therefore models **rendering itself as a protected consequence**.

A restricted object may be delivered, downloaded, cached, buffered, generated, transformed, decrypted inside a protected path, or decoded into protected memory while still remaining technically **NON-RENDERABLE** until current, scoped Rendering Finality Authority is independently verified at the protected effect boundary.

---

## Repository Status

This repository is currently a **comprehensive implementation reference and engineering blueprint**.

It includes:

- exact interoperability schemas;
- exact source examples;
- state machines;
- authority and sink logic;
- implementation pseudocode;
- device handover logic;
- Android/iOS-oriented platform integration guidance;
- GPU/compositor and media-path integration models;
- replay, revocation, freshness, and epoch logic;
- three authority-consumption modes;
- AI-generated and AI-transformed content handling;
- privacy and anti-surveillance requirements;
- 62 extracted normative-requirement paragraphs;
- 78 adversarial and implementation test scenarios.

It is **not yet**:

- a production-certified Android framework implementation;
- an iOS system-framework implementation;
- a commercial GPU driver;
- a production DRM stack;
- an Apple, Google, NVIDIA, AMD, Qualcomm, Intel, ARM, or Microsoft integration;
- a measured production-device benchmark;
- a certified payment, identity, or age-assurance product.

The goal of this repository is to make the architecture **inspectable, implementable, testable, and difficult to misinterpret** before platform-specific deployment work begins.

---

# 1. Problem Statement

Modern child-safety controls often operate before the final rendering boundary.

Examples include:

- account-age flags;
- parental settings;
- content classifications;
- recommender controls;
- platform policy;
- server-side moderation;
- age-assurance services;
- application filters;
- browser restrictions;
- app-store policies;
- profile settings.

Those controls matter, but they do not automatically prove that restricted content cannot later become perceptible through:

- another browser;
- an embedded WebView;
- a messaging client;
- a local file;
- a cache;
- a sideloaded application;
- a third-party SDK;
- a media decoder;
- a GPU;
- a compositor;
- a protected or unprotected surface;
- an audio route;
- an external display;
- casting;
- mirroring;
- XR;
- remote display;
- local AI generation;
- AI transformation.

The architecture therefore distinguishes:

```text
CONTENT DELIVERY
        !=
CONTENT RENDERING AUTHORITY

AGE CHECK
        !=
DISPLAY FINALITY

CONTENT CLASSIFICATION
        !=
DECRYPTION AUTHORITY

SERVER-SIDE ALLOW
        !=
DEVICE-SIDE MATERIALIZATION AUTHORITY
```

The relevant security question is:

> **Is this specific protected content authorized to become perceptible to this recipient, on this device, through this output path, under the current policy, eligibility, freshness, revocation, and protected-state conditions?**

---

# 2. Core Security Property

The protected effect is **perceptibility**.

The implementation model is:

```text
Restricted Content
        |
        v
Restricted Content Candidate Act
        |
        v
NON-RENDERABLE STATE
        |
        v
Protected Enforcement Domain (PED)
        |
        +-- recipient / eligibility
        +-- content / classification
        +-- device / application
        +-- policy / jurisdiction
        +-- policy epoch
        +-- revocation epoch
        +-- freshness / nonce
        +-- device or sink attestation
        +-- output target
        +-- intended Finality Sink
        |
        v
Protected Validation Evidence
        |
        v
Scoped Non-Bearer Rendering Finality Authority
        |
        v
Protected Rendering Finality Sink
        |
      +---+---+
      |       |
    VALID   INVALID / ABSENT / STALE
      |       |
      v       v
   RENDER   KEEP NON-RENDERABLE
```

The central invariant is:

> **No valid Rendering Finality Authority means no protected rendering effect.**

---

# 3. Why Android and iOS Are Important Targets

The architecture is designed to be platform-neutral, but Android and iOS are particularly important deployment targets because modern mobile content can pass through multiple independent application and system paths before reaching the display, speaker, external display, cast target, or other perceptible output.

## Android-oriented implementation targets

A future Android implementation could potentially integrate with appropriate protected boundaries such as:

- Android framework services;
- system server;
- MediaCodec / protected media pipeline;
- DRM-backed key release;
- hardware-backed keystore;
- TEE-backed services;
- SurfaceFlinger or protected compositor logic;
- protected buffers / protected surfaces;
- display HAL / composer boundary;
- audio service / protected audio output;
- screen-capture restrictions;
- casting / remote display stack;
- hardware attestation;
- device policy / family controls.

The protocol does **not** require all of these components.

The correct integration point is the point at which the protected effect can be made **non-bypassable for the security claim being made**.

## iOS-oriented implementation targets

A future iOS implementation could potentially integrate with appropriate platform-controlled boundaries such as:

- protected OS services;
- system media frameworks;
- protected video and DRM paths;
- Secure Enclave-backed authorization state;
- protected key-release services;
- Metal / GPU submission boundaries where appropriate;
- Core Animation / compositor-related enforcement;
- protected display surfaces;
- audio-output enforcement;
- AirPlay / screen-mirroring policy;
- external display policy;
- protected device-state transitions;
- local user-presence verification.

Again, the architecture does **not** claim that these integrations already exist in this repository.

The architecture defines the **security semantics and binding model** that a platform implementation would need to enforce.

---

# 4. Platform-Neutral Target Systems

Potential target systems include:

- Android smartphones and tablets;
- iPhone and iPad;
- laptops;
- desktop systems;
- smart TVs;
- streaming devices;
- game consoles;
- cloud-gaming clients;
- XR headsets;
- AI-enabled consumer devices;
- protected enterprise endpoints.

Potential protected components include:

- CPU TEEs;
- secure enclaves;
- secure elements;
- trusted OS services;
- protected media engines;
- GPUs;
- GPU command-submission boundaries;
- media decoders;
- video-decryption engines;
- protected memory;
- protected surfaces;
- display compositors;
- display controllers;
- audio paths;
- HSMs;
- DPUs;
- SmartNICs;
- XR compositors;
- cast controllers;
- mirror controllers.

---

# 5. Repository Contents

```text
.
├── IMPLEMENTATION_REFERENCE.md
├── NORMATIVE_REQUIREMENTS.md
├── TEST_MATRIX.md
├── schemas/
│   ├── restricted-content-candidate.schema.json
│   ├── rendering-policy-decision.schema.json
│   ├── rendering-finality-authority.schema.json
│   └── render-sink-verify-request.schema.json
├── examples/
│   ├── allow-transaction.example.json
│   ├── deny-transaction.example.json
│   ├── minor-registered-denial.example.json
│   └── handoff-revalidation.example.json
└── draft-das-child-safe-rendering-finality-03 (2).xml
```

## `IMPLEMENTATION_REFERENCE.md`

The main engineering reference.

It covers:

- Candidate Act construction;
- Non-Renderable State;
- PED evaluation;
- protected validation evidence;
- finality-authority issuance;
- sink verification;
- device state;
- family handover state;
- replay;
- revocation;
- policy epochs;
- freshness;
- output scope;
- AI transformation;
- offline behavior;
- assurance profiles;
- deployment choices;
- implementation limitations.

## `NORMATIVE_REQUIREMENTS.md`

An implementation checklist extracted from the source XML.

It contains **62 source paragraphs** containing `MUST`, `SHOULD`, or `MAY`.

The Internet-Draft remains the source specification if any ambiguity exists.

## `TEST_MATRIX.md`

A security-oriented matrix containing **78 adversarial and implementation scenarios**.

These define expected behavior.

They do **not** mean all 78 scenarios have already been executed against production Android, iOS, GPU, TEE, or commercial hardware.

---

# 6. Interoperability Objects

The reference profile uses **JSON Schema Draft 2020-12**.

Four principal objects are defined.

## 6.1 RestrictedContentCandidate

Represents a proposed restricted-content materialization.

Top-level required fields include:

```text
version
candidate_act_id
act_type
content
recipient_context
policy_context
render_request
freshness
finality_sink
```

### Act types

```text
RENDER_VIDEO
RENDER_IMAGE
PLAY_AUDIO
RENDER_INTERACTIVE
RENDER_GENERATED_MEDIA
CAST_PROTECTED_CONTENT
MIRROR_PROTECTED_CONTENT
```

### Content classes

```text
ADULT_SEXUAL_CONTENT
SEXUALLY_EXPLICIT_CONTENT
AGE_RESTRICTED_VIDEO
AGE_RESTRICTED_AUDIO
AGE_RESTRICTED_INTERACTIVE
AGE_RESTRICTED_GENERATED_MEDIA
OTHER_RESTRICTED
```

### Protection states

```text
ENCRYPTED
KEY_WITHHELD
PROTECTED_SURFACE_ONLY
NON_RENDERABLE_BUFFER
OTHER_PROTECTED
```

### Recipient eligibility

```text
ELIGIBLE
NOT_ELIGIBLE
UNKNOWN
EXPIRED
REVOKED
```

### Render modes

```text
LOCAL_DISPLAY
LOCAL_AUDIO
XR_DISPLAY
CAST
MIRROR
REMOTE_DISPLAY
```

### Output targets

```text
DISPLAY
SPEAKER
HEADSET
XR_COMPOSITOR
CAST_SINK
MIRROR_SINK
```

### Sink types

```text
CONTENT_KEY_RELEASE
DECRYPTION_GATE
MEDIA_DECODER_GATE
GPU_COMPOSITOR_GATE
PROTECTED_SURFACE_GATE
DISPLAY_ENABLE_GATE
AUDIO_OUTPUT_GATE
CAST_GATE
MIRROR_GATE
```

---

# 7. RenderingPolicyDecision

The PED produces a structured policy result.

Defined decisions include:

```text
ALLOW_RENDER
DENY_RENDER
ALLOW_RESTRICTED_MODE
REQUIRE_REVALIDATION
```

Defined protected effects include:

```text
RELEASE_CONTENT_KEY
DECRYPT
DECODE
CREATE_PROTECTED_SURFACE
COMPOSITE
DISPLAY
PLAY_AUDIO
CAST
MIRROR
```

Defined denial reasons include:

```text
RECIPIENT_NOT_ELIGIBLE
ELIGIBILITY_UNKNOWN
ELIGIBILITY_EXPIRED
PARENTAL_POLICY_DENY
PLATFORM_POLICY_DENY
CONTENT_CLASS_MISMATCH
POLICY_EPOCH_STALE
REVOCATION_EPOCH_STALE
NONCE_REPLAY
DEVICE_MISMATCH
APPLICATION_MISMATCH
SINK_MISMATCH
OUTPUT_TARGET_NOT_PERMITTED
ATTESTATION_FAILURE
AUTHORITY_EXPIRED
```

A policy `ALLOW` is **not itself finality authority**.

Successful PED evaluation permits creation of scoped authority; it does not itself make content perceptible.

---

# 8. RenderingFinalityAuthority

Rendering Finality Authority represents the cryptographically protected authorization that connects PED validation to the protected rendering boundary.

Important bindings include:

- Candidate Act;
- Candidate digest;
- content digest;
- recipient context;
- device;
- application;
- policy epoch;
- revocation epoch;
- freshness;
- nonce;
- permitted effects;
- Finality Sink;
- expiration;
- consumption profile.

Architecturally, authority is intended to be:

- act-bound;
- content-bound;
- recipient-bound;
- device-bound;
- application-bound where applicable;
- policy-epoch-bound;
- revocation-epoch-bound;
- freshness-bound;
- effect-bound;
- sink-bound.

Possession alone must not be sufficient.

The Finality Sink independently verifies the protected bindings.

---

# 9. Authority Consumption Modes

The profile defines three important authority modes.

## 9.1 `SINGLE_USE`

One protected effect.

```text
Issue
  |
  v
Verify
  |
  v
Consume Atomically
  |
  v
Protected Effect
```

A second use is rejected.

Suitable for one-shot, high-risk, or non-repeatable consequences.

---

## 9.2 `FRAME_WINDOW`

A bounded rendering window.

Possible bounds include:

```text
max_frame_count
max_duration_ms
```

The intention is to support high-frame-rate rendering without requiring a complete authorization transaction for every frame.

Example:

```text
Adult Verification
        |
        v
Policy Validation
        |
        v
Finality Lease
        |
        v
Protected Session
        |
        v
Render Many Frames
        |
        v
Expiry / Security Change
        |
        v
Revalidate
```

---

## 9.3 `SESSION_BOUND`

Authority is usable only within a protected session.

The session may be invalidated by:

- lock/unlock transition;
- protected profile change;
- application termination;
- material backgrounding;
- inactivity;
- session expiry;
- content-class escalation;
- policy change;
- revocation;
- output-path change;
- sink change;
- Temporary Under-18 activation;
- security epoch change;
- attestation-state change.

---

# 10. Protected Enforcement Domain

The PED evaluates the current rendering attempt.

A reference evaluation sequence is:

```text
1. Strictly validate Candidate schema.
2. Check Candidate freshness and nonce.
3. Check protected device age-registration baseline.
4. Check Temporary Under-18 state.
5. Evaluate recipient eligibility.
6. Evaluate parental policy.
7. Evaluate platform policy.
8. Verify policy epoch.
9. Verify revocation epoch.
10. Verify device binding.
11. Verify application binding.
12. Verify content digest / classification.
13. Verify output scope.
14. Verify intended Finality Sink.
15. Verify attestation where required.
16. Verify current Adult Viewing Context where required.
17. Commit Protected Validation Evidence.
18. Issue narrowly scoped Rendering Finality Authority.
```

No single upstream input should be treated as sufficient authorization.

---

# 11. Protected Validation Evidence

Before, or atomically with, authority issuance, the implementation records protected validation evidence.

Possible implementation mechanisms include:

- protected signed record;
- MAC;
- enclave assertion;
- HSM assertion;
- sealed state;
- monotonic-state commitment;
- append-only protected record;
- equivalent protected integrity mechanism.

A practical evidence record may bind:

```text
evidence_id
candidate_digest
decision_id
validated_policy_epoch
validated_revocation_epoch
eligibility_result_reference
device_binding
application_binding
sink_binding
permitted_effect_scope
timestamp
authority_id
integrity/authenticator data
```

This structure is implementation-oriented and does not create a new wire protocol.

---

# 12. Candidate Digest

The authority schema includes a SHA-256 Candidate digest.

A production interoperable implementation needs a deterministic representation.

Conceptually:

```text
canonical_candidate_bytes(candidate, profile_version)
        |
        v
SHA-256
        |
        v
candidate_digest
```

The repository intentionally does **not** pretend that arbitrary JSON serialization is safe for interoperability.

A future executable profile must explicitly define:

- canonicalization;
- field ordering;
- normalization;
- byte encoding;
- digest versioning.

---

# 13. Cryptographic Implementation Boundary

The source architecture requires a signature, MAC, or equivalent protected authenticator.

The current reference package intentionally leaves the following open:

- signature algorithm;
- MAC algorithm;
- COSE vs JWS vs platform-native protected object;
- key identifiers;
- trust anchors;
- key distribution;
- key rotation;
- hardware-backed custody;
- proof-of-possession mechanism;
- certificate representation.

This allows a platform to use appropriate protected cryptography without forcing a premature universal choice.

A future runnable interoperability profile should select and version these choices explicitly.

---

# 14. Finality Sink Verification

The Finality Sink independently verifies the actual rendering attempt immediately before effectuation.

Reference verification logic:

```text
Verify authority exists.
Verify authority format.
Verify authenticator.
Verify candidate_act_id.
Recompute and verify candidate_digest.
Verify content digest.
Verify recipient binding.
Verify device binding.
Verify application binding.
Verify sink ID.
Verify sink type.
Verify policy epoch.
Verify revocation epoch.
Verify issued/expiry time.
Verify device/sink attestation where required.
Verify output path.
Verify permitted effects.
Verify replay / consumption state.
Verify current runtime conditions.
```

Then:

```text
IF any required condition fails:
    KEEP NON-RENDERABLE

ELSE:
    enable ONLY the explicitly authorized protected effect
```

There is no warning-and-render fallback.

---

# 15. Finality Sink Variations

A production deployment may use one Finality Sink or a protected chain of sinks.

## Content key release

```text
Encrypted Content
       |
       v
Key Withheld
       |
    Valid Authority?
      /       \
    NO         YES
    |           |
Withhold     Release only into
            protected path
```

## Decryption gate

Prevents usable plaintext media from reaching an unprotected application path without valid authority.

## Media decoder gate

Prevents decoding into a usable output path unless the current effect is authorized.

## GPU / compositor gate

Potentially controls:

- GPU command submission;
- protected-surface admission;
- compositor admission;
- protected display composition.

## Protected surface gate

Restricts creation or use of protected rendering surfaces.

## Display enable gate

Controls the transition from protected graphics state to visible pixels.

## Audio output gate

Controls protected restricted audio independently from display authority.

## Cast gate

Requires explicit cast authorization.

## Mirror gate

Requires explicit mirror authorization.

---

# 16. Output Scope Is Load-Bearing

A local-display authorization must not silently become universal output authority.

```text
DISPLAY AUTHORITY
     |
     +--> DISPLAY  : potentially allowed
     |
     +--> CAST     : not implied
     |
     +--> MIRROR   : not implied
     |
     +--> XR       : not implied
```

The same principle applies across:

- speaker;
- headset;
- external display;
- cast;
- mirror;
- XR compositor;
- remote display.

This is important because blocking local display while leaving mirroring or casting unconstrained does not provide a complete protected rendering path.

---

# 17. Device Age Registration

The architecture separates device-registration state from current rendering authority.

## Minor-Registered Device

Under the strict child-safety profile:

```text
MINOR-REGISTERED DEVICE
        |
        v
Adult-only Rendering Finality Authority prohibited
        |
        v
Adult content remains NON-RENDERABLE
```

An adult account login does not override the protected minor-device baseline.

A stored `OVER_18` credential does not override it.

A previously valid adult session does not override it.

Downloaded or cached adult content does not override it.

If policy permits the device to change age-registration class, that transition requires an explicit protected reprovisioning or age-registration process.

## Adult-Registered Device

Adult registration means only:

> the device may attempt to establish adult rendering authorization.

It does **not** mean:

> adult content may always render.

A protected Adult Viewing Context may still be required.

---

# 18. Protected Adult Viewing Context

An Adult Viewing Context can be established using a protected local mechanism such as:

- PIN;
- passkey;
- local biometric confirmation;
- trusted wearable confirmation;
- another protected policy-approved mechanism.

The protocol does not require one universal biometric mechanism.

A bounded Adult Viewing Context can avoid repeated authentication for every frame or video.

Possible bindings include:

```text
device
profile
application
content class
session
policy epoch
revocation epoch
sink
freshness
expiry
revalidation conditions
```

The content provider need not receive:

- civil identity;
- exact date of birth;
- biometric template.

---

# 19. Temporary Under-18 Handover Mode

This is one of the most important state transitions in the architecture.

Before an adult hands an adult-configured device to a child:

```text
PROTECTED ADULT VIEWING CONTEXT
        |
        v
TEMPORARY UNDER-18 MODE ACTIVATED
        |
        +-- invalidate / suspend / consume /
        |   supersede incompatible adult authority
        |
        +-- advance protected state or epoch
        |
        +-- prohibit new adult-only authority
        |
        v
TEMPORARY MINOR BASELINE
```

While Temporary Under-18 Mode is active, these must not independently restore adult rendering authority:

- adult account login;
- `OVER_18` credential;
- browser cookie;
- application access token;
- cached adult session;
- downloaded restricted media;
- earlier adult verification;
- stale application state.

## Timer behavior

A timer expiring must not silently return the device to active adult rendering.

Safer behavior:

```text
Temporary Under-18 Mode
        |
        | timer expires
        v
Unverified Adult State
        |
        v
Adult content remains non-renderable
        |
        v
Protected Adult Re-Authorization
```

Design principle:

> **Entering the safer state can be easy; leaving the safer state is protected.**

---

# 20. Immediate Physical Handoff Limitation

The architecture explicitly recognizes a difficult real-world limitation.

If:

1. an adult successfully authenticates;
2. restricted content begins playing;
3. the still-valid device is immediately handed to a child; and
4. no protected event occurs that causes revalidation,

the device cannot reliably infer the physical viewer change.

The architecture therefore does **not** claim perfect continuous viewer identification.

Risk can be reduced by:

- shorter leases;
- inactivity;
- screen off/on;
- app transitions;
- new restricted title;
- content-class escalation;
- output change;
- protected continuity signals;
- Temporary Under-18 Mode.

Higher-risk policy may:

```text
PAUSE / BLANK / MUTE
        |
        v
REVALIDATE
        |
     +--+--+
     |     |
   PASS   FAIL
     |     |
   RESUME KEEP NON-PERCEPTIBLE
```

Continuous camera surveillance or continuous facial recognition is not required.

---

# 21. AI-Generated and AI-Transformed Content

The architecture explicitly addresses content with no stable catalogue identifier.

Examples:

- generative images;
- AI-generated video;
- generated audio;
- multimodal output;
- transformed images;
- local model output;
- generated game assets;
- generated XR scenes.

The rule is:

```text
AI GENERATION
      !=
RENDERING AUTHORITY
```

A transformed representation may require:

- a new content digest;
- a new classification;
- policy re-evaluation;
- new authority.

Low-risk transformations may inherit prior authorization only where policy explicitly permits it and all required bindings remain valid.

High-risk generative transformation can require a new authorization decision.

---

# 22. Replay and Revocation

Finality Authority must not become an unrestricted reusable bearer credential.

Potential replay protections include:

- unique nonce;
- expiration;
- session binding;
- device-key binding;
- sink binding;
- policy epoch;
- revocation epoch;
- atomic consumption;
- current status checks.

The sink should check current policy and revocation state immediately before effectuation.

Old authority must not override later:

- parental-policy change;
- account-state change;
- policy change;
- device revocation;
- eligibility revocation.

A production implementation must also address rollback and snapshot restoration.

---

# 23. Offline Operation

The architecture does not require a live network transaction for every frame.

A bounded Finality Lease may support controlled offline operation:

```text
Online Authorization
        |
        v
Bounded Finality Lease
        |
        v
Offline Protected Session
        |
        v
Lease Expiry
        |
        v
Renew / Revalidate
```

High-risk content may fail closed when current authorization cannot be established.

Lower-risk already-authorized operation may continue only within a still-valid bounded lease where policy permits.

Lease duration is a policy choice, not a hard-coded protocol constant.

---

# 24. Privacy Model

The architecture is designed to minimize disclosure.

Preferred separation:

```text
Age / Eligibility Assurance
        |
        v
Minimum Required Attribute
        |
        v
Policy / PED
        |
        v
Scoped Finality Authority
        |
        v
Protected Rendering Sink
```

The Finality Sink generally does not need:

- exact birth date;
- full identity;
- face image;
- biometric template;
- full browsing history.

Logging should prefer:

- opaque identifiers;
- digests;
- protected commitments;
- minimal eligibility attributes.

---

# 25. Assurance Profiles

The architecture supports progressive deployment.

## Software Finality

Potential location:

```text
Browser / App / OS service
```

Useful for experimentation and interoperability.

Security is limited if ordinary software or the OS can bypass the enforcement point.

## OS / TEE Finality

Potential location:

```text
Application
    |
    v
Protected OS Service / TEE
    |
    v
Protected Media Path
```

Provides stronger protection against compromised applications.

## Hardware Rendering Finality

Potential location:

```text
Protected OS / TEE
        |
        v
Protected Media Path
        |
        v
GPU / Decoder / Compositor / Display / Audio / XR
```

Provides stronger guarantees when the hardware and effect topology make the path non-bypassable.

No profile claims protection against an attacker with unrestricted control of the underlying physical hardware.

---

# 26. Staged Deployment Model

A realistic adoption path is:

### Stage 1 — Software enforcement

Browsers, applications, or OS services implement the protocol semantics.

### Stage 2 — Trusted execution

Authorization moves into protected OS or TEE components.

### Stage 3 — Protected media paths

Restricted content cannot bypass the protected execution path.

### Stage 4 — Hardware finality

The final gate moves closer to:

- GPU;
- compositor;
- decoder;
- display;
- audio;
- XR;
- secure video pipeline.

### Stage 5 — Ecosystem interoperability

Content providers, AI providers, browser vendors, app stores, operating systems, and device platforms use a common finality contract.

---

# 27. Android Reference Mapping

The following is a **conceptual implementation mapping**, not a claim of current integration.

| Execution-Finality concept | Possible Android-oriented implementation area |
|---|---|
| Protected device state | Device policy / protected system service / TEE-backed state |
| Adult Viewing Context | Protected local user verification + bounded system state |
| Temporary Under-18 Mode | Protected system/family-control state transition |
| Candidate Act | Framework or protected media request object |
| PED | Trusted system service / TEE / policy service |
| Validation evidence | Hardware-backed or protected signed/sealed state |
| Authority | Protected signed/MACed object or platform-native protected capability |
| Key-release sink | DRM / protected key service |
| Decoder sink | MediaCodec / protected decoder path |
| GPU/compositor sink | protected Surface / compositor / display composition boundary |
| Audio sink | protected audio routing control |
| Cast/mirror sink | system cast / screen-sharing control |
| Attestation | Android hardware/device attestation where appropriate |

---

# 28. iOS Reference Mapping

The following is also **conceptual**, not current Apple integration.

| Execution-Finality concept | Possible iOS-oriented implementation area |
|---|---|
| Protected device state | protected OS policy state |
| Adult Viewing Context | protected local user verification |
| Temporary Under-18 Mode | protected family/device-state transition |
| Candidate Act | system media/render request representation |
| PED | protected OS authorization service |
| Validation evidence | Secure Enclave / protected signed state where appropriate |
| Authority | protected system capability / signed authorization object |
| Key-release sink | protected DRM/media key-release boundary |
| Decoder sink | protected media decode pipeline |
| GPU/compositor sink | Metal/Core Animation/protected compositor boundary where feasible |
| Display sink | protected display-plane enablement |
| Audio sink | protected audio output |
| Cast/mirror sink | AirPlay / mirroring enforcement |
| Hardware trust | Secure Enclave / platform security primitives where appropriate |

---

# 29. Threat Model

The implementation reference considers threats including:

- stale age state;
- forged age state;
- revoked parental policy;
- adult-authorized shared device used by a minor;
- physical device handoff;
- stale adult authority surviving handoff;
- cache replay;
- app-path bypass;
- direct decoder access;
- compromised app;
- malicious SDK;
- stale key;
- sink substitution;
- content substitution;
- metadata substitution;
- AI-generation bypass;
- cast bypass;
- mirror bypass;
- screen capture;
- external display;
- key extraction;
- revocation-state rollback;
- stale attestation.

Physical capture with an external camera is outside the digital Finality Sink security claim.

---

# 30. Fail-Closed Matrix

| Condition | Protected result |
|---|---|
| Missing authority | NON-RENDERABLE |
| Malformed authority | NON-RENDERABLE |
| Invalid authenticator | NON-RENDERABLE |
| Expired authority | NON-RENDERABLE |
| Stale policy epoch | NON-RENDERABLE / revalidate |
| Stale revocation epoch | NON-RENDERABLE / revalidate |
| Replay | NON-RENDERABLE |
| Consumed single-use authority | NON-RENDERABLE |
| Content mismatch | NON-RENDERABLE |
| Recipient mismatch | NON-RENDERABLE |
| Device mismatch | NON-RENDERABLE |
| Application mismatch | NON-RENDERABLE |
| Sink mismatch | NON-RENDERABLE |
| Output mismatch | NON-RENDERABLE |
| Parental policy DENY | NON-RENDERABLE |
| Platform policy DENY | NON-RENDERABLE |
| Eligibility NOT_ELIGIBLE | NON-RENDERABLE |
| Eligibility EXPIRED | NON-RENDERABLE / revalidate |
| Eligibility REVOKED | NON-RENDERABLE |
| Temporary Under-18 active | No new adult-only authority |
| Minor-registered device | No adult-only authority under strict profile |
| Handover timer expires | Do not auto-enable adult content |
| DISPLAY authority used for CAST | Reject cast effect |
| DISPLAY authority used for MIRROR | Reject mirror effect |
| AI output changes materially | New applicable validation/authority |
| High-risk authorization unavailable | Fail closed |

---

# 31. Test Coverage

`TEST_MATRIX.md` defines **78 scenarios**.

Coverage includes:

## Schema tests

- required fields;
- unknown load-bearing fields;
- enum validation;
- digest format;
- time format.

## Binding tests

- content substitution;
- recipient substitution;
- device substitution;
- application substitution;
- sink substitution;
- output substitution;
- epoch substitution;
- nonce substitution;
- session substitution.

## Authority tests

- missing authority;
- invalid authenticator;
- expiry;
- effect mismatch;
- candidate mismatch;
- wrong sink.

## Consumption tests

- single use;
- replay;
- frame count;
- frame duration;
- session transfer.

## Device-state tests

- minor hard deny;
- adult login on minor device;
- adult registration without current verification;
- bounded adult context;
- stale adult context;
- Temporary Under-18 activation;
- timer expiry;
- protected re-authentication.

## Output-path tests

- display;
- speaker;
- headset;
- XR;
- cast;
- mirror;
- remote display;
- screen capture;
- external display.

## AI tests

- generated restricted content;
- transformed content;
- digest mismatch;
- low-risk inheritance where policy allows;
- high-risk regeneration requiring new authority.

## Offline tests

- bounded valid lease;
- lease expiry;
- high-risk service outage.

---

# 32. Languages and Formats

The current package uses:

## Markdown

For:

- implementation documentation;
- state machines;
- pseudocode;
- test matrices;
- engineering guidance.

## JSON

For:

- example Candidate Acts;
- policy decisions;
- finality authority;
- sink-verification requests.

## JSON Schema Draft 2020-12

For strict validation of interoperability objects.

## XML / RFCXML

The original Internet-Draft source is preserved as RFCXML.

---

# 33. Package-Generation Tooling

The implementation-reference package was generated using:

```text
Python: 3.13.5
lxml: 6.1.1
Architecture: x86_64
Operating system: Linux
Kernel: 6.18.35
```

Python standard-library tooling was used for:

- XML parsing;
- JSON extraction;
- file generation;
- directory generation;
- ZIP generation;
- TAR.GZ generation.

The JSON schemas and examples were programmatically extracted from the source RFCXML rather than manually retyped.

---

# 34. Quantitative Package Results

The current package contains:

```text
4 exact JSON interoperability schemas
4 exact JSON examples
62 normative-requirement paragraphs
78 adversarial / implementation test scenarios
```

These are **repository/package counts**.

They are not runtime performance measurements.

---

# 35. Performance Status

## No production performance claims are currently made

The repository does not currently contain measured production values for:

- cryptographic latency;
- GPU latency;
- compositor overhead;
- FPS impact;
- frame-time overhead;
- CPU utilization;
- GPU utilization;
- memory bandwidth;
- battery consumption;
- power consumption;
- video-decoding overhead;
- XR latency;
- cast latency;
- Android device performance;
- iPhone/iPad performance;
- Apple Silicon performance;
- NVIDIA GPU performance;
- Qualcomm performance;
- hyperscale performance.

No physical GPU, smartphone, protected compositor, secure display controller, DPU, SmartNIC, or production TEE was used to produce a hardware-performance claim for this package.

The performance architecture is nevertheless designed around one important principle:

> **Cryptographically protect meaningful authorization transitions, not every frame.**

---

# 36. What This Repository Demonstrates

The repository demonstrates that the source Internet-Draft can be expressed as a concrete engineering contract containing:

- exact schemas;
- explicit state machines;
- explicit authority scope;
- protected validation evidence;
- independent Finality Sink verification;
- effect-specific enforcement;
- replay control;
- revocation control;
- policy and security epochs;
- family-device handover logic;
- minor-device hard-deny semantics;
- bounded Adult Viewing Context;
- Temporary Under-18 Mode;
- output-path separation;
- AI-content variations;
- offline lease logic;
- assurance profiles;
- testable failure conditions.

It provides a development blueprint for building a runnable implementation.

---

# 37. What This Repository Does Not Yet Demonstrate

It does not yet provide:

- production executable platform code;
- Android framework patches;
- iOS framework code;
- Linux DRM/KMS implementation;
- Windows graphics integration;
- NVIDIA driver integration;
- AMD driver integration;
- Qualcomm integration;
- commercial DRM integration;
- production protected-video-path integration;
- real biometric integration;
- real age-assurance-provider integration;
- production HSM/KMS integration;
- live commercial attestation;
- distributed replay service;
- formal verification;
- fuzzing results;
- penetration-test results;
- independent security audit;
- certification;
- real-device benchmark results.

These remain implementation and validation stages.

---

# 38. Non-Bypassability Requirement

The architecture is only as strong as the protected effect topology.

This is insufficient:

```text
Application
     |
     +------> Protected Finality Sink ------> Display
     |
     +------> Unprotected rendering path ---> Display
```

A claimed protected path should look more like:

```text
Application / Content Source
          |
          v
Protected Enforcement / Finality Path
          |
          v
Protected Sink
          |
          v
Perceptible Effect
```

If an untrusted component can directly reach an uncontrolled output path, the deployment must not claim that the output is protected by Rendering Finality.

---

# 39. Five Core Rules

1. **No continuous biometric surveillance.**
2. **No age credential is itself execution authority.**
3. **No valid finality authority means no protected effect.**
4. **No alternate path may bypass the protected effect boundary.**
5. **Cryptographic verification protects authorization transitions, not every frame.**

---

# 40. Recommended Engineering Workflow

A platform team evaluating the design can proceed in this order:

```text
1. Select protected content classes.
2. Select the claimed protected output paths.
3. Identify the actual non-bypassable Finality Sink(s).
4. Implement strict Candidate schema validation.
5. Define canonical Candidate serialization.
6. Implement PED policy evaluation.
7. Implement protected validation evidence.
8. Select cryptographic authority format.
9. Implement authority issuance.
10. Implement independent sink verification.
11. Implement replay / consumption state.
12. Implement policy and revocation epochs.
13. Implement device-registration state.
14. Implement bounded Adult Viewing Context.
15. Implement Temporary Under-18 transitions.
16. Implement output-specific authority.
17. Implement AI-transformation handling.
18. Execute adversarial test matrix.
19. Benchmark the actual device path.
20. Perform independent security review.
```

---

# 41. Implementation Checklist

Before describing a deployment as enforcing Child-Safe Rendering Finality, verify:

- [ ] Candidate Act exists before protected rendering.
- [ ] Candidate remains non-renderable while validation is incomplete.
- [ ] Strict schema validation is active.
- [ ] Unknown load-bearing fields are rejected.
- [ ] Candidate digest is deterministic.
- [ ] PED evaluates all deployment-required predicates.
- [ ] Age assurance alone cannot cause rendering.
- [ ] Protected validation evidence is committed.
- [ ] Rendering Finality Authority is cryptographically protected.
- [ ] Authority is scoped to required bindings.
- [ ] Finality Sink independently verifies the actual effect request.
- [ ] Current policy epoch is checked.
- [ ] Current revocation epoch is checked.
- [ ] Replay state is enforced.
- [ ] Output widening is prevented.
- [ ] Display authority does not imply cast or mirror.
- [ ] Minor-device baseline is protected where claimed.
- [ ] Adult registration alone does not render.
- [ ] Adult Viewing Context is bounded.
- [ ] Temporary Under-18 blocks new adult-only authority.
- [ ] Prior adult authority is invalidated/suspended/superseded at handover.
- [ ] Timer expiry does not auto-enable adult rendering.
- [ ] Protected adult re-authorization is required where policy requires.
- [ ] Security transitions trigger revalidation where required.
- [ ] Frame-window bounds are enforced.
- [ ] Session-bound authority cannot transfer freely.
- [ ] AI generation does not create authority.
- [ ] Offline operation remains bounded.
- [ ] Logs minimize personal information.
- [ ] Claimed protected paths are non-bypassable.
- [ ] Failure is fail-closed.
- [ ] Security claims match the actual implementation.

---

# 42. Package Files

The same implementation-reference material may be distributed as:

```text
child-safe-rendering-finality-github-reference.zip
```

or:

```text
child-safe-rendering-finality-github-reference.tar.gz
```

ZIP is convenient for general desktop use.

TAR.GZ is convenient for Unix/Linux/macOS development workflows and versioned release archives.

---

# 43. Source Specification

This repository is derived from:

```text
draft-das-child-safe-rendering-finality-03
```

The original RFCXML source is included in the package.

If this README and the source Internet-Draft ever differ, the Internet-Draft is the source of truth for the specification text.

---

# 44. Appropriate Technical Description

> **A comprehensive vendor-neutral implementation reference and engineering blueprint for Child-Safe Rendering Execution Finality across Android, iOS, GPUs, compositors, protected media paths, display, audio, cast, mirror and XR boundaries, including exact interoperability schemas, protected device-state transitions, scoped non-bearer authority, Finality Sink logic, normative requirements, and adversarial test design.**

---

# 45. Summary

The architecture separates three events that are often incorrectly collapsed:

```text
CONTENT EXISTS
      !=
CONTENT IS DELIVERED
      !=
CONTENT IS AUTHORIZED TO BECOME PERCEPTIBLE
```

The intended sequence is:

```text
Restricted Content
        |
        v
Candidate Act
        |
        v
Non-Renderable State
        |
        v
Protected Enforcement Domain
        |
        v
Protected Validation Evidence
        |
        v
Scoped Rendering Finality Authority
        |
        v
Protected Finality Sink
        |
        v
Perceptible Effect
```

For an adult-owned family device:

```text
Adult Viewing Context
        |
        v
Temporary Under-18 Mode
        |
        v
Prior Adult Authority Invalidated / Suspended
        |
        v
Temporary Minor Baseline
        |
        v
Protected Adult Re-Authorization
        |
        v
New Bounded Adult Context
```

For restricted AI-generated output:

```text
AI Generation
      |
      v
Candidate Output
      |
      v
Classification / Policy
      |
      v
Scoped Finality Authority
      |
      v
Protected Rendering Sink
```

The central design principle remains:

> **Permission to deliver content is not permission to render it.**

And the enforcement rule is:

> **No valid finality authority means no protected rendering effect.**
>
> # License

Copyright © 2026 Sangam Das. All rights reserved except as expressly licensed below.

## Creative Commons License

The documentation, diagrams, schemas, examples, explanatory material, and other copyrightable content in this repository are licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**.

Under this license, the material may be copied, shared, redistributed, adapted, studied, cited, indexed, and used for research or other non-commercial purposes, provided appropriate attribution is given.

Commercial use is not authorized under this license.

## Patent Rights Notice

Certain concepts, architectures, methods, protocols, execution-finality mechanisms, protected rendering mechanisms, Candidate Act structures, Rendering Finality Authority mechanisms, Finality Sink mechanisms, protected device-state transitions, child-handover protections, and related technical subject matter described in this repository may be covered by pending patent applications in the **DAS Protocols patent family**.

### Mothership Patent Application

**Title:** THE DAS PROTOCOLS
**PCT Application:** PCT/IB2026/055615
**WIPO Publication:** WO 2026/150382
**WIPO PATENTSCOPE:**
https://patentscope.wipo.int/search/en/detail.jsf?docId=WO2026150382

**Patent Pending. Patent Rights Reserved.**

The CC BY-NC 4.0 license applies only to applicable copyright rights in the repository materials.

It does **not** grant, imply, or convey any:

* patent license;
* covenant not to sue;
* patent-right waiver;
* commercial implementation right;
* manufacturing right;
* deployment right;
* sublicensing right; or
* other authorization under any patent or patent application.

Publication of source material, schemas, examples, implementation references, pseudocode, test vectors, technical descriptions, or related materials in this repository does not by itself constitute a patent license.

Any commercial implementation, manufacture, deployment, incorporation into products or services, or other activity that practices applicable patent claims must be separately authorized where required.

## Non-Commercial Research and Evaluation

Subject to CC BY-NC 4.0, the repository materials may be used for purposes such as:

* academic study;
* non-commercial research;
* standards analysis;
* interoperability study;
* security review;
* technical evaluation;
* citation;
* indexing;
* educational use; and
* non-commercial experimentation.

Such copyright permission does not expand into a patent license.

## Attribution

Suggested attribution:

**Sangam Das — Child-Safe Rendering Execution Finality: Hardware-Rooted Enforcement for Age-Restricted Content**

Related patent family:

**THE DAS PROTOCOLS — PCT/IB2026/055615 — WO 2026/150382**

Copyright material:

**CC BY-NC 4.0**

Patent-related technical concepts:

**Patent Pending — Patent Rights Reserved**

## License Reference

Creative Commons Attribution-NonCommercial 4.0 International:

https://creativecommons.org/licenses/by-nc/4.0/

