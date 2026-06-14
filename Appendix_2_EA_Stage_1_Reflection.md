## Appendix 2: Reflection on EA Stage 1 Competency Standard (CV82)

> **Competency Reflection for Professional Engineer Entry to Practice**
> Master of Engineering (CV82), CQU
> Student: Moniruggaman Haris | ID: 12265231
> Thesis: "Smart Solar-Driven Energy Management for EV Charging Stations"
> Supervisor: Dr. Sujeewa Hettiwatte
>
> This reflection maps the thesis work against the Engineers Australia Stage 1 Competency Standard
> for Professional Engineers (PE1, PE2, PE3). For each competency element, a brief statement
> describes how the thesis demonstrates that competency, followed by guidance on the evidence
> hyperlinks to provide.

---

## PE1: Engineering Knowledge

### PE1.1 – Fundamentals of Mathematics, Natural and Physical Sciences

**Demonstrated competency:** The thesis applies fundamental principles from mathematics and physics
to model the charging station. The photovoltaic (PV) power model (Eq. 3.1) uses algebraic and
linear relationships to represent power generation as a function of irradiance, temperature and
efficiency. State-of-charge (SoC) accounting (Eq. 3.4) applies conservation of energy and
charge-discharge dynamics. Energy balance (Eq. 3.5) enforces the physical principle of energy
conservation. Daily energy and cost integrals (Eq. 3.2, 3.6, 3.7) use summation and discrete
calculus. The simulation iterates these relationships at 15-minute steps, requiring understanding of
discrete time-stepping and numerical stability.

**Evidence hyperlinks to provide:**
- `Chapter_3_Equations_Explained.docx` — Equations 3.1–3.8 with full term definitions and unit
  analysis. (Direct link to the file in outputs; alternatively, Chapter 3, Section 3.3.)
- `run_thesis_sim.m` lines 50–150 — the function `emsStep()` which implements all governing
  equations step-by-step. (Link to /mnt/user-data/outputs/run_thesis_sim_local.m.)
- Chapter 3, Section 3.2 — "Validation and consistency checks," confirming energy balance closure
  to zero error.

---

### PE1.2 – In-Depth Technical Competence in Electrical/Energy Engineering Discipline

**Demonstrated competency:** The thesis develops detailed technical competence in solar photovoltaic
systems, battery energy storage, electrical power balancing and grid integration. Specific technical
areas include PV temperature-corrected power models (Chapter 2, Section 2.2.2; Eq. 3.1 with
Skoplaki & Palyvos 2009 foundation), battery state-of-charge management with charge/discharge
efficiencies and SoC limits (Chapter 2, Section 2.2.3; Eq. 3.4), time-of-use tariff dispatch
logic and arbitrage (Chapter 2, Section 2.2.5), and grid power flow equations accounting for
multiple sources (Eq. 3.5). The student designed a complete model with realistic parameter values
for a Victorian grid-connected station: 10 kW PV, 20 kWh battery, 0.92 round-trip efficiency,
20–95% SoC limits, and 0.25 h (15-minute) timestep.

**Evidence hyperlinks to provide:**
- `Chapter_3_Equations_Explained.docx` — all equations with discipline-specific detail (PV
  temperature dependence, MPPT efficiency, SoC bounds, etc.).
- `ems_core.m` or `EMS_block_code.m` — parameter definitions (lines 7–19) and their physical
  significance. (Link to /mnt/user-data/outputs/EMS_block_code.m.)
- Chapter 2, Sections 2.2.2–2.2.6 — detailed literature review of PV, storage, control and tariff
  disciplines, showing depth of subject knowledge.
- Chapter 3, Section 3.4 — "Model parameters and assumptions," which justifies each chosen value
  against the literature.

---

### PE1.3 – Understanding of Engineering Principles and their Application

**Demonstrated competency:** The thesis applies foundational engineering principles including
energy conservation, thermodynamic efficiency, control theory and systems modelling. The power
balance (Eq. 3.5) applies conservation of energy to the entire station. Round-trip battery
efficiency (Chapter 3, Section 3.4) reflects thermodynamic loss. The rule-based EMS (Chapter 3,
Section 3.5) is a state-based control system that reads the battery SoC and electricity price and
makes dispatch decisions — a direct application of feedback control principles. The MATLAB/Simulink
model integrates these principles into a discrete-time dynamic system simulated over 96 daily
timesteps, demonstrating systems-level engineering thinking.

**Evidence hyperlinks to provide:**
- Chapter 3, Section 3.3 — "The energy management system (EMS)," which explains the rule-based
  logic and its feedback structure.
- Chapter 3, Section 3.5 — the full flowchart (Figure 3.1) showing the control decision hierarchy
  and time-stepping loop.
- `run_thesis_sim.m` or `EMS_block_code.m` — the control logic implementation showing how SoC
  (state) determines power dispatch (action).
- Chapter 3, Figure 3.1 — the detailed Simulink flowchart showing the closed-loop feedback from
  battery to controller to dispatch.

---

### PE1.4 – Broad Understanding of Engineering in its Social, Environmental and Economic Context

**Demonstrated competency:** The thesis is motivated by two societal challenges: the electrification
of transport (Chapter 1, Section 1.1) and the need for affordable, clean energy (Chapter 2, Section
2.2.1). The environmental impact is quantified through avoided CO₂ emissions using the Victorian
grid emission factor (Chapter 3, Section 3.4; Chapter 4, Section 4.6), and the economic context is
established through the time-of-use tariff structure that makes the EMS economically viable (Chapter
2, Section 2.2.5; Chapter 4, Section 4.3). The thesis explicitly maps its findings to UN Sustainable
Development Goals 7, 13 and 11 (Chapter 4, Section 4.6), situating the engineering outcomes within a
recognised global sustainability framework. The literature review (Chapter 2) surveys not only
technical solutions but also policy drivers, market structures and consumer impacts.

**Evidence hyperlinks to provide:**
- Chapter 1, Section 1.1 — "Problem statement and motivation," which frames the social/environmental
  context (EV uptake, grid decarbonisation).
- Chapter 2, Section 2.2.1 — "Electrification," discussing transport emissions and the need for clean
  energy.
- Chapter 4, Section 4.6 — "Contribution to the UN Sustainable Development Goals," explicitly linking
  the results to SDG 7, 13 and 11.
- Chapter 4, Section 4.2 — discussion of emissions avoidance (29.73 kg CO₂ avoided), showing
  environmental numeracy.
- Chapter 5, Section 5.3 — "Applications and implications," discussing deployment conditions and
  business viability.

---

## PE2: Engineering Application Ability

### PE2.1 – Ability to Identify, Formulate and Solve Engineering Problems

**Demonstrated competency:** The thesis follows a disciplined problem-solving methodology. The
student identified the core problem (Section 1.2 of the thesis, research questions Q1–Q3): *how
much do a simple EMS, a time-varying tariff, and the level of EV demand affect station
performance?* They formulated the problem by designing a controlled, one-variable-at-a-time
simulation study (Chapter 3, Table 3.2) that isolates each effect. The solution involved developing
a working MATLAB/Simulink model, implementing a rule-based controller, debugging and validating it
(Chapter 4, Section 4.5, validation checks), and running six scenarios to produce quantified results.
Each problem was tackled systematically: the initial all-zero model output was diagnosed and fixed
(documented in the thesis revisions), assumptions about the EMS baseline were questioned and
corrected, and the final results stand on a clean energy balance (zero error across all runs).

**Evidence hyperlinks to provide:**
- Chapter 1, Section 1.2 — the three research questions (Q1–Q3) that frame the problem.
- Chapter 3, Section 3.2 — "Design of experiment," Table 3.2, showing the systematic one-variable-
  at-a-time study design.
- Chapter 3, Section 3.3–3.6 — the mathematical formulation of the model (Eq. 3.1–3.8), the control
  logic, and the parametric design.
- `run_thesis_sim_local.m` — the complete implementation showing end-to-end problem-solving from
  inputs to metrics calculation.
- Chapter 4, Section 4.5 — validation section, showing energy-balance verification and assumption
  checking.

---

### PE2.2 – Understanding of Professional Responsibility, Including Social, Cultural, Global and
Environmental Responsibilities and the Need to Employ Principles of Sustainable Development

**Demonstrated competency:** The thesis is explicitly framed around sustainability. The PV array and
battery storage represent a commitment to on-site renewable energy (Chapter 2, Section 2.2.2–2.2.3).
The EMS improves not only cost (24% reduction, Chapter 4, Section 4.2) but also grid emissions: the
avoided CO₂ (29.73 kg/day in the base case, scaled to ~10.8 tonnes/year) demonstrates environmental
responsibility. The tariff structure (Victorian time-of-use) is grounded in regional policy and
consumer fairness. The student explicitly acknowledged limitations (Chapter 4, Section 4.5) —
deterministic inputs, storage-limited peak reduction, model simplifications — which is honest
professional practice. The future-work recommendations (Chapter 5, Section 5.5) propose extensions
that would increase sustainability impact: stochastic analysis for real-world variability, storage
sizing for peak reduction, and V2G for grid support.

**Evidence hyperlinks to provide:**
- Chapter 2, Section 2.4 — "Research gaps," identifying underexplored aspects of PV-EV integration
  and showing professional awareness of what remains unknown.
- Chapter 4, Section 4.2 — quantified emissions avoided, showing environmental calculation capability.
- Chapter 4, Section 4.6 — explicit mapping to UN SDGs 7, 13 and 11.
- Chapter 4, Section 4.5 — "Errors and limitations," full disclosure of assumptions and boundaries
  (storage sizing, deterministic input, linear PV model).
- Chapter 5, Section 5.5 — future-work recommendations that would strengthen sustainability outcomes
  (stochastic analysis, V2G, field validation).

---

### PE2.3 – Ability to Conduct Engineering Investigations

**Demonstrated competency:** The thesis conducts a systematic investigation through a controlled
parametric study. Each of the four studies (Chapter 4, Sections 4.1–4.4) holds other variables
constant and measures the effect of one independent variable: irradiance, EMS mode, tariff and
demand. The investigation is disciplined: inputs are derived from realistic Victorian grid and
weather data, the simulation runs deterministically so results are reproducible, and the outputs are
validated against energy conservation (zero error). The student investigated why the preliminary
simulation produced all-zero outputs, diagnosed it as a Simulink block configuration issue (not a
logic error), and corrected it. This shows investigative thinking and problem-solving under
uncertainty.

**Evidence hyperlinks to provide:**
- Chapter 3, Table 3.2 — "Design of experiment," the parametric study structure.
- Chapter 4, Sections 4.1–4.4 — each study section follows: description, interpretation, discussion
  and literature linking, showing investigative depth.
- Chapter 4, Table 4.1 — the complete results table for all six runs, showing data completeness.
- Chapter 4, Section 4.5 — validation and error analysis, showing investigative rigour.
- `Results_Comparison.xlsx` — the actual numerical results, providing the data foundation for
  investigation.

---

### PE2.4 – Ability to Design Solutions Considering Constraints

**Demonstrated competency:** The thesis designs an EMS that explicitly respects real-world constraints:
the battery SoC is constrained to 20–95% (Chapter 3, Section 3.4) to protect long-term health, the
charge/discharge power is limited to 10 kW (the rated converter capacity), the PV generation is
limited by the installed 10 kW array and Victorian weather patterns, and the tariff rates are set to
the actual Victorian time-of-use structure. The student acknowledged that a larger battery would be
needed for peak reduction (Chapter 4, Section 4.5) and proposed MPC as a future extension to handle
more complex constraints (Chapter 5, Section 5.5). The design balances competing goals: simplicity
(rule-based logic) against optimality (cost reduction), and computational speed against realism.

**Evidence hyperlinks to provide:**
- Chapter 3, Section 3.4 — "Model parameters and assumptions," detailing every constraint and its
  justification.
- Chapter 3, Section 3.5 — the EMS control rules, showing how constraints are incorporated into the
  decision logic (SoC limits, power caps, tariff thresholds).
- Chapter 4, Section 4.2 — "Effect of the energy-management system," acknowledging the peak
  limitation and explaining the storage constraint.
- Chapter 5, Section 5.5 — "Recommendations for future work," proposing MPC and larger storage as
  solutions to the identified constraints.

---

### PE2.5 – Ability to Use Appropriate Tools and Technologies

**Demonstrated competency:** The student demonstrates competence in both simulation software and
engineering analysis tools. They developed a working MATLAB/Simulink model (Chapter 3, Section 3.3),
designed it using block diagrams (Figure 3.1), implemented a rule-based EMS controller embedded in a
MATLAB Function block, and debugged a non-trivial integration issue (the 3-D array reshaping in the
To Workspace block). They used Excel to tabulate and analyse results (Results_Comparison.xlsx),
generated publication-ready figures with MATLAB (Figures 4.1–4.4), and managed version control of
the model and scripts. They are proficient in mathematical modelling (differential equations, energy
balance), numerical simulation (discrete timestep integration), and technical communication (thesis
writing, figure design).

**Evidence hyperlinks to provide:**
- `EMS_block_code.m` — the MATLAB code implementing the control logic, showing coding competence.
- `run_thesis_sim_local.m` — the complete simulation script, showing integration of multiple tools
  and data flows.
- Chapter 3, Figure 3.1 — the Simulink block diagram, showing visual design of the system.
- Chapter 4, Figures 4.1–4.4 — four publication-ready figures generated from simulation output,
  showing graphical communication skill.
- `Results_Comparison.xlsx` — the results table, showing data management and analysis.

---

### PE2.6 – Ability to Manage Technically Demanding Projects

**Demonstrated competency:** The thesis is a well-scoped, technically demanding project completed
within the Master's program. The student managed multiple challenges: formulating the problem (three
research questions), designing an experiment (four controlled studies), building and validating a
model (with energy-balance checking), debugging a real integration issue, documenting the methodology
clearly (Chapter 3, with a detailed flowchart and 8 equations), conducting the analysis (Chapter 4,
with four sections of results and discussion), and synthesising conclusions (Chapter 5). The project
integrates knowledge from multiple domains (renewable energy, storage, control systems, grid
operations, economics) and results in a working simulation that produces defensible, peer-comparable
findings (cost reduction 24%, self-sufficiency 45%, emissions avoided). The student also managed risk
by identifying limitations (Chapter 4, Section 4.5) and recommending future work (Chapter 5, Section
5.5) rather than overstating the scope of the simple controller.

**Evidence hyperlinks to provide:**
- Chapter 1 — the full thesis structure, showing project scoping.
- Chapter 2 — the literature review (21 verified sources), showing breadth of knowledge integration.
- Chapter 3 — the complete methodology, showing technical depth and clarity of explanation.
- Chapter 4 — the comprehensive results and discussion, showing ability to extract and interpret
  findings.
- Chapter 5 — the conclusions and future work, showing reflection and risk management.

---

## PE3: Professional and Personal Attributes

### PE3.1 – Ability to Communicate Effectively

**Demonstrated competency:** The thesis is written in clear, descriptive academic English across
five chapters, each with its own narrative arc: Chapter 1 introduces the problem and motivation,
Chapter 2 reviews the literature and positions the gap, Chapter 3 explains the methodology in detail
with equations and flowcharts, Chapter 4 presents results with figures, tables and quantitative
analysis, and Chapter 5 concludes with implications and future work. Equations (Eq. 3.1–3.8) are
presented with full term definitions and units, so they are understandable to a reader unfamiliar
with the specifics of solar charging. The flowchart (Figure 3.1) uses standard Simulink notation and
a descriptive legend. The figures (Figures 4.1–4.4) use clear axes labels, legends and captions. The
results are presented in both tabular form (Table 4.1, with rows and columns named) and graphical
form, so readers can extract the information they prefer. The text explicitly connects findings to
the literature (Chapter 2 and Section 4.2), to the objectives (Chapter 5, Section 5.4), and to
sustainability goals (Chapter 4, Section 4.6), demonstrating communication that reaches both
technical and non-technical audiences.

**Evidence hyperlinks to provide:**
- Chapters 1–5 — the entire thesis, as evidence of sustained written communication over 50+ pages.
- Chapter 3, Section 3.3 — the equations with full definitions (Eq. 3.1–3.8), showing clarity in
  technical writing.
- Chapter 3, Figure 3.1 — the detailed flowchart with legend, showing ability to communicate
  algorithm/process visually.
- Chapter 4, Section 4.1 (or any of 4.1–4.4) — a representative section showing how results are
  described in prose alongside figures and tables, reaching multiple communication modes.
- Chapter 5, Section 5.4 — the explicit mapping of conclusions to research questions, showing
  clarity of communication about scope.

---

### PE3.2 – Ability to Manage Information and Documentation

**Demonstrated competency:** The student organised a complex, multi-part project: the thesis itself
(five chapters plus appendices), the MATLAB/Simulink model (`EV_EMS_Model.slx`), supporting scripts
(`run_thesis_sim_local.m`, `ems_core.m`, `EMS_block_code.m`), parameter definitions in code and in
Chapter 3, results in an Excel file (Results_Comparison.xlsx), four PNG figures, equations in both
a Word docx and the thesis markdown, and a full literature reference list (21 verified sources cited
in Harvard style). The student maintained version control — the model was debugged and the Simulink
block was fixed (the To Workspace array shape issue). The thesis references this software by
filename and location, enabling reproducibility. The methodology chapter (Chapter 3) is detailed
enough that someone else could rebuild the model from the written description. Documentation is
consistent: equations are labelled, figures and tables have captions, chapter references are precise.

**Evidence hyperlinks to provide:**
- Chapter 2, "References cited in this chapter" — the full bibliography in Harvard style, showing
  source management discipline.
- Chapter 3, Table 3.2 — the design-of-experiment table, showing parameter and scenario organisation.
- Chapter 3, Section 3.4 — "Model parameters and assumptions," showing systematic documentation of
  every model input.
- Chapter 3, "Validation and consistency checks" — statement of the energy-balance check, showing
  quality-control documentation.
- `Chapter_3_Equations_Explained.docx` — the equations document, showing parallel organisation of
  technical content.

---

### PE3.3 – Professional and Ethical Responsibility

**Demonstrated competency:** The student demonstrated professional responsibility through honest
reporting. The results include an acknowledged limitation: the EMS did not reduce instantaneous peak
power, which is reported clearly rather than hidden (Chapter 4, Sections 4.2 and 4.5). The student
explained why (storage is energy-limited) and identified that a larger battery or MPC would be needed
to address it — a critical, independent analysis. All sources are properly cited, and no claim is
made without literature or simulation support. The preliminary study (mentioned briefly during the
Simulink debugging) produced misleading results due to a baseline definition error; the student
corrected this rather than accepting the wrong answer. The model uses region-specific data (Victorian
tariff, Victorian emission factor) and acknowledges where the data are representative rather than
exact (Chapter 4, Section 4.5). The future-work section (Chapter 5, Section 5.5) is honest about
what the simple controller cannot do, showing maturity in professional practice.

**Evidence hyperlinks to provide:**
- Chapter 4, Section 4.5 — "Errors and limitations of the simulation," the full transparency section
  on model assumptions, numerical error, and boundary conditions.
- Chapter 4, Section 4.2 — the paragraph on "one result deserves honest emphasis," acknowledging the
  peak limitation candidly.
- Chapter 5, Section 5.1 — the final conclusion, which states what the EMS *does* achieve and what
  it *does not*, without overclaiming.
- Chapter 5, Section 5.4 — "Achievement of the aim, objectives and research questions," which maps
  each objective to the evidence and notes where the simple controller falls short.

---

### PE3.4 – Teamwork and Interpersonal Skills [if relevant]

**Demonstrated competency (contextual):** This is an individual thesis project, so direct teamwork
evidence is limited. However, the student engaged with a supervisor (Dr. Sujeewa Hettiwatte) and
completed the work to professional standards, incorporating feedback and revisions. The thesis
acknowledges that the model was validated against literature benchmarks (Hermans et al. 2024 for
peak-shaving comparison), showing ability to engage with peers' work and position their own. The
SDG mapping (Chapter 4, Section 4.6) shows awareness of how engineering work integrates with
societal teams and stakeholders. Future work recommendations include V2G and demand-response
integration (Chapter 5, Section 5.5), which inherently require coordination with grid operators and
other stakeholders.

**Evidence hyperlinks to provide:**
- Thesis cover or acknowledgments section — reference to supervisor and any feedback incorporated.
- Chapter 4, Section 4.2 — comparison of results to Hermans et al. (2024) measured peak reduction,
  showing engagement with literature and peer findings.
- Chapter 4, Section 4.6 — the SDG mapping, which contextualises engineering within a multi-stakeholder
  system.
- Chapter 5, Section 5.5 — V2G and demand-response recommendations, showing awareness of collaborative
  future practice.

---

## Summary and Self-Assessment

This thesis demonstrates achievement of **PE1 (Engineering Knowledge)** through mastery of PV
generation, battery storage and control system modelling grounded in fundamental mathematics,
physics and discipline-specific theory. **PE2 (Engineering Application Ability)** is demonstrated
by identifying and solving a well-scoped problem through a controlled simulation study, respecting
real constraints, using appropriate tools, and managing a technically demanding project to completion.
**PE3 (Professional and Personal Attributes)** is shown through clear written communication,
disciplined information management, honest reporting of results including limitations, and ethical
integrity throughout.

The student is ready for entry to professional engineering practice under the supervision and
guidance of more experienced engineers, with a foundation in renewable energy systems that directly
serves the transition to clean, affordable, sustainable energy infrastructure.

---

> **Note to examiner:** Hyperlinks in the evidence column should point to:
> - **Thesis chapters** (e.g. "Chapter 3, Section 3.3" or "Chapter 4, Table 4.1"): point to the final
>   thesis PDF or docx.
> - **Code files** (e.g. `run_thesis_sim_local.m`, `EMS_block_code.m`): link to the files in the
>   /mnt/user-data/outputs/ folder or attach as supplementary files.
> - **Excel file** (Results_Comparison.xlsx): attach as supplementary.
> - **Figures** (Figures 4.1–4.4): refer to chapter section (Figures are embedded in Chapter 4).
