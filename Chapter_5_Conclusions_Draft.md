## Chapter 5: Conclusions and Recommendations

> **Draft for the final thesis — "Smart Solar-Driven Energy Management for EV Charging Stations".**
> Written to be descriptive so you can trim. Follows the template's requirements: conclusions
> supported by the results, the relationship of the findings to the reviewed literature, the
> applications and implications of the work, explicit demonstration that the objectives, research
> questions and main aim were achieved, and recommendations for future work. Items in **[square
> brackets]** are notes for you to action — delete them before submission.

This chapter draws together the outcomes of the project, relates them to the literature reviewed in
Chapter 2, sets out the practical implications of the findings, and demonstrates that the project's
aim, objectives and research questions have been met. It closes with recommendations for future work
that follow directly from the limitations identified in Chapter 4.

---

### 5.1 Conclusions

This project set out to design and evaluate a smart, solar-driven energy management system (EMS) for
an electric vehicle (EV) charging station, and to determine whether such a controller improves the
station's cost, grid dependency and environmental performance. A grid-connected station comprising a
photovoltaic (PV) array, a battery energy storage system and an EV charging load was modelled in
MATLAB/Simulink and controlled by a transparent, rule-based EMS. The model was exercised through four
controlled, one-variable-at-a-time studies so that the effect of each factor could be isolated. The
results support several clear conclusions.

The principal conclusion is that the smart EMS delivers a substantial and measurable improvement in
the station's performance. On the same day and with the same demand, enabling the EMS reduced the
daily charging cost by 24%, reduced grid energy import by 23%, raised solar self-sufficiency from 29%
to 45%, and increased the carbon dioxide avoided by about 57% (Chapter 4, Section 4.2). Because only
the controller changed between these two cases, the improvement is attributable to the EMS itself
rather than to favourable conditions — the confounding that weakened the preliminary study was
removed by the controlled design. The mechanism is straightforward and was confirmed in the results:
the controller captures midday solar surplus in the battery and releases it during the expensive
evening tariff window, displacing the most costly grid imports.

A second conclusion is that the value of the EMS is created by the time-of-use price signal. When the
tariff was made flat, the controller correctly held the battery idle and produced no dispatch
benefit (Section 4.3). The smart logic therefore adds value precisely because, and only when, the
electricity price varies through the day — an important qualification for any deployment decision.

A third conclusion concerns the conditions under which the benefits are strongest. The study of solar
irradiance showed the expected sensitivity of a solar station to its resource, with cloud reducing
the direct solar contribution by about 62% while the storage preserved the evening dispatch (Section
4.1). The study of demand showed that solar self-sufficiency falls from 67% to 32% as demand grows
from half to one-and-a-half times the nominal level, because the fixed solar and storage capacity
covers a shrinking share of a larger load (Section 4.4). The EMS is therefore most effective at
moderate demand relative to the installed capacity.

A final, honest conclusion is that the simple rule-based EMS did not reduce the instantaneous peak
grid power, which remained governed by the evening demand once the battery was exhausted (Sections
4.2 and 4.4). This is a consequence of storage sizing rather than of faulty control: the battery is
energy-limited against the evening demand block and can shave only part of the peak. The conclusion is
not that the controller fails, but that peak reduction is a distinct objective requiring more storage
or a more capable control strategy. Throughout, the model's internal validity held: the energy
balance closed exactly in every run and the battery operated strictly within its limits.

---

### 5.2 Relationship of the findings to the literature

The findings sit consistently within the body of work reviewed in Chapter 2 and, in places, sharpen
it. The cost reduction and the suppression of grid draw during the peak reproduce, for a rule-based
controller, the same qualitative behaviour that optimisation-based and predictive controllers achieve
in the literature (Cheikh-Mohamad et al. 2023; Hermans et al. 2024). The result that a transparent
rule-based controller captures a 24% cost saving supports the position, advanced in the rule-based
control literature, that simple priority logic can deliver much of the available benefit without the
computational weight of an optimiser (Danielsson et al. 2025). The strong dependence of the
controller's value on the tariff structure aligns directly with the time-of-use pricing literature,
in which the price signal is the lever that makes storage and load-shifting worthwhile (Jeon et al.
2020; Yang et al. 2024).

At the same time, the findings clarify where the simple approach reaches its limit. The peak grid
power was not reduced, whereas a model-predictive controller with adequate storage achieved a large
measured peak reduction in a comparable grid-connected station (Hermans et al. 2024). This contrast
locates the present work precisely on the methodological spectrum reviewed in Chapter 2: the
rule-based controller is sufficient for cost, grid-energy and emissions objectives but not for peak
reduction, which is consistent with the storage-sizing literature that treats the matching of
capacity to demand as the binding constraint (Rehman et al. 2022). The weather and demand
sensitivities observed also reinforce the consensus that a solar station's performance is governed
first by its resource and by the role of storage in buffering it (Kumar et al. 2019; Alrubaie et al.
2023).

---

### 5.3 Applications and implications

The findings have several practical implications. For an operator of a small grid-connected,
solar-equipped charging station on a time-of-use tariff, the results indicate that adding a
transparent rule-based EMS is a low-complexity way to cut operating cost and grid dependence
materially — here by roughly a quarter — without the engineering effort of an optimisation-based
controller. Because the logic is simple and computationally light, it is well suited to low-cost
embedded hardware, which lowers the barrier to adoption.

The tariff-dependence result has a direct implication for where such a system should be deployed: the
benefit is realised only under a time-varying tariff, so the business case is strongest in markets
with pronounced peak pricing, such as the Victorian time-of-use structure used here, and weak or
absent under flat tariffs. The demand-sensitivity result implies a sizing guideline: for a given
solar and storage capacity there is a demand level beyond which self-sufficiency falls steeply, so the
PV and battery should be sized against the expected charging load rather than in isolation. Finally,
the project contributes, in a quantified way, to the United Nations Sustainable Development Goals —
principally SDG 7 (Affordable and Clean Energy) through higher renewable self-consumption and lower
charging cost, and secondarily to SDG 13 (Climate Action) through the avoided emissions and SDG 11
(Sustainable Cities and Communities) through reduced pressure on the urban network (Section 4.6) —
which situates the engineering outcomes within a recognised sustainability framework.

---

### 5.4 Achievement of the aim, objectives and research questions

The project achieved its stated aim and objectives. The **main aim** — to develop and evaluate a smart
solar-driven EMS for an EV charging station and determine its effect on cost, grid dependency and
emissions — was met: the EMS was designed, implemented in MATLAB/Simulink and evaluated, and its
effect was quantified (24% lower cost, 23% less grid energy, self-sufficiency raised from 29% to 45%,
57% more CO₂ avoided).

Taking the **objectives** in turn: a working PV–battery–grid charging-station model with a rule-based
EMS was built and validated (Chapter 3, confirmed by the exact energy balance in Chapter 4); the
effect of the EMS was isolated and measured through a controlled comparison (Section 4.2); the
influence of the operating context — irradiance, tariff and demand — was characterised (Sections 4.1,
4.3, 4.4); and the economic and environmental performance was evaluated using region-specific tariff
and emission data (Sections 4.2, 4.6). The **research questions** are answered correspondingly: a
smart solar-driven EMS does improve the cost, grid dependency and emissions performance of the
station (Section 4.2); that improvement is conditional on a time-varying tariff (Section 4.3); and it
is bounded by the installed solar and storage relative to demand (Section 4.4). The one objective the
simple controller did not satisfy — reducing the instantaneous peak grid power — was identified
honestly and explained as a storage limitation rather than left unexamined, which itself answers the
question of where a more advanced approach is required.

---

### 5.5 Recommendations for future work

The limitations identified in Chapter 4 point to clear and worthwhile extensions:

- **Model predictive control (MPC).** The rule-based controller acts only on present conditions.
  Replacing it with an MPC that uses short-term forecasts of solar generation, demand and price would
  allow the stored energy to be positioned more strategically and is the most promising route to
  reducing the instantaneous peak, which MPC has achieved in a real grid-connected station (Hermans
  et al. 2024).
- **Storage and PV sizing study.** Because the peak result was storage-limited, a systematic study
  that varies battery capacity and PV rating against demand would identify the capacity needed to
  shave the peak and would turn the demand-sensitivity finding into a concrete sizing tool (building
  on Rehman et al. 2022).
- **Stochastic evaluation.** The present study is deterministic. Introducing randomness into the
  inputs — day-to-day cloud variability and random EV arrival times and numbers — and running each
  scenario many times would yield distributions and confidence bounds on the headline savings rather
  than single values, following the stochastic approaches in the literature (Danielsson et al. 2025;
  Saleem et al. 2024).
- **Vehicle-to-grid (V2G) and demand response.** Extending the controller to return energy to the
  grid or to modulate charging in response to network signals would let the station provide active
  grid support rather than only reducing its own import (İnci et al. 2024).
- **Validation against measured data and exact local tariffs.** Confirming the model against field
  measurements from a real installation, and substituting the exact published Victorian tariff rates,
  would convert the representative figures into validated, site-specific results.

---

### 5.6 Closing remark

This project demonstrated, through a controlled simulation study, that a simple and transparent
solar-driven energy management system can deliver meaningful reductions in charging cost, grid
dependence and emissions for an EV charging station, while also identifying — honestly and
quantitatively — the boundary at which a more advanced controller or larger storage becomes
necessary. In doing so it both answers its research questions and lays a clear, evidence-based
foundation for the future work that would carry the system from a validated model toward a deployable
solution.

---

### How this chapter connects to the rest of the thesis

- **Chapter 1 (aim/objectives/questions):** Section 5.4 maps the conclusions back onto the aim,
  objectives and research questions stated in the introduction.
- **Chapter 2 (literature):** Section 5.2 relates each finding to the reviewed sources and shows where
  the work confirms, and where it bounds, that literature.
- **Chapter 3 (methodology):** the conclusions rest on the controlled design of experiment and the
  validation checks established there.
- **Chapter 4 (results):** every conclusion in Section 5.1 is traceable to a specific result, and the
  recommendations in Section 5.5 follow directly from the limitations in Section 4.5.

> **Note:** all quantitative claims here are taken from your Results_Comparison.xlsx via Chapter 4. If
> any result changes on re-running, update the figures quoted in Sections 5.1, 5.3 and 5.4 to match.
