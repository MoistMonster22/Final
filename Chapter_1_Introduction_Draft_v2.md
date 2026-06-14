## Chapter 1: Introduction

> **Draft (v2) for the final thesis — "Smart Solar-Driven Energy Management for EV Charging Stations".**
> Background is now one flowing section (no sub-headings). Scope reworked to add Inclusions/Exclusions
> and an Equipment, Budget and Tools table. **[Insert …]** markers show where your own proposal
> figures go; **[citation needed]** marks where your proposal stated a fact without a source — fill
> these (IEA 2025, already verified for Chapter 2, covers the growth claims). Items in **[square
> brackets]** are notes for you to action.

### 1.1 Overview

This thesis investigates a smart, solar-driven energy management system (EMS) for electric vehicle
(EV) charging stations. The transport and energy sectors are undergoing parallel transformations — the
rapid electrification of road transport and the rapid growth of renewable generation — and the point
at which they meet, the EV charging station, is where this project sits. This chapter introduces the
background and motivation, states the problem, aim, objectives and research questions, defines the
scope, and outlines the structure of the thesis.

---

### 1.2 Background

The global automotive industry has changed rapidly in recent years, with electric vehicle (EV) sales
growing explosively — surpassing ten million units in a single year — driven by technological progress,
government measures such as subsidies and tax incentives, and tightening vehicle-emission standards
**[citation needed — cite IEA 2025, already verified for Chapter 2]**. In parallel, renewable energy
has become a central part of the global power system: the steep fall in solar photovoltaic (PV)
installation costs over the past decade has driven rapid growth in solar capacity worldwide, and solar
is projected to supply a substantial share of electricity in the years ahead **[citation needed — e.g.
IEA or IRENA]**. This growth in variable renewable generation creates an urgent need for smart energy
solutions that can manage the fluctuation of solar output and bridge intermittent renewable supply with
steady demand. Where these two transitions meet — the EV charging station — the role of a smart energy
management system (EMS) is to balance solar supply against charging demand while making the best use of
renewable energy according to solar conditions and low grid-price periods.

Australia is particularly well placed for this kind of system. The country has some of the most intense
solar irradiance in the world, and its solar PV installation rate has risen across residential,
commercial and utility-scale sectors, supported by government programs through the Australian Renewable
Energy Agency (ARENA) and the Clean Energy Finance Corporation (CEFC) **[citation needed — ARENA/CEFC
or Clean Energy Council report]**. EV adoption is also rising quickly, driven by greater public
awareness, improving vehicle capability and government backing. Many urban areas, however, continue to
experience grid congestion and high peak demand — conditions that decentralised, renewable-based
charging infrastructure can help to relieve. Australia's abundant sunlight and evolving energy
regulations therefore make it a strong setting for smart, solar-powered EV charging, and this study is
significant because it addresses Australian-specific conditions while contributing to global progress
in sustainable transport and energy management.

The motivation for a smarter approach becomes clear when the limitations of conventional charging are
considered. Existing grid-connected chargers are largely static: they operate on fixed schedules that
do not respond to real-time changes in energy supply, demand or price. This produces inefficiency
through higher energy costs under time-of-use tariffs, under-use of available solar energy during sunny
periods, heavy grid dependency during peaks, and limited integration with battery storage to smooth
fluctuations. The same systems interact poorly with the grid, since uncoordinated charging can worsen
peak demand and contribute to instability, and they are slow to accommodate emerging capabilities such
as vehicle-to-grid (V2G) operation and dynamic pricing. A smart EMS overcomes these shortcomings by
making real-time control decisions, distributing power across solar, battery and grid sources to charge
EVs at lower cost and lower emissions, adjusting charging in response to current prices and expected
loads, and directing solar energy where it is most valuable.
**[Insert Figure 1.1 — "Smart charging infrastructure" (Koch & Bräuchle 2025), your proposal figure.]**

Solar-powered EV charging offers several clear benefits alongside genuine obstacles. It provides grid
relief, since distributed solar reduces stress on the network, especially at midday when solar output
peaks; it delivers cost savings by substituting expensive grid power during high-demand periods; and it
brings environmental benefits through lower emissions and cleaner urban air. These opportunities are
balanced by the intermittency of solar generation, which depends on weather and therefore requires
storage to be usable on demand; by questions of economic viability, since the upfront cost of PV and
battery systems is high and correct sizing is essential to long-term benefit; and by the demands of
system integration, since a reliable real-time controller must link PV output, battery storage and the
charging load. A well-designed EMS is precisely what allows the opportunities to be captured while these
challenges are managed.
**[Insert Figure 1.2 — "Benefits of solar-powered EV charging" (Mckay 2024), your proposal figure.]**

For this reason the smart EMS is the central component of a modern, sustainable and efficient charging
station. Its key functions are real-time monitoring of variables such as solar irradiance, battery
state of charge, EV demand and grid price; dynamic control of power distribution across solar, battery
and grid so that operation is both efficient and economical; predictive analytics that use historical
and present data to anticipate available energy, demand and price; and scalability to accommodate
future growth in charging capacity, renewable generation and storage. Through these functions the EMS
creates a flexible power system that maximises renewable energy use and reduces grid dependence,
delivering both economic and environmental gains.
**[Insert Figure 1.3 — "Challenges in EV charging infrastructure" (Ahmad & Bilal 2023), your proposal
figure.]** This thesis develops and evaluates a rule-based EMS that performs the monitoring and
dynamic-control functions, with predictive analytics and full scalability identified as directions for
future work.

---

### 1.3 Problem statement

Despite the clear potential of solar-powered EV charging, the contribution of the energy-management
controller itself is often hard to isolate, because studies tend to vary weather and control strategy
together or to report only aggregate performance. In addition, many high-performing controllers rely on
computationally heavy optimisation, and many studies use generic tariffs rather than a real regional
structure (Chapter 2, Section 2.4). There is therefore a need to evaluate, in a controlled way, how
much a simple, transparent rule-based EMS can improve the cost, grid dependence and emissions of a
solar EV charging station under realistic local conditions — and to identify where such a simple
controller reaches its limits.

---

### 1.4 Aim and objectives

**Aim.** To develop and evaluate a smart, solar-driven energy management system for an EV charging
station, and to determine its effect on charging cost, grid dependency and carbon emissions under
realistic Victorian conditions.

**Objectives.**

1. To develop a model of a grid-connected PV–battery–EV charging station with a rule-based EMS in
   MATLAB/Simulink (Chapter 3).
2. To isolate and quantify the effect of the EMS through a controlled comparison of the controller
   switched off versus on (Chapter 4, Section 4.2).
3. To characterise how the operating context — solar irradiance, tariff structure and EV demand —
   influences performance (Chapter 4, Sections 4.1, 4.3, 4.4).
4. To evaluate the economic and environmental performance using region-specific tariff and emission
   data, and to relate the outcomes to sustainability goals (Chapter 4, Sections 4.2, 4.6).

---

### 1.5 Research questions

1. Does a smart solar-driven EMS improve the cost, grid dependency and emissions performance of an EV
   charging station, and by how much?
2. How does the benefit of the EMS depend on the electricity tariff structure?
3. How do solar irradiance and EV demand level affect the station's performance and self-sufficiency?

---

### 1.6 Scope and limitations

This study is a simulation-based investigation of a single, small grid-connected charging station
modelled in MATLAB/Simulink. The EMS is rule-based; model-predictive and optimisation-based control are
treated as future work. The model is deterministic, using representative daily profiles for irradiance,
temperature, demand and price, so it does not capture day-to-day stochastic variation. The tariff and
grid emission factor are Victorian and representative, and the battery is charged from solar only. These
boundaries are stated here and revisited in the results (Chapter 4, Section 4.5) and conclusions
(Chapter 5).

#### Inclusions and exclusions

The boundaries of the study are summarised in Table 1.1.

**[Insert Table 1.1 here — Inclusions and exclusions.]**

| Included in this study | Excluded from this study |
|---|---|
| A single grid-connected PV–battery–EV charging station | Networks of multiple stations and their interactions |
| A rule-based (logic-driven) energy management system | Model-predictive / optimisation-based controllers (future work) |
| Deterministic daily profiles for irradiance, temperature, demand and price | Stochastic / random day-to-day variation (future work) |
| Battery charged from solar only | Grid charging of the battery; battery degradation modelling |
| Victorian time-of-use tariff and Victorian grid emission factor | Other regions' tariffs and emission factors |
| Cost, grid energy, peak grid power, self-sufficiency and CO₂ as performance metrics | Detailed power-electronics, converter and power-quality modelling |
| One-day (24-hour) simulation at 15-minute resolution | Long-term (seasonal/annual) and lifecycle financial analysis |
| Vehicle demand represented as an aggregate charging load | Individual vehicle behaviour, V2G and demand-response signalling |

#### Equipment, budget and tools required

The project is simulation-based, so the resource requirements are modest and centred on software and
computing. They are summarised in Table 1.2.

**[Insert Table 1.2 here — Equipment, budget and tools.]**

| Item | Purpose in the project | Cost to the project |
|---|---|---|
| MATLAB R2026a with Simulink | Building and running the charging-station model and the rule-based EMS | University-provided licence (no direct cost) |
| Personal computer (standard specification) | Running simulations, analysis and document preparation | Already owned (no direct cost) |
| Microsoft Excel | Tabulating and analysing simulation results (Results_Comparison.xlsx) | University-provided licence (no direct cost) |
| Microsoft Word | Thesis writing and documentation | University-provided licence (no direct cost) |
| CQU library and online databases | Accessing peer-reviewed literature for the review | University-provided (no direct cost) |
| Victorian tariff and emission-factor data (ESC; DCCEEW) | Realistic economic and environmental inputs to the model | Publicly available (no direct cost) |

Because the work is conducted entirely in simulation, no physical hardware, laboratory time or
consumables were required, and the project carried no direct monetary budget; the principal resource
was the software listed above together with the time allocated to model development, simulation and
writing.

---

### 1.7 Thesis structure

The remainder of the thesis is organised as follows. **Chapter 2** reviews the literature around seven
themes and identifies the research gap. **Chapter 3** describes the methodology — the model, the eight
governing equations, the rule-based EMS and the controlled design of experiment. **Chapter 4** presents
and interprets the results of the four studies, including an errors-and-limitations analysis and the
contribution to the UN Sustainable Development Goals. **Chapter 5** draws conclusions, relates them to
the literature, discusses applications and implications, and recommends future work.

---

### References used in this chapter

Ahmad, F. & Bilal, M. 2023, 'A comprehensive analysis of electric vehicle charging infrastructure,
standards, policies, aggregators and challenges for the Indian market', *Energy Sources, Part A:
Recovery, Utilization, and Environmental Effects*, vol. 45, no. 3, pp. 8601–8622.

Koch, A. & Bräuchle, M. 2025, *Smart charging – intelligent electric vehicle charging*, Bosch Global,
viewed [date], <https://www.bosch.com/stories/smart-charging/>.

Mckay, B. 2024, *Beyond the plug: discover the potential of solar electric cars*, GreenMatch, viewed
[date], <https://www.greenmatch.co.uk/blog/potential-of-solar-electric-cars>.

> **Notes for you to action:**
> - **Re-insert your three proposal figures** (1.1–1.3) with the captions and citations shown.
> - **Fill the [citation needed] marks** in the Background — send me the IEA/ARENA references and I'll
>   format them in Harvard and drop them in.
> - **Equipment/budget table:** I assumed standard university-provided software and no monetary budget,
>   which fits a simulation project. If your program requires a costed budget (e.g. notional licence
>   values or your time costed at an hourly rate), tell me and I'll add a costed version.
