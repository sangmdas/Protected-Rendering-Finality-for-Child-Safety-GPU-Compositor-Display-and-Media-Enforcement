# Normative Requirements Index

> Extracted from `draft-das-child-safe-rendering-finality-03`. This file is an implementation checklist, not a replacement for the Internet-Draft.

Total source paragraphs containing RFC 2119/8174-style keywords: **62**.

## Dual-Boundary Architecture

1. A deployment MAY use two execution-finality boundaries.

## Restricted Content Candidate Act

2. The Candidate Act MUST remain non-renderable while required execution-finality validation is incomplete.

## JSON Interoperability Profile

3. This revision defines a JSON-based interoperability profile so that implementations can evaluate a concrete contract rather than only an abstract policy model. JSON objects MUST be encoded as UTF-8. Implementations MUST reject unknown load-bearing fields when the active schema declares additionalProperties=false.

## Protected Enforcement Domain Validation

4. The Protected Enforcement Domain (PED) MUST evaluate all predicates required by the applicable deployment before releasing Rendering Finality Authority. Typical predicates include recipient eligibility, age-threshold evidence, parental policy, content classification, content digest, jurisdiction, platform policy, device identity, application identity, requested output mode, output target, current policy epoch, current revocation epoch, nonce freshness, device attestation, and intended Finality Sink identity.

5. A successful upstream age check MUST NOT itself cause decryption or display. PED validation success authorizes creation of scoped finality authority; it does not complete the rendering consequence.

## Protected Validation Evidence

6. Before, or atomically with, Rendering Finality Authority issuance, the PED MUST commit protected validation evidence. The evidence MAY be represented by a protected signed record, MAC, enclave or HSM assertion, sealed state, monotonic-state commitment, append-only protected record, or equivalent protected mechanism.

## Rendering Finality Authority

7. Rendering Finality Authority MUST be act-bound, content-bound, recipient-bound, device-bound, application-bound where applicable, policy-epoch-bound, revocation-epoch-bound, nonce/freshness-bound, effect-bound, and sink-bound.

8. Possession alone MUST NOT be sufficient. The authority is non-bearer because the Finality Sink independently verifies the protected bindings before enabling the permitted materialization effects.

9. An authority for DISPLAY MUST NOT automatically authorize CAST or MIRROR. An authority for one device MUST NOT automatically authorize another device. An authority for one content digest MUST NOT authorize modified or substituted content.

## Where Enforcement Occurs

10. The most important deployment question is where the final technical gate is placed. The gate SHOULD control the first boundary at which the protected content becomes perceptible or can be trivially converted into a perceptible representation.

## Content-Key and Decryption Enforcement

11. Where encrypted or otherwise protected media is used, withholding the usable content key is a strong enforcement point. The key-release component MAY act as the Finality Sink or as one element of a chained finality path.

12. If validation fails, the usable key MUST NOT be released into an unprotected application context. If a key was previously released under a bounded authority, its validity SHOULD be constrained by session, content, policy epoch, time, frame window, or equivalent protected state.

## Decoder, GPU and Compositor Enforcement

13. For media that is already locally encrypted or decoded inside a protected pipeline, the Finality Sink MAY be placed at a decoder, GPU, protected-surface, or compositor boundary. The implementation MUST prevent ordinary application code from converting a denied Candidate Act into a visible frame through an alternate unprotected surface.

14. Protected rendering MAY therefore require that decoded frames remain in protected memory or protected surfaces until the compositor verifies current finality authority.

## Audio, Casting, Mirroring and Secondary Outputs

15. Adult or age-restricted content can be materialized through secondary paths even when a local display is controlled. Audio, HDMI or external displays, wireless casting, screen mirroring, remote-display APIs, accessibility capture paths, and similar outputs MAY therefore require separate consequence classes or sink-bound authority.

16. A local-display authority MUST NOT be interpreted as universal authority for every secondary output.

## Fail-Closed Requirements

17. If required authority is missing, malformed, expired, stale, replayed, revoked, consumed, content-mismatched, recipient-mismatched, device-mismatched, application-mismatched, policy-mismatched, output-mismatched, or sink-mismatched, the content MUST remain non-renderable.

18. Failure MUST NOT be converted into a warning followed by display. Implementations MUST NOT use 'best effort' rendering for content subject to this protected finality profile when current authority cannot be established.

## Replay, Revocation and State Changes

19. Eligibility and parental state may change after content is delivered. A device may also restore an old application snapshot or replay a previously valid rendering authority. The Finality Sink SHOULD therefore verify current policy and revocation epochs immediately before materialization and MUST enforce nonce or equivalent anti-replay state.

20. An old authorization MUST NOT automatically override a later parental-control change, account-state change, legal-policy change, device revocation, or recipient eligibility change.

## Generated and AI-Transformed Content

21. Generative AI may create age-restricted media that has no stable catalogue identifier. The Candidate Act therefore supports content digests, generation-model identifiers, classification sources, and protected output state. A generated object SHOULD be bound to the classification and authority actually used for the attempted rendering.

## Privacy Considerations

22. Child-safety enforcement can itself create privacy risk if systems unnecessarily expose a child's identity, exact age, browsing history, or content choices. Implementations SHOULD minimize disclosure and MAY consume privacy-preserving eligibility proofs that state only whether the required threshold or policy condition is satisfied.

23. Logs and validation evidence SHOULD avoid recording unnecessary content titles, URLs, identity attributes, or detailed viewing history where a digest, opaque identifier, or protected commitment is sufficient.

## Security Considerations

24. Implementations SHOULD place load-bearing checks within protected execution, trusted operating-system, hardware-backed, or equivalently isolated enforcement paths when the threat model includes compromise of ordinary application software.

## Interoperability with Existing Age-Assurance and Platform Systems

25. The execution-finality layer is designed to consume outcomes from existing systems rather than replace them. Inputs MAY come from privacy-preserving age verification, platform account state, parental-control systems, content classifiers, regulatory policy, trusted identity providers, device policy, or enterprise/family safety services.

## Adult-Owned Device With Possible Minor Co-Use

26. During device setup, family-safety configuration, or later security settings, an adult MAY declare that the device is primarily an adult-owned device but may sometimes be used by a minor son, daughter, or other child. This declaration does not convert the device permanently into a Minor-Registered Device. Instead, it informs the protected device policy that family handover is an expected operating condition and that adult-content authority must not be treated as an indefinite property of device ownership.

27. A device in this shared-use configuration MAY support a bounded Adult Viewing Context for the adult's own use. The adult can establish that context through a protected local verification mechanism, after which multiple restricted-content items MAY be rendered within the same valid, scoped session or epoch without requiring a fingerprint, face scan, passkey, or PIN for every individual video, image, page, or frame.

28. This is important for practical deployment. Requiring a new biometric or fingerprint operation for every restricted-content object would create excessive friction and could make ordinary adult use unacceptable. The security boundary SHOULD therefore authenticate meaningful state transitions rather than every frame or every video. A sufficiently recent adult verification can authorize a bounded protected adult session, subject to expiry, revocation, sink binding, policy changes, or other revalidation conditions.

## Temporary Under-18 Handover Mode

29. Before an adult hands the device to a minor, the adult MAY activate a protected Temporary Under-18 Mode from device settings, family controls, a protected quick-setting surface, or another implementation-defined user interface. The mode is intended to be simple enough to use at the moment of handover.

30. Once Temporary Under-18 Mode becomes active, the protected rendering architecture MUST treat the device, for the protected restricted-content classes governed by this profile, as though the applicable child-safety baseline were active. Adult-only Rendering Finality Authority MUST NOT be newly issued while that mode is active.

31. Activation of Temporary Under-18 Mode SHOULD invalidate, suspend, consume, or cryptographically supersede any prior Adult Viewing Context or Rendering Finality Authority that would otherwise permit adult-only rendering. A protected policy or security epoch SHOULD advance, or equivalent protected state SHOULD change, so that stale adult authority cannot simply survive the handover.

32. While Temporary Under-18 Mode is active, an existing adult account login, stored OVER_18 credential, browser cookie, application token, cached adult session, downloaded media object, previously released application state, or earlier adult verification MUST NOT by itself restore adult rendering authority. The Protected Rendering Finality Sink continues to require authority consistent with the current temporary minor state.

33. The adult MAY select a bounded duration, such as a short family-use interval, or MAY select an explicit-until-disabled mode. However, expiration of a timer SHOULD NOT silently restore active adult Rendering Finality Authority while the device may still be in the child's possession. A safer implementation can transition at timer expiry to an unverified adult state in which adult-only rendering remains non-renderable until the adult performs protected reactivation.

34. Leaving Temporary Under-18 Mode and restoring an adult rendering context MUST require an adult-authorized protected transition when the applicable policy requires it. In simple terms, entering the safer state can be easy; leaving the safer state is protected.

## Why This Is Preferable to Requiring Verification for Every Video

35. This architecture therefore separates adult session establishment from child handover protection. For the adult's own continuous use, the system MAY rely on a bounded Adult Viewing Context or Finality Lease that permits multiple items or many frames within a defined scope. When the adult expects the device to leave his or her control, Temporary Under-18 Mode provides a deliberate handover boundary.

## Device Registration and Age-Class Baseline

36. This case study distinguishes a device registered or provisioned for a minor from a device registered or provisioned for an adult. A device MAY obtain its protected age-class baseline from a verified date of birth, a privacy-preserving age credential derived from that date of birth, a family or platform enrollment record, or another trusted age-assurance process accepted by policy.

37. If the verified registration state establishes that the registered user is below the applicable adult threshold, the device is a Minor-Registered Device under this profile. A Minor-Registered Device MUST NOT issue Rendering Finality Authority for content classified as adult-only or otherwise prohibited to that age class. Adult content therefore remains non-renderable on that device even if an adult account is later logged in, an application possesses an adult service credential, or adult content bytes are downloaded or cached.

38. The Minor-Registered Device rule is a device-policy floor, not a claim that the operating system continuously identifies the person holding the phone. Ordinary application login, account switching, possession of an OVER_18 service credential, or a previous adult session MUST NOT silently override the protected minor registration state. If policy permits the device itself to change from minor-registered to adult-registered status, that change MUST occur through an explicit authorized re-provisioning or age-registration procedure rather than through ordinary content access.

39. If the verified registration state establishes an adult user, the device is an Adult-Registered Device. Adult registration makes the device eligible to request adult-content authority, but it MUST NOT itself authorize adult rendering. When adult or other age-restricted content is about to become perceptible, the protected device environment MUST require fresh or sufficiently recent verification of the adult-authorized user as required by the applicable assurance profile. Only after that verification succeeds may the PED issue a bounded Rendering Finality Authority for the protected session.

## Threat Scenario

40. The following facts MUST NOT, by themselves, authorize rendering for the child:

## Required Five-Way Separation

41. A deployment addressing registered-device age state and shared adult/minor use SHOULD separate five states:

## Protected Adult Viewing Context

42. A higher-assurance deployment MAY maintain a protected Adult Viewing Context as local protected state. The protocol does not require one universal biometric or identity mechanism. The context may be established by an implementation-specific user-presence mechanism such as a secure device PIN, passkey, local biometric confirmation, trusted wearable confirmation, or another protected user-verification mechanism accepted by policy.

43. The Adult Viewing Context MAY be bound to device identity, protected user or profile context, application identity, content class, active session, policy epoch, revocation epoch, intended rendering sink, freshness state, and an expiration or revalidation condition. These bindings may be represented inside the Rendering Finality Authority or maintained as protected local state checked by the PED and Finality Sink.

44. An implementation MUST NOT infer current adult presence solely from possession of a previously issued OVER_18 credential or from the continued existence of an adult account session.

## Normal Adult Rendering Flow

45. For usability, a deployment MAY authorize a bounded series of rendering operations inside the same protected Adult Viewing Context. The adult need not re-authenticate for every video when device, session, sink, policy, freshness, and other required bindings remain valid. Revalidation is associated with meaningful state changes, expiry, handover, or risk transitions rather than with every frame.

## Adult-Registered Phone: Verification Before a Protected Adult Rendering Session

46. Consider a phone registered to an adult. The device's adult registration does not create a permanent adult-content pass. Before establishing or renewing a protected adult rendering session, the protected environment requests fresh or sufficiently recent verification of the adult-authorized user, for example through a protected PIN, passkey, biometric confirmation, or another policy-approved local verification mechanism. The architecture does not require a new biometric interaction for every video or frame; a successful verification MAY establish a bounded Adult Viewing Context covering multiple permitted items while its protected conditions remain valid.

## Handoff to the Minor

47. When the device moves outside the protected adult viewing context, the implementation SHOULD invalidate, suspend, or require revalidation of the corresponding Rendering Finality Authority according to the applicable assurance profile. Relevant state changes can include:

## Immediate Handoff While Content Is Already Playing

48. A deployment requiring stronger protection SHOULD therefore use bounded or event-driven revalidation rather than issuing an indefinitely valid adult session. Revalidation MAY be triggered by lease expiry, inactivity, screen-off/screen-on transition, application transition, a new restricted title, content-class escalation, output-path change, a high-risk policy event, or another protected continuity signal.

49. For high-risk content, an implementation MAY pause, blank, mute, or otherwise return the protected output to a non-perceptible state before requesting revalidation. Rendering resumes only if the required adult-authorized context is re-established and new or renewed Finality Authority is successfully verified.

50. A deployment MAY also use privacy-preserving local continuity mechanisms, but this document does not require continuous camera monitoring, continuous facial recognition, or continuous transmission of biometric information. The protocol requirement concerns the validity of current authority, not continuous identification of a human viewer.

## Security Claim and Limitation

51. The defensible security claim is narrower and technically important: a successful adult age check, an adult-owned device, an adult account, or a previous adult session MUST NOT automatically become permanent or freely transferable restricted-rendering authority for every later user of that device.

## Core Rules Derived from the Case Study

52. A device registered or provisioned as belonging to a minor, based on verified date of birth or equivalent trusted age-class evidence, MUST NOT issue adult-only Rendering Finality Authority under the strict child-safety profile.

53. Ordinary adult account login or possession of an adult service credential MUST NOT override a protected minor-device registration state.

54. An adult-registered device MUST still require fresh or sufficiently recent adult verification before adult-content Rendering Finality Authority may issue.

55. Adult device ownership MUST NOT be treated as adult rendering authority.

56. An adult account login MUST NOT be treated as indefinite restricted-content authority.

57. An age credential establishes eligibility; it MUST NOT by itself establish indefinite current-viewer authority.

58. A child profile MUST NOT inherit a previously issued adult Rendering Finality Authority.

59. A materially changed device, session, profile, application, output, policy, revocation, or protected-presence state SHOULD trigger expiry, suspension, or revalidation as required by the applicable assurance profile.

60. Where current adult authorization cannot be established, the Protected Rendering Finality Sink MUST keep the restricted effect non-renderable.

61. A device-age or parental-control setting that is enforced only at an upstream application or account boundary MUST NOT be treated as equivalent to protected rendering finality when alternate materialization paths remain available.

62. If a deployment claims that its age-setting mechanism is itself sufficient final enforcement, every protected materialization path within that claim MUST enforce the current setting or authority before the restricted effect becomes perceptible and MUST fail closed when that enforcement state cannot be established.
