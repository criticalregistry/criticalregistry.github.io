---
layout: default
title: Landscape
---

<!-- Last updated: 2026-05-15 -->

# Landscape

Several organisations are working on open source security and sustainability. Each addresses a real problem. The gaps that remain are not oversights: they follow from each organisation's governance, methodology, and buyer relationship. This page maps the terrain.

---

## Alpha-Omega

[Alpha-Omega](https://alpha-omega.dev) is a Linux Foundation project funded by Amazon, Google, and Microsoft, with an annual budget exceeding $7 million. It funds security improvements at critical open source organisations: staffing security roles, commissioning audits, hardening package manager infrastructure. Its 2024 grant recipients include the Python Software Foundation, the Rust Foundation, the Eclipse Foundation, the Linux Kernel project, and Node.js.

Work funded by Alpha-Omega reaches components that are also present in operational technology environments. Improvements to the Linux kernel benefit manufacturers running long-term-supported Linux in rail, power, and industrial control systems. That is real value.

The gaps are structural. Alpha-Omega's governance requires consensus among its three funders, whose commercial interest is cloud and developer infrastructure. Its methodology is built on package registry data covering npm, PyPI, RubyGems, cargo, and similar sources. C libraries distributed through apt, vendoring, and static linking do not appear in that data. The components embedded in vehicle firmware, medical devices, and railway signalling are invisible to a methodology whose entry filter is registry downloads or manifest edges. That is not a criticism of the methodology for what it is designed to do. It is a description of where it stops.

Alpha-Omega funds open source projects. It does not assess the exposure of the manufacturers depending on them. It does not produce named-maintainer bus factor analysis. It does not supply underwriting-grade risk data to insurers or PE due diligence teams. It does not route a risk assessment to a CFO.

---

## OpenSSF

The [Open Source Security Foundation](https://openssf.org) produces frameworks, scorecards, training, and tooling for the developer community. Its Criticality Score is one of the data sources the CSRR registry builds on. Its governance reflects the same structural dynamic as Alpha-Omega: large technology companies with conflicting interests set priorities, and output stays within the engineering community.

---

## ecosyste.ms

[ecosyste.ms](https://ecosyste.ms) is an open data infrastructure project indexing over 14 million packages across nearly 2,000 sources, including 16 major package managers. It is the most comprehensive publicly available dataset of package registry dependency relationships, covering npm, PyPI, RubyGems, Maven, cargo, and others. It recently added commercial licensing.

Its coverage boundary is the same as the methodology it supports: package registries. C libraries distributed through system package managers, static linking, and vendoring are outside its scope by design. It is the ceiling of what registry-based analysis can see. The CSRR registry uses it as one input into its algorithmic baseline layer, then builds beyond it.

---

## Civil Infrastructure Platform

The [Civil Infrastructure Platform](https://cip-project.org) is a Linux Foundation project maintaining a long-term-supported Linux kernel and core packages for civil infrastructure and industrial automation. Its members include Siemens, Hitachi, and Toshiba. Domains in scope include transportation, power generation, industrial control, and healthcare.

CIP and CSRR address different questions. CIP asks: how do we maintain a Linux platform that OT manufacturers can deploy for twenty years? CSRR asks: which components in those deployments are organisationally fragile, and what is the liability exposure of the manufacturers depending on them? The first is an engineering maintenance problem. The second is a risk assessment directed at a different audience.

---

## German Sovereign Tech Agency

The [Sovereign Tech Agency](https://www.sovereign.tech) administers the Sovereign Tech Fund, a German government programme providing direct grants to critical open source projects. Its model is correct in principle: sustained, multi-year funding for specific projects based on their dependency footprint. The Samba funding (€688,000 over 18 months, administered by SerNet) is a representative example.

The limitations are scale (annual budgets of €13 to €22 million against a global problem), geography (Germany, with some EU reach), and the absence of a commercial data product, a certification framework, or a manufacturer-facing risk assessment.

---

## EU Sovereign Tech Fund Proposal

In May 2026, OpenForum Europe submitted a [public support letter](https://eu-stf.openforumeurope.org) to the European Commission calling for a pan-European sovereign tech fund modelled on the German STF, with a proposed budget of €350 million over the 2028 to 2034 Multiannual Financial Framework.

The policy primer underpinning the proposal names dependency mapping as a core activity: the fund should begin by mapping software dependencies across critical infrastructure to identify under-maintained, vulnerable, or strategically important open source components. The primer recommends the European Commission commission an initial mapping to inform funding prioritisation before the MFF negotiations close.

Signatories of the support letter include the Chief Software Officer of Mercedes-Benz AG and the Chair of the OpenRail Association. The proposal validates the thesis of the CSRR whitepaper at policy level. It also positions the registry as infrastructure the fund would procure rather than build.

---

## Where CSRR fits

The initiatives above share a common profile: they fund or maintain components, advocate for policy, or produce engineering tooling. None produces a named-maintainer risk assessment as a commercial, continuously maintained data asset. None supplies underwriting-grade data to insurers. None operates independently of the technology companies or governments whose interests shape their priorities.

Four gaps remain unfilled by any of the above.

**Named-maintainer bus factor analysis as a commercial data product.** Knowing a component is widely deployed is not the same as knowing who maintains it in practice, what their employment situation is, and what the real bus factor is. That assessment requires human research, not a registry crawl. It is the layer that makes the difference between an algorithmic score and a risk finding an insurer can act on.

**The OT and firmware dependency layer.** C libraries distributed through apt, vendoring, and static linking are structurally invisible to methodologies built on registry APIs. The components embedded in vehicle ECUs, medical device firmware, and railway signalling systems are the components with the longest patch timescales and the highest consequence of availability failure. They are also the ones no existing initiative is positioned to assess.

**Independent governance.** An entity assessing the sustainability of components that manufacturers depend on cannot be governed by those manufacturers or by the cloud companies whose infrastructure priorities dominate existing foundations. Independence is not a differentiator. It is the precondition for the assessment to be trusted by all parties.

**The manufacturer-facing conversation.** The organisations above speak to engineers and grant committees. The buyer who carries the liability is the CFO, the board risk committee, and the insurance underwriter. Reaching that buyer requires framing the problem as an unpriced supply chain liability, not as an engineering concern. That conversation does not happen through an OpenSSF working group.

---

*Last updated: 2026-05-15 · [Whitepaper](https://criticalsoftwareriskregister.com/whitepaper/) · <a href="https://github.com/criticalregistry/registry" target="_blank" rel="noopener">Registry</a> · [Methodology](https://criticalsoftwareriskregister.com/methodology)*


