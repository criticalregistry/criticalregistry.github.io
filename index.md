# The Invisible Foundation
## How Critical Open Source Infrastructure Became the World's Most Undermanaged Risk — and What to Do About It

*Holger Schmidt*  
*April 2026*

---

## Executive Summary

Global digital infrastructure — including connected products in automotive, medical devices, industrial automation, and rail — depends on a layer of open source software components that few organisations have mapped and almost none have funded. These components are maintained, in many cases, by individual people working without institutional support, in programming languages with shrinking developer pipelines, against a backdrop of nation-state actors who have identified maintainer burnout as a systematic attack surface.

This is not an engineering concern. It is a balance sheet problem. The organisations whose products depend on this infrastructure carry an unpriced liability — one that is becoming harder to ignore as regulation tightens, insurance underwriters ask sharper questions, and the incidents grow larger with each cycle.

The numbers are not small. Twenty billion installations of a single networking library with one primary maintainer since 1996. Most of the world's encrypted internet traffic secured by a library that had a single full-time developer when its most critical vulnerability was discovered in 2014. A nation-state attack in 2024 that came within days of shipping a backdoor into SSH on most Linux distributions — not through a technical exploit, but through approximately three years of patient relationship-building with one exhausted volunteer maintainer.

Each incident has had a larger blast radius than the last. In 2025 and early 2026, the pattern has continued and accelerated: a widely-used XML parsing library briefly lost its only maintainer, declaring the project "more or less unmaintained for now"; a Kubernetes component used in approximately half of all cloud-native environments was formally retired after its volunteer team burned out; and AI-assisted vulnerability discovery tools identified 271 vulnerabilities in a single browser codebase in one evaluation pass — a number that exceeds the entire previous year's high-severity findings for the same project. The next major incident will be in hardware: in automotive firmware, in medical devices, in industrial control systems where patching is measured in years, not days — and where the questions are not just "can we patch" but "can we still get a patch at all, and do we even know what we're running."

This paper argues that the problem has a correctable structure — but that the entity needed to correct it does not yet exist. It proposes what that entity looks like, how it operates, and why it is a business rather than a charity.

---

## Part 1: The Problem

### 1.1 The Software Your Products Depend On

Every connected product shipped today contains open source components. This is not a choice — it is simply what modern software development looks like. Open source libraries handle encryption, network communication, image processing, time synchronisation, logging, parsing, and hundreds of other functions that no organisation would duplicate if they could avoid it.

The commercial logic is sound. A car manufacturer's core competency is not implementing the TLS cryptographic protocol. A medical device company's core competency is not writing an HTTP client. Using well-tested, widely deployed open source components for these functions is rational. The problem is not that organisations use open source software. The problem is that they have systematically failed to look at what happens at the end of that supply chain.

The supply chain terminates, repeatedly, at individuals.

Consider a few examples that are not edge cases — they are the infrastructure on which critical products run today:

**libcurl** is the software library responsible for data transfer across networks. It is present in an estimated twenty billion installations: in cars from almost every major manufacturer, in medical devices, in game consoles, in nuclear fusion experiments, in every major operating system, in the firmware of industrial controllers. It has been maintained since 1996 primarily by Daniel Stenberg, a Swedish engineer employed full-time by wolfSSL since 2019 to work on curl — the first sustained commercial arrangement of its kind for the project. curl has hundreds of contributors, but the contributor community cannot sustain the project without Stenberg. When a vulnerability is found in libcurl, the question of how quickly it can be assessed, triaged, and coordinated across the installed base is, in material part, a function of one person's continued availability and engagement.

**OpenSSL** provides the cryptographic layer for most encrypted internet traffic — including authentication, data-in-transit protection, and certificate validation across enterprise, government, and consumer infrastructure. When the Heartbleed vulnerability was publicly disclosed in April 2014, OpenSSL had one full-time developer. The bug had been present in the codebase since 2012 — two years during which any attacker could have been reading memory from any vulnerable server without leaving a trace. The estimated remediation cost was approximately $500 million, considering only the cost of certificate revocation and reissuance. Actual costs, including human resources, emergency audit programmes, and the irretrievable exposure of credentials and private keys, were substantially higher.

**Samba** implements the SMB network file-sharing protocol, providing the interoperability layer between Windows and Linux/Unix systems across enterprise, government, and industrial environments. It underpins file sharing, printer sharing, and Active Directory integration in organisations ranging from law firms to railway operators to defence contractors. Its active committer base numbers approximately ten to fifteen people — the project's own team page states that "the number of people actively doing Git checkins is approximately 10–15" — with a core codebase written in C. It is funded episodically — most recently by the German Sovereign Tech Fund at approximately €688,000, managed by SerNet over 18 months.

**zlib, libjpeg, libpng, expat, FreeType, BIND, and glibc** follow the same pattern. These components — handling compression, image parsing, DNS resolution, text rendering, and the fundamental C standard library — are embedded in billions of devices and depend on teams of five to fifteen people, many of them volunteers, some of them operating without any institutional funding at all.

The XKCD web comic captured the structural reality in 2020 with a single image: a vast tower of modern infrastructure balanced on a small block labelled "A project some random person in Nebraska has been thanklessly maintaining since 2003." It was drawn as a joke. It was also a reasonably accurate engineering diagram.

### 1.2 Why This Is Different From Normal Software Risk

In proprietary software, the liability sits with the vendor. If a vulnerability is found in a Microsoft product, Microsoft bears responsibility for the patch, the timeline, and the consequences of inaction. The risk is priced into the licence fee, the support contract, and ultimately the vendor's share price.

In open source software, the liability sits with whoever ships the product. The open source project itself typically disclaims all warranties and liabilities in its licence terms, and it is entirely within their rights to do so — the software is provided free of charge, developed as a public good. The "free" in open source means free to use. It does not mean free of risk.

The consequence is that organisations using open source components have, in effect, taken on supplier risk without the standard mechanisms for managing it: no service level agreement, no contractual obligation to patch, no indemnification, no escalation path. In many cases, they have done this without even cataloguing what they are using. The Log4Shell incident in 2021 demonstrated this at scale: enterprises across the world could not answer the basic question "do we use Log4j" for weeks after a critical vulnerability was disclosed. Not because their security teams were incompetent, but because the component was embedded in hundreds of third-party tools and applications, layered several levels deep in dependency trees that nobody had bothered to map.

The legal landscape is shifting — and shifting quickly. The EU Cyber Resilience Act entered into force in December 2024, with vulnerability reporting obligations taking effect in September 2026 and full enforcement from December 2027. The regulation requires manufacturers of products with digital elements to know what components are in their products, to maintain them securely across the product lifecycle, and to report actively exploited vulnerabilities within 24 hours of becoming aware. The FDA's updated software guidance creates similar obligations in the medical device sector. SEC cyber disclosure rules in the United States require public companies to report material cybersecurity incidents and describe their risk management processes.

Each of these regulatory instruments creates the same practical requirement: organisations need to know what software is in their products. Many of them currently do not. And the act of discovering what is in their products will, for a significant number of organisations, surface dependencies on components that are materially unmaintained.

Insurance underwriters are arriving at the same place from a different direction. Cyber insurance renewal questionnaires are increasingly asking about software bill of materials practices, open source dependency management, and patch management for third-party components. For organisations that cannot answer these questions, the consequences are higher premiums, coverage exclusions, or — increasingly — refusal to underwrite. The risk is being repriced. The question is whether manufacturers will price it themselves or wait for insurers and regulators to do it for them.

### 1.3 Recent Evidence That This Is Not Theoretical

The risk described in this paper is not a hypothetical. It is a pattern that has already generated multiple major incidents, each larger than the last, and each demonstrating the same underlying structural failure.

**Heartbleed (2014)** was a missing bounds check in the OpenSSL heartbeat extension — a single absent line of validation logic that had been present in the codebase since 2012. When disclosed in April 2014, approximately 17.5% of all SSL/TLS-secured servers were vulnerable, allowing an attacker to read arbitrary chunks of server memory — including private keys, session tokens, and credentials — without authentication and without leaving a trace. The cryptographer Bruce Schneier called it "catastrophic." The EFF called it "catastrophic." At the time of disclosure, OpenSSL had one full-time developer. Remediation — including emergency audits, certificate revocation, reissuance, system patching across millions of servers, and credential rotation — cost an estimated $500 million by the most conservative accounting. The actual figure was higher.

The response was the Core Infrastructure Initiative: a multi-million-dollar fund established by the Linux Foundation with contributions from Amazon, Facebook, Google, IBM, Intel, Microsoft, and others. It was well-intentioned and genuinely useful. It did not solve the structural problem. It solved the immediate crisis for OpenSSL specifically, while the same vulnerabilities of understaffing, underfunding, and single-point-of-failure maintenance persisted across hundreds of other critical components.

**Log4Shell (2021)** demonstrated what happens when the affected component is not well-known outside engineering circles. Log4j is a logging library for Java applications — a foundational component present, at the time of disclosure, in an estimated 88% of organisations using Java-based systems. It was not a component most executives or risk managers had heard of. Its maintaining team was small, volunteer-driven, and entirely unprepared for the scale of what followed when a critical remote code execution vulnerability (CVSS score: 10/10 — the maximum possible severity) was publicly disclosed in December 2021.

The immediate consequence was not exploitation. The immediate consequence was that organisations around the world could not answer the question: do we use this? The hidden dependency problem crystallised in real time. One US federal agency reported spending 33,000 hours on Log4j vulnerability response alone. The DHS Cyber Safety Review Board concluded that full remediation would take a decade. As of December 2024, 12% of Java applications were still running vulnerable versions. The average incident response cost for a Log4Shell engagement exceeded $90,000; in the thousands of cases where it led to ransomware deployment, the costs were orders of magnitude higher.

**XZ Utils (2024)** changed the nature of the threat in a way that demands specific attention. XZ Utils is a data compression library present on virtually every Linux distribution. It is not a library most engineers think about — it is simply there, foundational, assumed to be maintained. Its single active maintainer, Lasse Collin, had been showing signs of burnout publicly on the project mailing list for some time.

In early 2024, a contributor named "Jia Tan" who had spent approximately three years building trust with the XZ Utils community — providing quality patches, offering code review, gradually earning commit access — shipped a sophisticated, carefully obfuscated backdoor into the library. The backdoor, if it had reached production in major Linux distributions, would have compromised SSH authentication on most Linux servers in the world. It was discovered almost by accident, by a Microsoft engineer who noticed anomalous CPU usage in a benchmarking test.

The technical sophistication of the attack was significant. The social engineering sophistication was arguably more so. The attacker did not exploit a software vulnerability. They exploited a human vulnerability: a burned-out, unsupported maintainer, isolated in a critical role, who was grateful for help. The incident confirmed what security researchers had been saying for years: nation-state actors have identified the maintainer burnout problem and are treating it as an attack surface.

**libxml2 (2025)** offered a preview of what an unmanaged availability failure looks like. libxml2 is the XML parsing library used by virtually every Linux distribution, every major browser's HTML engine, and embedded across billions of devices. In mid-2025, its sole maintainer announced he would no longer honour security embargoes — the standard practice of holding vulnerability details private while a patch is prepared and distributed — because the unpaid coordination burden had become unsustainable. In his own words, the project should be treated as software "maintained by a single volunteer, badly tested, written in a memory-unsafe language and full of security bugs." By September 2025 he had stepped down entirely, declaring the project "more or less unmaintained for now." New maintainers eventually stepped forward, but for a period, a component embedded in billions of devices had no active maintainer. The near-miss received brief coverage and was then forgotten.

**Ingress NGINX (2025–2026)** crossed the line from near-miss to actual retirement. The community-maintained Kubernetes ingress controller — used in approximately half of all cloud-native environments according to Kubernetes project data — was maintained for years by one to two developers working evenings and weekends. In November 2025, the Kubernetes Security Response Committee and SIG Network announced formal retirement effective March 2026: no further releases, no bug fixes, no security patches. The announcement was explicit: organisations continuing to use it after that date would be running unpatched infrastructure with no upstream remediation path. This was not a vulnerability event. It was not a nation-state attack. It was a component used by half the cloud-native ecosystem ceasing to exist as maintained software because its volunteer maintainers burned out and no organisation that depended on it found it worth funding.

The pattern across these incidents is consistent and worsening: small teams, inadequate support, increasing blast radius with each cycle. Heartbleed cost hundreds of millions. Log4Shell cost billions. XZ Utils nearly compromised a foundational authentication layer across internet infrastructure. The 2025 cohort — libxml2, Ingress NGINX, and others receiving less coverage — demonstrates that the pace is accelerating, not slowing.

**The AI acceleration problem** has crossed a threshold in early 2026 that makes previous projections look conservative. The primary maintainer of curl reported in April 2026 that security report submission rates had more than doubled year-over-year and projected approximately fifty curl CVEs in 2026 against a historical baseline of roughly sixteen per year — a pattern he confirmed across dozens of other major projects. That data predates the Mythos findings by days.

In April 2026, Anthropic and Mozilla jointly disclosed that Anthropic's Claude Mythos Preview — a frontier model currently restricted to a small group of vetted organisations — had identified 271 vulnerabilities in Firefox in a single evaluation. For context: Mozilla patched approximately 73 high-severity Firefox vulnerabilities in all of 2025. The preceding generation of the same model found 22 vulnerabilities in a two-week engagement with the same codebase. Mythos found 271. Mozilla's CTO described his team's reaction as "vertigo." Palo Alto Networks, another Mythos preview participant, reported that the model accomplished the equivalent of a year's worth of penetration testing in under three weeks.

Both organisations have framed this as a net positive for defenders: if AI can find vulnerabilities at this scale and speed, defenders who deploy it proactively can patch before attackers exploit. That argument is valid for software systems that can patch in days or weeks.

It does not apply to operational technology. An automotive OEM whose infotainment firmware contains a vulnerable version of libcurl cannot patch its deployed fleet in days. A railway signalling system certified to SIL4 cannot receive a software update without recertification — a process measured in months to years and costs measured in tens of millions of euros. A medical device manufacturer cannot push a patch without an FDA submission process. If AI tools can now enumerate the full vulnerability surface of a codebase in a single pass, and the organisations whose products embed that codebase require years to respond, the "defenders first" window is irrelevant. The vulnerabilities exist in deployed hardware regardless of who found them first.

The asymmetry is now structural and quantified: finding vulnerabilities is cheap and getting cheaper; fixing them in OT environments still requires expert human time, deep project knowledge, and regulatory approval processes measured in years. Under-resourced maintainers who were already struggling with doubled report rates now face the prospect of AI-generated vulnerability queues that no small team can clear. This will drive the next generation of maintainer exits.

The next major incident will be in hardware. In automotive firmware, where a vulnerability in an embedded library requires a recall. In a medical device, where a patch requires an FDA submission. In a railway interlocking system, where "patch this week" is simply not a concept that the operational model accommodates.

### 1.4 The OT Dimension: Security and Availability at Long Timescales

The incidents described above were serious. They affected enterprise IT systems, web servers, cloud infrastructure. Costly, disruptive, in some cases irreversible — but patchable, in principle, within a reasonable timeframe. The affected systems could be taken offline, updated, brought back online. In the worst cases, incident response teams were engaged, systems were rebuilt, operations were disrupted for days or weeks.

Operational technology is different in kind, not just in degree.

An OT system is one that monitors or controls physical processes: a railway interlocking, an infusion pump, an automotive brake control module, an industrial controller managing chemical process temperatures. These systems are characterised by long deployment lifecycles — often twenty to thirty years — limited or no ability to patch in service, and approval or certification processes that make software updates costly and time-consuming regardless of the reason for the update.

The patching model that makes IT security tractable — discover vulnerability, test patch, deploy patch, verify — does not apply in OT. A railway signalling system may be certified to SIL4 (the highest safety integrity level under the CENELEC railway standards — EN 50126 for RAMS, EN 50716:2023 for software, EN 50129 for safety-related electronic systems — derived from the generic IEC 61508 framework) and remain in service for twenty to thirty years. A software update to a SIL4-certified system requires re-certification: a process that takes months to years and costs tens of millions of euros. The practical consequence is that OT systems operate on component versions that IT teams would consider dangerously obsolete, and they are correct to do so — the alternative is either non-certification or a years-long certification process for every patch cycle.

Medical devices face analogous constraints under FDA regulation. A software update to a Class III device requires a 510(k) submission or PMA supplement. Automotive systems are subject to type approval processes that make ad-hoc software updates impractical across a deployed fleet. Industrial control systems in chemical plants, power generation, and water treatment are governed by IEC 62443, which recognises the patch-resistance of OT and calls instead for compensating controls: network segmentation, monitoring, anomaly detection.

These compensating controls are the right answer for the security dimension. Hardening the IT/OT barrier — segmenting OT networks, monitoring traffic, establishing unidirectional data flows where feasible — is the established and correct approach to managing exposure when a component cannot be patched. A vulnerability in a library embedded in a SIL4-certified interlocking is a security and network architecture problem, not a safety certification problem. Safety engineers in this domain already know not to trust third-party library code — SIL4 development methodology requires verifying or replacing components precisely because you cannot assume anything external is trustworthy. That discipline is well-established.

The gap that is not addressed by zone boundary hardening is **availability**. Security controls can manage the risk that a known-vulnerable component is exploited. They cannot manage the risk that the component stops being maintained altogether. If OpenSSL loses its remaining core developers, if libcurl loses Stenberg, if the Samba committer base ages out without succession — the organisations depending on those components lose a supplier. There is no patch for "the project no longer exists." There is no zone boundary between your product and the fact that a critical dependency has reached end of effective life with no replacement.

This is the dimension that is genuinely unmanaged in OT environments. An automotive OEM can segment its network to limit exposure to a vulnerable version of libcurl. It cannot segment away the fact that in five years, there may be nobody maintaining it. A medical device manufacturer can document that its network architecture compensates for an unpatched OpenSSL version. It cannot document a compensating control for the permanent unavailability of the component. Rail operators and their system suppliers have decades-long product lifecycles for systems that today depend on components whose organisational continuity is not guaranteed beyond the next maintainer burnout event.

The more important point for the business risk analysis is not security — it is availability and continuity. The organisations deploying these systems have taken on a long-term obligation to maintain and support products that depend on a supply chain they have not mapped and whose health they do not monitor. In most cases, they could not answer the question "what happens to our product if this component stops being actively maintained" — because they do not have a complete inventory of what components are in their products, let alone a view of each component's organisational health.

---

## Part 2: Why Current Efforts Are Not Enough

### 2.1 The Funding Initiatives

The problem described in this paper is not invisible to the people closest to it. The open source security community, several governments, and some large technology companies have funded responses. These responses are genuine efforts, run by capable people, and they have made a real difference. They are also structurally insufficient.

The **Open Source Security Foundation (OpenSSF)**, established in 2020 under the Linux Foundation, is the most comprehensive current attempt to address open source security systematically. It funds projects, produces frameworks, runs training programmes, and maintains tools like the Criticality Score, which algorithmically ranks open source projects by their dependency centrality across the software ecosystem. It is a serious organisation with competent staff.

Its limitations are structural. OpenSSF is governed by a board dominated by large technology companies: Google, Microsoft, Amazon, Intel, IBM, and others. These companies are simultaneously the largest beneficiaries of the open source infrastructure they are asked to fund and the organisations with the largest downstream exposure when that infrastructure fails. The resulting governance dynamic is predictable: decisions tend to be slow, consensus-driven, and calibrated to avoid actions that would embarrass or create liability for member organisations. More practically, OpenSSF operates almost entirely within the engineering community. Its outputs are frameworks, scorecards, and technical guidance. They rarely reach the people who control capital allocation: CFOs, board risk committees, insurance underwriters.

The **German Sovereign Tech Fund**, established in 2022, provides government grants to critical open source projects. Its model is correct in principle: direct, multi-year funding for specific projects based on their criticality to digital infrastructure. The Samba project's €688,000 funding round is a representative example. The limitations are scale (annual budgets of €13 million in 2022 and €22 million in 2023 — significant for a single government programme, insufficient relative to the global dependency), geography (Germany and to some extent the EU), and duration (grants rather than sustainable ongoing funding structures). It cannot compel action, it cannot move at private-sector speed, and it cannot address the availability and continuity risk in OT-dependent industries where the stakes are highest.

The **Core Infrastructure Initiative (CII)**, now absorbed into OpenSSF, was established in direct response to Heartbleed. It funded critical projects including OpenSSL, OpenSSH, and the Linux kernel. It was a meaningful intervention. It was also reactive: it addressed a specific crisis rather than building a systematic capability to identify and prevent the next one.

These initiatives share a common failure mode: they operate within the engineering community, on engineering timescales, speaking engineering language. The people who control the capital required to fix the problem structurally — at manufacturers, at insurers, at investment firms — are not in the room.

### 2.2 Regulation: Right Direction, Wrong Instrument

Regulation is arriving. The EU Cyber Resilience Act creates mandatory obligations for manufacturers to know their software supply chains, report exploited vulnerabilities within 24 hours, and maintain products securely across their lifecycle. The FDA's updated software guidance requires software bills of materials for medical devices. The SEC's cyber disclosure rules require public companies to describe their cybersecurity risk management. US federal procurement has required SBOMs since 2021.

The direction is correct. The obligation to know what software is in your product, and to manage it responsibly, is the right regulatory intervention. The problem is execution.

The CRA's full enforcement does not arrive until December 2027. Its OT applicability is limited — vehicles, medical devices, and aviation systems are explicitly excluded from the CRA's scope because they have their own sectoral regulation. Those sectoral regulations do not contain equivalent SBOM and vulnerability management requirements. The gap between the CRA's intent and the sectors where the highest-stakes OT systems live is not a rounding error — it covers automotive, rail, aviation, and most medical devices.

Even within the CRA's scope, the regulation creates a compliance obligation, not a sustainability solution. A manufacturer can comply with the CRA's SBOM requirement by generating a list of components they depend on. That list may show that they depend on an unmaintained component with known vulnerabilities. The CRA requires them to document this. It does not provide a mechanism to fund or fix the underlying component. The compliance checkbox is documented evidence of a known risk, not a resolution of it.

Regulation creates the forcing function. It does not provide the solution. The solution requires a different kind of entity.

### 2.3 The Missing Piece: Organisational Infrastructure

The open source community has engineers. In many cases, extraordinary engineers — people who have maintained complex, widely deployed systems for decades with minimal support, at a level of quality that would be difficult to replicate under any funding model. The problem is not a shortage of engineering talent.

What the open source world systematically lacks is the operational discipline that makes large-scale, long-duration engineering projects sustainable:

**Product managers** who own roadmaps, prioritise ruthlessly, say no to scope creep, and translate between technical reality and stakeholder expectations. Successful open source projects usually have someone playing this role informally. Most do not.

**Systems architects** who can look at a twenty-year-old codebase and design the path from where it is to where it needs to be — not by accumulating patches on existing foundations, but by thinking clearly about what needs to be preserved, what can be discarded, and what needs to be rebuilt.

**Programme managers** who keep work honest. Who track dependencies, identify blockers, ask uncomfortable questions about timelines, and ensure that the plan for next year is not simply this year's plan plus optimism.

**Governance structures** that provide accountability without bureaucracy — that can accept funding, make decisions, and execute without requiring consensus from a mailing list of several hundred interested parties.

The projects that demonstrate it is possible — SQLite, being the most frequently cited example — are successful not because they have more money, but because they have more operational structure. SQLite has a small, professional team, a clearly defined scope, comprehensive documentation, and a governance model that allows fast decisions. SQLite is, according to its own creators and independent researchers, the most widely deployed database engine in the world — present in every iOS and Android device, every major web browser, and billions of other installations. The constraint was never money alone. It was structure applied to engineering effort.

---

## Part 3: The Proposed Solution

### 3.1 The Map

The first deliverable is the most important, and it is also the most underestimated. A public, maintained, machine-readable dependency map of critical open source infrastructure components.

Not a survey. Not a report. A living data asset: continuously updated, authoritative, cited in regulatory filings and insurance assessments, and accessible to the engineers inside manufacturing organisations who need something external to point at when they escalate internally.

What the map contains, for each critical component:

- **Component identity:** name, version history, repository, licence
- **Maintainers:** named individuals, their employers (where known), their tenure, their other project commitments, and a real bus factor — not a theoretical "how many contributors" number, but an assessment of what actually breaks if two specific people are unavailable for six months
- **Dependency graph:** what depends on this component, recursively, including known hardware and firmware embeddings where SBOM data exists
- **Health indicators:** commit frequency trend over 36 months, contributor count trend, time-to-patch for reported vulnerabilities, test coverage, documentation quality, last independent security audit date
- **Risk score:** a composite assessment that weights the bus factor, the dependency breadth, the criticality of dependent systems, and the language/ecosystem risk (C codebase with no modern alternative = higher risk than a well-resourced Python library)
- **Funding status:** known current funding sources, amounts where public, funding horizon

Why public? Because the primary use case is not the map itself — it is what the map enables. Engineers inside Toyota, Bosch, Siemens, Medtronic already understand that some of their critical dependencies are fragile. What they lack is an external, authoritative reference they can point at when escalating internally. An internal engineer saying "I think this library might be a problem" is a concern. The same engineer pointing at an independent public risk assessment that scores their dependency at critical risk — and that their insurer's underwriting team has also seen — is a supply chain risk that demands a board response.

Naming is not a shaming exercise. It is the minimum condition for accountability. "One maintainer" is an abstraction. "Daniel Stenberg, the primary maintainer of libcurl since 1996, whose library is present in your vehicle infotainment firmware" is a named supplier relationship that any responsible supply chain manager would be required to assess.

The map does not start from scratch. The OpenSSF Criticality Score provides an algorithmic baseline. SBOM tooling in formats like SPDX and CycloneDX is increasingly mature. The US Executive Order on cybersecurity and EU CRA compliance requirements are beginning to generate SBOM data at scale. The raw material exists. The missing element is an entity that integrates it, validates it, adds the human-intelligence layer (who actually maintains this — not according to the git log, but in practice), and maintains it as a continuously updated, trusted data asset rather than a point-in-time research report.

### 3.2 The Certification Framework

The second deliverable is a sustainability certification for open source components — analogous in form and function to CE marking, UL listing, or ISO 9001, but specifically designed for the sustainability and security of open source infrastructure.

A certified component has been assessed against a defined framework and found to meet minimum standards for:

- **Maintainer continuity:** bus factor above a defined threshold; succession planning documented; key knowledge not resident in a single person's head
- **Contributor pipeline:** new contributors being onboarded; contributor base not declining year-over-year
- **Security practices:** regular security audits by independent parties; a responsible disclosure process; a documented patch timeline; test coverage above a defined threshold
- **Documentation quality:** sufficient for a competent engineer to understand the component's behaviour without access to the original authors
- **Governance:** a decision-making structure that can accept funding, make commitments, and be held accountable; not a single maintainer operating informally
- **Funding stability:** sufficient funding to maintain the current level of activity for a defined horizon

The certification mark creates a market incentive that currently does not exist. A manufacturer documenting their CRA compliance, or responding to an FDA software review, or filling out a cyber insurance application, can reference certified components as evidence of due diligence. The mark, once accepted as a reference point by regulators and underwriters, becomes a requirement rather than a differentiator. Projects that do not have it face questions from the manufacturers who depend on them. Manufacturers who depend on uncertified components face questions from their insurers and regulators.

The certification framework is not purely a carrot. A component's absence from the certified list, or its failure to maintain certification, is itself information that the map makes visible. The combination of public risk assessment and certification status creates a complete risk picture: this component is critical, it is not certified, and here is why.

### 3.3 The Intervention Model

The map identifies risk. The certification framework measures it. When a critical component scores high on the risk assessment and fails to achieve certification, something needs to actually happen to the component. That is the third deliverable: a structured intervention capability.

The model is not donation. It is not grant funding. It is an operational intervention, designed to produce a specific outcome on a defined timeline: a component that is sustainable, maintainable, and no longer dependent on the heroics of one or two individuals.

When a critical project is identified as at risk, a structured intervention team is assembled. The team is small and specifically composed:

- A **systems architect** who can assess the current codebase, understand its dependencies and constraints, and design a credible path to a modern, maintainable implementation
- **Senior engineers** — typically three to five — with the relevant domain expertise to execute that design
- A **product manager** who owns the outcome, not the process. The product manager's job is to define what "done" looks like, ruthlessly prioritise the roadmap, manage the relationship with the existing maintainer community, and ensure the project doesn't drift into scope creep or academic perfectionism. This role is non-negotiable; it is also the role most frequently absent from struggling open source projects
- A **programme manager** who tracks progress, identifies blockers, and keeps the work honest

The prioritisation framework for a rewrite follows a pragmatic commercial logic: address the platforms and use cases with the largest current installed base and the highest security sensitivity first. For a component like Samba, this means modern Windows Server and Windows 10/11 interoperability first — the cases where enterprise and government systems depend on it today. Legacy platform support comes second, served by the existing codebase on a maintenance-only basis until the new implementation can address those cases. Truly ancient configurations continue to be served by the old codebase until they are decommissioned — the rewrite is not a forcing function for customer migration on a timeline that doesn't suit them.

The case for rewriting rather than refactoring is specific and important. A rewrite in a modern language — Rust being the current most credible choice for systems-level components due to its memory safety properties and growing ecosystem — means a codebase that is accessible to a much larger pool of engineers. The aging C codebase of many critical components is not just a security risk; it is a recruitment problem. The supply of engineers who are productive in C, who understand the subtleties of manual memory management, and who are willing to work on thirty-year-old systems is declining. A modern implementation is a bet on the next generation of maintainers.

The hardest part of any such rewrite is not implementing the specification. For most critical infrastructure components, the specification is documented — in the case of SMB, published under the 2007 EU antitrust judgment against Microsoft. The hard part is the thirty years of undocumented behaviour: the edge cases, the client quirks, the deviations from the standard that real implementations tolerate because real clients depend on them. This knowledge lives in test suites and in the memories of aging engineers. Extracting, documenting, and preserving it is the actual engineering challenge. It is also exactly the kind of work that a well-resourced, structured team with a capable product manager can execute — given time, money, and the right access to the current maintainer community.

### 3.4 The C-Suite Conversation

The three capabilities above — the map, the certification, the intervention — are operationally useful and technically credible. They address the engineering reality. They will not, by themselves, change the behaviour of the manufacturers whose products carry the exposure. For that, a different conversation is required.

The conversation with a CFO or board risk committee is not a technical briefing. It does not start with CVE numbers or bus factors. It starts with a supply chain risk framing: "You have a critical supplier relationship that you have not assessed, that you have no SLA with, and that represents a single point of failure in your product. Here is what that supplier looks like. Here is what happens to your product line if that supplier is unavailable for six months. Here is what your insurer is about to ask you about this at renewal."

This conversation is most effectively initiated bottom-up. Engineers inside manufacturing organisations who are aware of the risk — and in most large organisations, there are engineers who know exactly which critical components are fragile — need an external reference to escalate with credibility. The public map provides that reference. An engineer pointing at an independent risk score, published by a credible third party, that is also visible to their insurance underwriter and their regulatory authority, is operating with a qualitatively different kind of leverage than one expressing a personal concern.

The insurance lever is particularly powerful and is currently underutilised. Cyber insurance underwriters are moving toward more granular software supply chain risk assessment. As the data product matures and becomes referenced in underwriting decisions, the cost of carrying undisclosed dependency risk — in the form of higher premiums, exclusions, or coverage limits — becomes a direct financial consequence that reaches the CFO without requiring an engineering briefing.

The regulatory lever operates on a slightly longer timeline but is already in motion. CRA compliance requires knowing your dependencies. FDA software guidance requires SBOMs. SEC disclosure rules require describing material cyber risks. The manufacturer who cannot demonstrate that they assessed and managed their open source dependency risk faces regulatory exposure that is, for regulated industries, a board-level concern.

---

## Part 4: The Business Model

### 4.1 Why This Is a Business, Not a Foundation

The problem described in this paper has generated a string of foundation-type responses: the Core Infrastructure Initiative, the OpenSSF, the Sovereign Tech Fund. These organisations have done real work and have genuine merit. They are also, structurally, poorly suited to execute the kind of intervention described in Part 3.

Foundations are consensus-driven. Their governance structures are designed to prevent any single participant from dominating — which, in the specific context of OpenSSF, means that the large technology companies whose commercial interests shape the agenda cannot be overruled by technical judgment. Foundations are also episodic: they respond to crises, fund projects for a year or two, and move on. They do not build the kind of institutional knowledge and operational capability that comes from doing the same kind of work repeatedly over years.

A business has a profit and loss statement. It has accountability. It has the incentive to execute rather than to deliberate. It can hire the right people, pay them at market rates, and let them go if they do not perform. It can make decisions quickly because it does not require the consensus of a multi-stakeholder board.

The trust challenge is real. An entity that publicly assesses the sustainability of open source components will be seen as having conflicts of interest if it is simultaneously billing the manufacturers who depend on those components. The governance model needs to be designed to manage this tension explicitly: an independent assessment function, a clear separation between the data and certification business and the consulting and intervention business, and a board composition that excludes product manufacturers from the assessment and certification governance.

This is a solvable problem. Rating agencies operate under similar structural tensions — they assess the creditworthiness of issuers who pay for the ratings — and while the model has well-documented failure modes, it has also generated durable, valuable institutions. The key design principle is that the data product's value depends on its perceived independence. The business model must protect that independence as a structural requirement, not as an afterthought.

### 4.2 Revenue Streams

The business model is built on multiple independent revenue streams, each addressing a different buyer and serving a different function in the system.

**Data subscriptions** are the core and highest-leverage revenue stream. The dependency map, continuously maintained and authoritative, is valuable to cyber insurance underwriters who need it to price risk, to private equity and M&A due diligence teams who need it to assess the software supply chain risk of acquisition targets, and to compliance teams at manufacturers who need it to evidence their regulatory obligations under the CRA and equivalent frameworks. These buyers have high willingness to pay for accurate, maintained risk data — the alternative is building the capability internally at significantly higher cost. Subscription revenue is recurring and scales with the breadth and depth of the map rather than with the number of consulting engagements.

**Certification fees** are paid by projects seeking certification and by the manufacturers whose compliance documentation benefits from referencing certified components. Annual renewal creates recurring revenue. As the certification mark gains regulatory and insurance acceptance, the fee becomes a line item in a compliance budget rather than a discretionary expenditure — which is to say, it becomes non-discretionary.

**Intervention consulting** is the highest-value and highest-margin engagement, but also the least scalable. A structured intervention into a critical at-risk project — the team of architects, engineers, and programme managers described in Part 3 — represents a multi-year engagement at a price point that reflects the genuine scarcity of the required expertise. This revenue stream is not the primary growth driver. It is the proof of concept that demonstrates the model works and generates the institutional knowledge that makes future interventions cheaper and more effective.

**Training and tooling** — helping organisations integrate SBOM practices, map their own dependencies, and implement the operational processes needed for CRA compliance — is a volume play that opens doors into manufacturing organisations and creates the bottom-up awareness that drives the C-suite conversations.

### 4.3 The Moat

The defensible advantage in this business is not a technology. It is trust, combined with data quality, combined with network effects.

The dependency map, once it is authoritative and trusted, becomes self-reinforcing. Organisations that use it to manage their own compliance contribute data back — SBOM disclosures, incident reports, assessment findings. More users means more data; more data means higher accuracy; higher accuracy means more users. The map that the second entrant in this market has to build is a map that starts from zero and has to compete with one that is already cited in regulatory filings and insurance underwriting decisions. That is a very difficult competitive position to overcome.

The certification mark operates similarly. Once it is referenced in a CRA compliance filing or an FDA software review, it becomes a standard. Standards are not easily displaced. The UL listing and CE marking have remained dominant in their domains for decades not because there is no alternative, but because their value is precisely in their ubiquity — the fact that everyone recognises them is the product.

The institutional knowledge accumulated through interventions is the deepest moat. The understanding of how a thirty-year-old C codebase works, what its edge cases are, which parts of the specification are implemented correctly and which are implemented "as the clients actually behave" — this knowledge does not exist in any document. It lives in the heads of a small number of engineers and in the test suites they have written. An entity that has executed several major interventions has access to that knowledge, both directly (through the engineers involved) and indirectly (through the documented outcomes of the rewrite process). That is genuinely irreplaceable.

---

## Part 5: What Needs to Happen Now

### 5.1 The Immediate Priorities

The path from concept to operational capability is sequenced. The first step does not require the full organisation — it requires a credible minimum viable version of the map, executed well enough to be taken seriously by the first wave of users.

**Build the MVP map.** Start with fifty components — the most widely deployed, the most clearly critical, the most verifiable in terms of maintainer data and dependency breadth. For each component, produce the full data set described in Part 3.1. Publish it. Make it citable. The first version does not need to be comprehensive; it needs to be accurate. One wrong data point that a manufacturer can challenge undermines the entire credibility model. Get the first fifty right.

**Execute three pilot assessments.** Apply the certification framework to three projects — chosen to represent different risk profiles, not three projects that will pass comfortably. The pilot that fails certification is as important as the ones that pass, because it demonstrates that the assessment is real and not a box-checking exercise.

**Initiate three pilot C-suite conversations.** One automotive OEM, one medical device manufacturer, one industrial equipment manufacturer. Not a sales call. A demonstration: "Here is your dependency profile as we currently understand it. Here is the risk score on your three most critical unmaintained components. Here is what your insurer is beginning to ask about this. Can we talk about what you want to do with this information?"

**Find the missing team members.** The founding capability needs three things that are hard to combine in one person: deep open source community standing (credibility with the maintainers who will need to trust the intervention model), relationships in the financial and insurance industry (to access the data subscription buyers and drive the insurance lever), and operational execution capability (to build the map and run the assessments). The combination of these in the founding team is the most important single factor in whether this succeeds or not.

### 5.2 A Call to Specific Actors

**To manufacturers:** Your engineers already know which of your critical dependencies are fragile. Most of them have raised it at some point and been told it is not a priority. Give them the mandate and the budget to map your actual dependency risk before your insurer or your regulator does it for you. The answer will be uncomfortable. The alternative is discovering it during an incident.

**To insurers:** The data you need to price software supply chain risk accurately does not exist at the scale or quality required. You are currently underwriting blind in a part of the risk landscape that is growing. The map described in this paper is the data infrastructure that changes that. Fund its creation, or accept that you will continue pricing this risk on assumptions rather than evidence.

**To governments:** The Sovereign Tech Fund model demonstrates that direct government investment in critical open source infrastructure is feasible and politically achievable. It needs to be ten times larger and international in scope. The EU, working with G7 partners, has the mechanism and the mandate to make this happen. The CRA creates the regulatory framework; the funding infrastructure needs to match it.

**To open source maintainers:** The problem you are experiencing is structural, not personal. The burnout, the isolation, the sense that you are carrying a responsibility that nobody else will acknowledge — these are not individual failures. They are the predictable consequences of a funding and governance model that has never been fit for the criticality of the software it produces. There is a path to sustainability that does not require martyrdom. It requires the same thing as any other critical infrastructure: the right organisational model, adequate resources, and someone with the standing to tell the people who depend on your work that they need to pay for it.

### 5.3 The Closing Argument

Rail safety certification exists because trains crashed until it did. The rail industry — one of the first industries to operate at scale with life-safety implications — developed safety integrity levels, formal methods, independent certification, and mandatory audit processes not because engineers thought it would be intellectually satisfying, but because regulators and courts demanded accountability after disasters.

The software infrastructure world has not had its train crash yet — not at a scale that cannot be explained away or absorbed by emergency response. XZ Utils came close. The next attempt will be better resourced and better planned, because the organisations executing these attacks learn from the ones that were discovered. The question is whether we build the accountability infrastructure before or after the crash that makes it unavoidable.

This is not a charity problem. The infrastructure described in this paper underpins products generating trillions of dollars of annual revenue. The economics of fixing it are straightforwardly positive: the cost of a serious, adequately funded intervention into a critical at-risk component is measured in the tens of millions of euros. The cost of that component failing in a way that propagates through the products that depend on it is measured in billions — in recalls, in regulatory enforcement, in litigation, in reputational damage that takes years to repair.

The tools exist. The regulatory pressure is arriving. The technical understanding is there. The timing, for the first time, is right. What is missing is the entity that assembles these elements — that speaks credibly to engineers and to boards, that operates with the independence required to be trusted and the commercial discipline required to execute, and that treats critical open source infrastructure with the same seriousness as any other critical infrastructure.

That entity does not yet exist. It needs to.

---

*The author is developing a structured initiative to address this systematically. For collaboration, partnership enquiries, or further discussion, please contact: contact@criticalsoftwareriskregister.com.*

---

## Appendices

### Appendix A: Methodology — How the Risk Map Is Constructed and Maintained

The dependency map combines three data layers, each requiring different methods of collection and validation.

**Layer 1: Algorithmic baseline.** The OpenSSF Criticality Score provides a quantitative starting point, based on factors including contributor count, commit frequency, dependent project count, and release history. This layer is largely automatable and updates continuously from public repository data.

**Layer 2: Dependency graph.** The graph of what depends on what is constructed from a combination of package manager dependency data (npm, PyPI, Maven, cargo, apt, and others), published SBOM data (in SPDX and CycloneDX formats) from manufacturers and system integrators who have made it available, and known hardware/firmware embeddings documented in vendor disclosures, CVE databases, and research publications. The OT layer — what is embedded in vehicle firmware, medical device software, industrial controllers — is the least complete and the most important. It is built incrementally from disclosure data, research, and direct engagement with manufacturers.

**Layer 3: Human intelligence.** Maintainer data — who actually maintains a project in practice, what their bus factor is, what their employment and financial situation is, what their succession plans are — cannot be derived algorithmically. It requires human research: reading mailing lists, interviewing contributors, reviewing financial disclosures where available, and in some cases direct engagement with the maintainers themselves. This layer is the differentiator that makes the map credible rather than merely large.

Data is validated against multiple sources before publication. Any data point about a named individual is verified directly where possible. Update frequency varies by layer: the algorithmic layer updates continuously; the dependency graph updates quarterly; the human intelligence layer updates annually or upon trigger events.

---

### Appendix B: Case Studies — Detailed Timeline and Cost Analysis

**Heartbleed (CVE-2014-0160)**

- Vulnerability introduced: late 2011, as part of the TLS heartbeat extension implementation
- Present in codebase undetected: approximately 2 years
- Discovery: independently by Google's Neel Mehta and Finnish firm Codenomicon, March–April 2014
- Public disclosure: April 7, 2014
- Scope at disclosure: approximately 17.5% of all SSL/TLS-secured public web servers; effectively all systems running OpenSSL versions 1.0.1 through 1.0.1f
- Nature of attack: buffer over-read allowing extraction of arbitrary server memory contents (including private keys, session tokens, credentials) without authentication, without leaving a log trace
- Maintainer status at disclosure: one full-time developer on the OpenSSL project; a small number of part-time contributors
- Estimated remediation cost: $500 million (certificate revocation and reissuance alone); actual total cost substantially higher
- Annual donations to OpenSSL in the year before disclosure: $2,000
- Immediate response: Core Infrastructure Initiative established by the Linux Foundation; $5.5 million raised from 14 technology companies

The Heartbleed incident established the template for every subsequent open source infrastructure crisis: a critical component, maintained by a tiny team, with a vulnerability present for years before discovery, generating remediation costs orders of magnitude larger than the annual investment required to have prevented it.

---

**Log4Shell (CVE-2021-44228)**

- Vulnerability: remote code execution via JNDI injection in the log message parsing path of Apache Log4j 2.x
- CVSS score: 10.0 (maximum)
- Disclosure: December 9–10, 2021
- Exploitation in the wild: documented from December 1, 2021, nine days before public disclosure
- Scope: Log4j estimated present in 88% of organisations using Java-based systems; used in enterprise software, cloud services, industrial control systems, and OT environments
- Maintainer status: volunteer-maintained Apache project; core team unprepared for crisis-scale response
- Immediate consequence: organisations worldwide unable to answer "do we use this" for weeks; emergency scanning programmes launched across global enterprises
- One US federal agency: 33,000 hours spent on vulnerability response alone
- DHS Cyber Safety Review Board assessment: full remediation will take a decade
- Average incident response cost where exploitation occurred: >$90,000 per engagement
- As of December 2024: 12% of Java applications still running vulnerable Log4j versions

The Log4Shell incident demonstrated the hidden dependency problem at full scale. The question was not whether organisations were vulnerable — most were — but whether they knew it. Most did not.

---

**XZ Utils backdoor (2024)**

- Component: XZ Utils, a data compression library present in virtually all Linux distributions
- Maintainer status: single primary maintainer (Lasse Collin), operating in isolation, with publicly documented signs of burnout
- Attack method: social engineering over approximately three years
- Attack actor: identified as "Jia Tan," attributed by multiple security researchers to a nation-state actor
- Method of operation: "Jia Tan" introduced themselves to the project as a new contributor in 2021, provided quality patches over eighteen months, gradually earned maintainer trust and partial commit access, inserted a sophisticated multi-stage backdoor into the build process
- Target: SSH daemon on systems using the backdoored library; successful exploitation would have allowed authentication bypass on most Linux servers
- Discovery: March 2024, by Microsoft engineer Andres Freund, who noticed anomalous CPU usage and SSH authentication delays during benchmarking — an accidental discovery, not a security audit
- Near-miss: the backdoored version had reached Debian unstable and Fedora testing builds before discovery; production release was days away

The XZ Utils incident represents a qualitative shift in the threat model. Previous incidents were the result of missed bugs and underfunded maintenance. XZ Utils was an intentional, patient, sophisticated attack that used maintainer isolation and burnout as the attack vector. The technical sophistication required was significant; the social engineering sophistication was greater.

---

### Appendix C: Regulatory Landscape

**EU Cyber Resilience Act (CRA)**
- In force: December 10, 2024
- Vulnerability reporting obligations: September 11, 2026
- Full enforcement: December 11, 2027
- Scope: manufacturers, importers, and distributors of products with digital elements sold in the EU
- Key obligations: secure-by-design requirements, SBOM for critical/important products, 24-hour reporting of actively exploited vulnerabilities, support period documentation
- OT exclusions: vehicles (covered by UNECE WP.29), medical devices (covered by MDR/IVDR), aviation (covered by EASA regulation), and rail (covered by ERA regulation)
- Penalties: up to €15 million or 2.5% of global annual turnover for critical failures

**FDA Software as a Medical Device Guidance**
- Current framework: FDA's 2023 guidance on cybersecurity for medical devices requires SBOM submissions with pre-market applications
- Scope: Class II and Class III medical devices with software components
- Ongoing requirements: post-market vulnerability monitoring, patch plans as part of device lifecycle management

**SEC Cyber Disclosure Rules (US)**
- In effect: December 2023
- Requirements: material cybersecurity incidents reported within four business days on Form 8-K; annual disclosure of cybersecurity risk management processes and governance on Form 10-K
- Relevance: supply chain software risk qualifies as material cyber risk requiring disclosure; creates board-level accountability for software dependency risk

**US Federal SBOM Requirements**
- Origin: Executive Order 14028 on Improving the Nation's Cybersecurity (May 2021)
- Current scope: federal software procurement; contractors providing software to US government agencies must provide SBOM
- Data availability: SBOM data generated under this requirement is the most consistent publicly available source of component dependency data for US-deployed software systems

---

### Appendix D: Existing Initiatives — What They Do and Where the Gaps Are

| Initiative | Founded | Funding model | Primary activity | Gap |
|---|---|---|---|---|
| OpenSSF | 2020 | Member fees (large tech companies) | Frameworks, tooling, training | Governance conflict; doesn't reach capital allocators; no OT domain credibility |
| Sovereign Tech Fund (Germany) | 2022 | Government grant | Direct project funding | EU-scoped; insufficient scale; episodic not structural |
| Core Infrastructure Initiative | 2014 (now OpenSSF) | Member fees | Direct project funding | Reactive; absorbed into OpenSSF |
| EU CRA | 2024 | Regulation | Compliance requirements | No OT applicability in highest-risk sectors; compliance ≠ sustainability |
| CISA (US) | — | Government | Vulnerability advisories; SBOM guidance | Advisory only; no funding mechanism; no intervention capability |

---

### Appendix E: Draft Certification Framework Criteria

The following criteria represent a starting point for the certification framework. They will be refined through consultation with the open source community, manufacturers, and insurance underwriters before the framework is formally published.

**Tier 1: Maintainer Continuity (minimum: 40% of total score)**
- Bus factor ≥ 3 (project can absorb the simultaneous loss of any two contributors without critical capability loss)
- Documented succession planning: named backups for all critical knowledge holders
- Key knowledge documented: architectural decisions, edge case behaviours, test rationale

**Tier 2: Security Practices (minimum: 30% of total score)**
- Independent security audit within the last 24 months
- Responsible disclosure process: documented, actively monitored, with defined response timelines
- Patch to release timeline for critical vulnerabilities: ≤ 30 days
- Test coverage ≥ defined threshold (component-specific; documented deviation requires justification)

**Tier 3: Governance and Sustainability (remaining 30%)**
- Governance structure: documented, includes financial accountability, not dependent on single individual
- Funding horizon: ≥ 24 months of current activity level funded
- Contributor pipeline: new contributor onboarding documented; contributor count not declining >20% year-over-year
- Documentation quality: assessed against defined standard for the component's complexity class

**Certification levels:**
- **Certified:** meets all minimums; annual renewal required
- **Conditional:** meets Tier 1 and Tier 2 minimums but gaps in Tier 3; 12-month remediation period
- **At Risk:** does not meet Tier 1 or Tier 2 minimums; public designation; eligible for intervention programme

---

### Appendix F: Glossary

**Bus factor** — the minimum number of team members whose simultaneous loss would make a project unable to function. A bus factor of 1 means a single person's unavailability — illness, burnout, death — halts the project. High bus factor is a fundamental sustainability requirement.

**SBOM (Software Bill of Materials)** — a structured inventory of all software components in a product, including their versions, licences, and known vulnerabilities. Analogous to an ingredient list for software. Required under EU CRA, FDA guidance, and US federal procurement rules.

**SIL (Safety Integrity Level)** — a measure of the required risk reduction for a safety function, defined in IEC 61508 (the generic functional safety standard). In rail specifically, SIL levels are applied through the CENELEC suite: EN 50126 (RAMS), EN 50716:2023 (software — superseding the withdrawn EN 50128 and EN 50657), and EN 50129 (safety-related electronic systems). SIL1 is the lowest, SIL4 the highest. SIL4 systems include railway interlockings, nuclear safety systems, and certain aviation and automotive functions. Achieving SIL4 certification requires extensive documentation, formal verification, and independent audit; maintaining it through software updates is costly and time-consuming.

**OT (Operational Technology)** — hardware and software that monitors or controls physical processes, as distinct from IT (information technology) that processes information. OT systems include industrial controllers, railway signalling, medical devices, automotive control systems, and building management systems. OT security differs from IT security primarily in that patchability is limited and physical consequences of failure are direct.

**CVE (Common Vulnerabilities and Exposures)** — the standard naming system for publicly disclosed software vulnerabilities. Each CVE has a unique identifier (e.g., CVE-2021-44228 for Log4Shell) and a severity score expressed as CVSS (Common Vulnerability Scoring System), ranging from 0 to 10.

**CVSS score** — the numerical severity rating of a vulnerability. Scores of 9.0–10.0 are "Critical"; 7.0–8.9 are "High." Both Heartbleed and Log4Shell received maximum (10.0) CVSS scores.

**Remote code execution (RCE)** — a class of vulnerability that allows an attacker to run arbitrary code on a target system over a network connection, without requiring physical access or prior authentication. Generally the highest-severity category of software vulnerability.

**Zero-day** — a vulnerability that is unknown to the software's maintainers and therefore has no patch available when it is first exploited. Log4Shell was exploited as a zero-day for nine days before public disclosure.

**OpenSSF Criticality Score** — an algorithm developed by the Open Source Security Foundation that ranks open source projects by their criticality to the software ecosystem, based on factors including dependent project count, contributor count, and activity metrics. Available as a public data set.

**Purdue model** — a hierarchical architecture for industrial control systems that separates operational technology from information technology through defined network boundaries. Widely used as a security reference architecture for OT environments. Named for the ISA-95 standard developed at Purdue University.

---

*This paper was written in April 2026. Data cited reflects publicly available information as of that date. The author has taken care to source claims accurately; corrections and additions are welcomed.*

---

[Impressum](./impressum.md) · [Datenschutz](./datenschutz.md) · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) · contact@criticalsoftwareriskregister.com
