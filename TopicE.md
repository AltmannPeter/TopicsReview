## Legal background

### Main legal text 910/2014

The [main legal text](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A02014R0910-20241018) establishes a permissive stance for the use of pseudonyms as clarified in Article 5:

> Without prejudice to specific rules of Union or national law requiring users to identify themselves or to the legal effect given to pseudonyms under national law, the use of pseudonyms that are chosen by the user shall not be prohibited.

This establishes that pseudonym use may be restricted by legal requirements and confirms that pseudonyms are user-chosen.

Article 5a.4.(b) introduces functional requirements for the EUDIW Solution:

> European Digital Identity Wallets shall enable the user, in a manner that is
user-friendly, transparent, and traceable by the user, to: [...] (b) generate pseudonyms and store them encrypted and locally within the European Digital Identity Wallet;

This mandates that the EUDIW Solution must support local, encrypted storage and generation of pseudonyms. Other pseudonym generation methods are not explicitly forbidden.

Article 5b.9 establishes when a relying party can rely on a pseudonym:

> Relying parties shall not refuse the use of pseudonyms, where the identification of the user is not required by Union or national law.

This does not preclude other valid reasons for refusing pseudonyms; it defines legal mandate as a reason for refusal.

Articles 32.1(e) and 32a.1(e) specify that pseudonyms must be clearly marked:

> the use of any pseudonym is clearly indicated to the relying party if a pseudonym was used at the time of signing;

Article 5f.3 is relevant for online platforms in particular:

> Where providers of very large online platforms [...] require user authentication for access to online services, they shall also accept and facilitate the use of European Digital Identity Wallets that are provided in accordance with this Regulation for user authentication only upon the voluntary request of the user and in respect of the minimum data necessary for the specific online service for which authentication is requested.

Though pseudonyms are not mentioned directly, the article emphasizes data minimization in authentication processes.

In sum, user-chosen pseudonyms must be a core wallet function. Service providers must accept pseudonyms unless:

1. legal obligations require user identification (clearly stated).
2. the service provision requires unique identification of the identity subject (highly likely).

#### Annexes

Pseudonyms are referenced in several annexes, primarily for syntactic clarity and to specify that, when used, they must be explicitly marked.

Annexes I, IV, and VII note that a pseudonym may be used in place of a name (i.e., used as an attribute), provided it is clearly indicated as such. 

In contrast, Annex V does not specify that a pseudonym can be used in place of a name as an attribute, but provides clarifications:

> a set of data unambiguously representing the entity to which the attested attributes refer; if a pseudonym is used, it shall be clearly indicated;

This highlights that an attestation, even if it includes a pseudonym, must still unambiguously represent the identity subject.

Overall, the annexes do not define what constitutes a pseudonym or how it is managed or linked to an identity subject. There is no operational or semantic guidance on pseudonym handling, including aspects such as persistence (e.g., single-use, session-based, or pairwise), assignment authority, identity mapping, and any usage restrictions.

### Amendment 2024/1183

[Regulation (EU) 2024/1183](https://eur-lex.europa.eu/eli/reg/2024/1183/oj/eng) amends Regulation 910/2014 and explicitly addresses pseudonym use and adds semantic detail.

Recitals (19), (22), and (57) emphasize that the EUDIW Solution should be able to generate user-chosen pseudonyms for purposes of authentication and that online platforms should respect the right of the user to use a pseudonym.

> (19) Reliance on the legal identity should not hinder European Digital Identity Wallet users to access services under a pseudonym, where there is no legal requirement for legal identity for authentication.
> (22) European Digital Identity Wallets should include a functionality to generate user-chosen and managed pseudonyms, to authenticate when accessing online services.
> (57) [...] very large online platforms should accept [the EUDIW] for [authentication], while respecting the principle of data minimisation and the right of the users to use freely chosen pseudonyms.

Recital (60) addresses the relationship between legal identification requirements and pseudonym use:

> (60) Unless specific rules of Union or national law require users to identify themselves, accessing services by using a pseudonym should not be prohibited.

This confirms that legal identification requirements can justify prohibiting pseudonym use but does not state that such legal grounds are the only valid basis for doing so.

### CIR 2024/2979

[CIR 2024/2979](https://eur-lex.europa.eu/eli/reg_impl/2024/2979/oj/eng) details integrity and core functionality requirements for the EUDIW. It mentions pseudonyms in two places.

Recital (14) clarifies that pseudonyms should enable the user to authenticate without providing unnecessary information to the service provider.

> The generation of wallet-relying party specific pseudonyms should enable wallet users to authenticate themselves without providing wallet-relying parties with unnecessary information. As set out in Regulation (EU) No 910/2014, wallet users are not to be hindered from accessing services under a pseudonym, where there is no legal requirement for legal identity for authentication. Therefore, the wallets are to include a functionality to generate user-chosen and managed pseudonyms, to authenticate when accessing online services. The implementation of the specifications set out in Annex V should enable these functionalities accordingly. Further, wallet-relying parties are not to request users to provide any data other than those indicated for the intended use of wallets in the relying party register. Wallet users should be enabled to verify the registration data of relying parties at any point in time.

The text suggests that even pairwise pseudonyms are user-chosen and managed.

Article 14 further details the EUDIW functionality regarding pseudonyms.

> 1.   Wallet units shall support the generation of pseudonyms for wallet users in compliance with the technical specifications set out in Annex V.

> 2.   Wallet units shall support the generation, upon the request of a wallet-relying party, of a pseudonym which is specific and unique to that wallet-relying party and provide this pseudonym to the wallet-relying party, either standalone or in combination with any person identification data or electronic attribute attestation requested by that wallet-relying party.

These provisions confirm that pseudonyms are pairwise distinct and that relying parties can request person identification data alongside the pseudonym.
