# Feedback on Topic C

The text would benefit from clarification of the security events that lead to revocation, what the ecosystem roles are requirements are, how information propagates, and what he use case requirements are. The present lack of clarity makes it impossible to meaningfully discuss WUA revocation.

The text provides partial insights but leaves many questions unanswered.

* In section 2.1. User requested revocation is mentioned. While a nice requirement, it is not clarified how to prevent unauthorized User requested revocation, which ought to be especially challenging in cases where the user's device is lost.
* Section 2.1. mentions that a legal authority can trigger a revocation. This requires the introduction of a mechanism that allows a party other than the user to trigger revocation. How would you prevent misuse of such a mechanism?
* Section 2.2. mentions that the PID Provider must revoke any PID associated with the WUA. This is an extremely challenging task and it is not clear why this very challenging approach was taken to prevent the user from presenting a still valid PID from an Wallet that cannot produce a valid WUA.

The following is recommended:

* Provide a more exhaustive list of security events would be welcome, including e.g., a WSCD / WSCA breach, LoA H identification used during onboarding is revoked etc.
* Clarify who actually controls the WUA revocation and analyze the effects of this control. While it is unproblematic to assume that the Wallet Provider, i.e., WUA issuer, can revoke a WUA, it is not entirely unproblematic to also allow Users and attestation Issuers to revoke a WUA. It is not evident why e.g., a PID Provider should be able to trigger a WUA revocation. The security events that would allow such triggers need to be clarified.


Additional points:

* Section 3 mentions privacy requirements without explicitly detailing how these requirements should be achieved. This is a massive black box currently that is pragmatically impossible to achieve currently with existing tools.
* What is the revocation requirement, really?
  * Is there a scenarios where a Verifier needs to know that a WUA is revoked within a 24 hour period?
  * What are the responsibilities of Issuers when learning about WUA revocation? Presently, the text suggest chain revocation without clear justification of why that approach is needed.
  * More generally, when should an Issuer access WUA status information?
* The text includes both a reliance on pull-based flows, and push-based flows. This is not entirely ideal.
  * Section 2.3. CIR 2024/2979 Article 7.4. states that Wallet Providers shall "make publicly available the validity status" and "describe the location of that information" in the WUA. Wording like "publicly available" and "describe the location" suggests a proactive pull-based.
  * Section 3 requirement 2 suggests that the validity status check is performed based on the information in the WUA, i.e., pull-based, as opposed to receiving automatic validity status updates.
  * Section 5.1. WUA_08b and WUA_08d implies that the validity status must be requested, i.e., pull-based.
  * Section 2.3. CIR 2024/2979 Article 7.3 requires that the Wallet Provider actively notifies, i.e., a push-based approach, wallet Users. Consequently, Wallet Providers must support a push-based approach. However, there is no equivalent flow suggested for attestation Issuers or Verifiers.

Assuming that we actually do need to be able to revoke a WUA (which seemingly introduces massive complexity without any counterbalancing benefit when compared to simply just relying on short-lived WUAs) then it is vital that the information flows and the approach is clarified. Arguably, there are three main mutually-exclusive parameters that a revocation approach can optimize for:

1. Real time validity status checks.
2. Scalability and efficiency.
3. Privacy.

Out of the three above, a focus on privacy is likely going to demand significant efforts and further development; far beyond what is advisable for the EUDIW presently. Technologies that rely on cryptographic accumulators or ZKP for unlinkable revocation checks are possible, but far from practical presently and would require a lot of standardization efforts. Even seemingly simple solutions like W3C Status List 2021 has challenges that we need to clarify and discuss suitable approaches for.

If any of the other two are a primary concern, there are existing solutions that offer highly scalable validity status checks or those that offer timely checks. But even these solutions present unique scalability or timeliness challenges that must be addressed. E.g., OCSP must be designed to handle traffic spikes and to resist DoS attacks. Solution suitable for long-term attribute attestations that can simultaneously perform well at scale, offer timely validity status updates, and offer reasonable assurances with regards to user privacy are rare. The one we are aware of is RFC 6961, i.e., OCSP Must Staple. Others are either experimental (e.g., Sparse Merkle Trees as used in Revocation Transparency) or rather mature and easy to understand but require much work to be suitable for WUA revocation (e.g., W3C Status List 2021).

However, if we conclude that RFC 6961 is the approach that represents the tradeoffs we are comfortable with, then we must ask what the benefit of using OCSP Must Staple is over the User simply requesting a new short lived attestation. It would seem that simply reissuing the attestation achieves the same goal, but requires far less added overhead and complexity for all parties involved.

In our view, until such time that solutions tailor made to address privacy, scalability, and timeliness mature, we should simply focus on short lived attestations and completely avoid a mountain of challenges that will surely and needlessly burden us. Short lived attestations may be needed in any case if we require hard-failures given that any and every revocation mechanism is subject to failure, and that the arguably best backstop is a short lived token.

## A a brief comparison of existing approaches that can guide efforts

Any solution should match a throughout and detailed analysis as requested above. However, it is still worthwhile to consider possible approaches just to highlight what monumental challenges lie ahead if we insist of pursuing long lived attestations as opposed to just re-issuance of short lived ones.

* CRLs require clients to periodically download a CRL. Monitoring can be made easy with publishing a hash digest of the latest CRL and delta updates can help scalability. Offers herd based privacy primarily based on the CRL range. Revocation surges are problematic to propagate and present significant scalability challenges. Timeliness is delayed and is thus only suitable in cases where the Verifier can accept this delay (e.g., 24h). Existing PKI solutions often use soft-failure to preserve service availability and it is unclear how CRLs will work on scale when hard-failure is enforced. Widely used, fully standardized, and not technically complex. Will likely not work well for the WUA.
* W3C Status List 2021 is an alternative to a CRL that will likely scale a lot better. But bitstrings with random revocations are not as space efficient as the W3C specification may hint. Furthermore, a fixed size bitstring requires additional measures when considering batch issuance, and is not as easy to manage as a CRL that is practically unconstrained. Thus there is a lot of standardization required. Easy to layer a ZKP ontop to provide complete privacy instead of herd privacy.
* OCSP / OCSP Must staple (RFC 6960 and RFC 6961) require a client query. Privacy is high when used in Must Staple mode since it is the User that requests the OCSP response and staples it with the attestation. Increases scalability dramatically where the stapled OCSP response can be reused. Increases scalability overall since an extra round-trip to the CA is eliminated. Timely and widely used with low complexity and good standards support. Will likely work well for the WUA, but is very similar to simply relying on the easier short lived WUA approach.
* OAuth 2.0 token revocation (RFC 7009) can inspire solutions where the client requests a revocation through an authenticated call. Very lightweight and highly scalable. Timeliness is instant on the server side and any real time revocation check will immediately show the updated validity status. Widely adopted, although for session management and not for attestation revocation (unclear how, or even whether, it fits the EUDIW ecosystem needs).
* Using an event-driven subscription model. The issuer can push revocation events via a suitable mechanism. Attestation Issuers and Users (and also Verifiers, but there is little reason why a Verifier should ever need to see the WUA if we rely on short lived attestations) can register at an endpoint and receive event notifications. Scales well with a moderate amount of subscribers, but is very timely as subscribes are notified instantly following a revocation. Rather common but requires standardization to align on a common approach (e.g., RFC 8417).
* Revocation Transparency is a push based aggregate revocation mechanism. Although distribution is highly scalable (due to Merkle Proofs and small proof sizes), the propagation of revocation information is not. Only useful for highly critical revocations and an estimated 99% of revocations never get propagated. Similar accumulator based approaches seem to exhibit the same problems with actually being able to propagate revocation information.
* Other cryptographic accumulators tend to all share the same characteristics: distribution is highly scalable, but the maintenance of the accumulator is incredibly challenging. Work is done to reduce this maintenance but everything is still experimental and early stage.
* Relatedly, ZKP solutions that can provide proofs that presented attestation is not revoked as of the latest epoch are highly complex and offer limited benefits over OCSP Must Staple. Arguably, the benefits of ZKP seem to be that it does not allow for communication monitoring between the User and the Issuer. If we use single show attestations, the benefits of ZKP over OCSP Must Staple seem to vanish in all but high-privacy contexts.
