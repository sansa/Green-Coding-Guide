---
title: "Software Sustainability Handprint"
nav_order: 9
permalink: /handprint/
---

# 9. Software Sustainability Handprint

Green coding, as described in this guide, is primarily concerned with reducing the direct energy cost of running software. But software also has an indirect environmental dimension: the systems it enables, the behaviours it mediates, and the physical processes it replaces or improves can have positive environmental effects that far exceed the energy cost of the software itself. This positive environmental contribution is referred to as the **software sustainability handprint**.

## 9.1 From Footprint to Handprint

The concept of a *handprint* originates in sustainability research as a counterpart to the more familiar *footprint*. Where a footprint measures the environmental burden of an activity — the resources consumed, the emissions generated — a handprint measures the environmental benefit it creates: the footprint that is avoided, reduced, or restored as a result of the activity (Biggs et al., 2020).

Applied to software, the distinction maps cleanly onto two separate questions:

- **Footprint question**: How much energy does this software consume when it runs?
- **Handprint question**: What environmental harm does this software prevent or reduce by functioning as intended?

These are independent. Software with a large footprint can have a large positive handprint — for example, a computationally intensive route optimisation system that saves millions of litres of fuel annually. Software with a small footprint can have a negligible handprint — for example, a well-optimised but functionally trivial application. And software can, in some cases, have a negative handprint — enabling activities or consumption patterns that increase total environmental harm.

Green coding practice addresses the footprint dimension. Understanding the handprint dimension requires asking a different and broader question: *what does this software make possible, and what is the environmental consequence of that?*

## 9.2 How Software Creates Positive Environmental Impact

Software creates environmental benefit through several distinct mechanisms. The following categories illustrate the range of impact, from substitution of physical processes to optimisation of physical systems.

**Dematerialisation**
Software can replace physical goods and services that require energy, materials, and logistics to produce and deliver. Digital documents replace printed ones. Video calls replace flights. Streaming media replaces physical discs and the supply chains that produce and distribute them. The environmental gain from dematerialisation is real but not automatic: it depends on whether the digital alternative is actually adopted in place of the physical one, rather than in addition to it (Hilty and Aebischer, 2015).

**Enabling remote work and distributed collaboration**
Software platforms that support remote work reduce the need for daily commuting and business travel, both of which are significant sources of transport-related emissions. The environmental benefit depends on travel displacement actually occurring, and is offset to a degree by increased residential energy consumption and the energy cost of the collaboration software itself. The net effect is generally positive when long commutes or flights are replaced (Hook et al., 2020).

**Optimising physical systems**
Some of the largest handprint opportunities arise from software that makes physical infrastructure more efficient. Examples include:
- *Smart building management systems* that dynamically adjust heating, cooling, and lighting based on occupancy and weather, reducing energy consumption in buildings — which account for a substantial share of global energy use.
- *Intelligent transport systems* that optimise traffic flow, reduce idling, and improve public transport scheduling, lowering fuel consumption across vehicle fleets.
- *Precision agriculture platforms* that use sensor data and modelling to apply water, fertiliser, and pesticides only where and when they are needed, reducing resource consumption and associated emissions.
- *Grid management and demand response software* that integrates renewable energy sources, shifts flexible demand to periods of high renewable generation, and reduces curtailment of wind and solar power.

**Enabling scientific and engineering progress**
Software tools that accelerate research into clean energy, materials science, climate modelling, and low-carbon engineering can have handprint effects that compound over time. The environmental value of a simulation tool that reduces the cost of developing a more efficient battery chemistry, for example, cannot be captured in the software's own energy footprint — it emerges through the downstream applications of the research it enables.

## 9.3 Evaluating Handprint Claims

The handprint concept is valuable but easily misused. Because handprint effects are indirect and involve counterfactuals — what *would have happened* without the software — they are inherently more difficult to measure and verify than footprint effects. Several principles guide rigorous handprint evaluation.

**Additionality**
A genuine handprint requires that the environmental benefit would not have occurred without the software. If a route optimisation system is deployed in place of a system that was already performing adequate optimisation, the marginal benefit of the new system is the relevant handprint — not the total emissions avoided by any route optimisation whatsoever. Additionality is frequently overstated in sustainability reporting.

**Rebound effects**
Efficiency improvements enabled by software can trigger increases in the activity being made efficient, partially or fully offsetting the environmental gain. More efficient navigation reduces fuel consumption per trip — but cheaper, faster, or more convenient transport can increase the total number of trips. More efficient data compression reduces the energy cost per gigabyte — but lower cost per gigabyte increases total data consumption. These *rebound effects* must be considered when estimating net handprint.

**Counterfactual specificity**
A handprint claim requires specifying what the alternative scenario is. "Software X reduces carbon emissions" is not a complete claim — it must be compared to a defined baseline. Is the alternative no software at all? An earlier version of the software? A manual process? A competitor's product? The magnitude of the handprint depends heavily on which counterfactual is chosen, and the choice should be explicit and justified.

**Scope and time horizon**
Handprint effects can operate over very different timescales. The fuel saved by a route optimisation system is realised immediately, while the effect of accelerating clean-energy research may take decades to materialise. Long time horizons increase uncertainty. Be explicit about the scope of the handprint claim — which effects are included, over what time period, and with what uncertainty.

## 9.4 Net Impact and the Relationship Between Footprint and Handprint

For most software systems, the direct energy footprint is small relative to the functional value delivered. A well-designed smart thermostat system, for example, might consume a few kilowatt-hours per year in communication and computation while enabling heating and cooling savings of hundreds of kilowatt-hours per year in the buildings it manages. In such cases, the handprint substantially dominates the footprint, and the net environmental impact of the software is clearly positive.

However, this favourable ratio is not universal, and it should not be assumed. Software that consumes significant energy — large-scale data processing, continuous video streaming, AI model training — can only claim a positive net impact if its handprint effects are real, additional, and large enough to outweigh its footprint. Claims to that effect should be held to the same standard of evidence as any other empirical claim.

For software developers and architects, the practical implication is this: **reducing the footprint and understanding the handprint are complementary, not competing, concerns**. A system with a genuine positive handprint can maximise its net benefit by also minimising its operational footprint. And the discipline of thinking rigorously about environmental impact — quantitatively, counterfactually, with attention to rebound effects — is the same discipline whether it is applied to footprint or handprint.

---

## References

Biggs, R., de Vos, A., Preiser, R., Clements, H., Maciejewski, K. and Schlüter, M. (eds.) (2020) *The Routledge Handbook of Research Methods for Social-Ecological Systems*. Routledge.

Hilty, L.M. and Aebischer, B. (2015) *ICT for sustainability: An emerging research field*. In: Hilty, L.M. and Aebischer, B. (eds.) ICT Innovations for Sustainability. Springer, pp.3–36.

Hook, M., Sovacool, B.K. and Sorrell, S. (2020) *A systematic review of the energy and climate impacts of teleworking*. Environmental Research Letters, 15(9), 093003.

Plepys, A. (2002) *The grey side of ICT*. Environmental Impact Assessment Review, 22(5), pp.509–523.
