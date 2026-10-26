# CCPA (ccpa)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

The California Consumer Privacy Act (CCPA), amended by the California Privacy Rights Act (CPRA), is a state statute that grants California residents rights over their personal information: the right to know, delete, correct, opt-out of sale/sharing, limit use of sensitive personal information, and non-discrimination for exercising privacy rights. It is enforced by the California Privacy Protection Agency (CPPA) and the California Attorney General. Technical interoperability mechanisms include the Global Privacy Control (GPC) browser signal and the IAB Tech Lab US Privacy (USP) / Global Privacy Platform (GPP) signals for advertising technology. This index tracks the official regulatory resources, technical privacy signals, and commercial APIs that help businesses comply with CCPA/CPRA obligations.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/ccpa/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags:

 - Privacy, Compliance, Data Protection, Legal, California, CPRA, Regulation, Data Subject Rights

## Timestamps

- **Created:** 2025-01-01
- **Modified:** 2026-04-23

## APIs

### Global Privacy Control (GPC) Specification
Global Privacy Control is a browser-level signal that communicates a user's opt-out preference to websites. The California Attorney General has affirmed that GPC must be treated as a valid CCPA "Do Not Sell or Share" opt-out request.

**Human URL:** [https://globalprivacycontrol.org/](https://globalprivacycontrol.org/)

#### Tags:

 - Browser Signal, Opt-Out, Standard

#### Properties

- [Website](https://globalprivacycontrol.org/)
- [Specification](https://privacycg.github.io/gpc-spec/)
- [SourceCode](https://github.com/privacycg/gpc-spec)

### IAB Tech Lab Global Privacy Platform (GPP)
The IAB Tech Lab Global Privacy Platform (GPP) is the successor to the US Privacy (USP) string. It provides a standardized way to communicate user consent and opt-out signals between publishers, consent management platforms, and adtech vendors for CCPA, CPRA, and other jurisdictions.

**Human URL:** [https://iabtechlab.com/gpp/](https://iabtechlab.com/gpp/)

#### Tags:

 - IAB, Consent, AdTech, Signals

#### Properties

- [Documentation](https://iabtechlab.com/gpp/)
- [SourceCode](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform)
- [LegacySpec](https://github.com/InteractiveAdvertisingBureau/USPrivacy)

### California Privacy Protection Agency (CPPA) Resources
Official resources from the California Privacy Protection Agency, the body empowered by CPRA to implement, enforce, and publish regulations under the CCPA.

**Human URL:** [https://cppa.ca.gov/](https://cppa.ca.gov/)

#### Tags:

 - Regulation, Enforcement, Rulemaking

#### Properties

- [Website](https://cppa.ca.gov/)
- [Regulations](https://cppa.ca.gov/regulations/)
- [Enforcement](https://cppa.ca.gov/enforcement/)

### California Data Broker Registry
Official California Attorney General registry of data brokers required to register under Civil Code section 1798.99.80, providing a public list that consumers can use to submit opt-out requests.

**Human URL:** [https://oag.ca.gov/data-brokers](https://oag.ca.gov/data-brokers)

#### Tags:

 - Data Brokers, Registry

#### Properties

- [Registry](https://oag.ca.gov/data-brokers)
- [Registration](https://oag.ca.gov/data-brokers/submit)

## Common Properties

- [Website](https://oag.ca.gov/privacy/ccpa)
- [Documentation](https://oag.ca.gov/privacy/ccpa)
- [Regulator](https://cppa.ca.gov/)
- [StatuteText](https://leginfo.legislature.ca.gov/faces/codes_displayText.xhtml?lawCode=CIV&division=3.&title=1.81.5.&part=4.&chapter=&article=)
- [Regulations](https://cppa.ca.gov/regulations/)
- [Enforcement](https://cppa.ca.gov/enforcement/)
- [DataBrokerRegistry](https://oag.ca.gov/data-brokers)
- [GPC](https://globalprivacycontrol.org/)
- [GPP](https://iabtechlab.com/gpp/)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
