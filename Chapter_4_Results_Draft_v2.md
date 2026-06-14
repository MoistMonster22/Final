## Chapter 4: Results and Analysis

> **Draft (v2) for the final thesis — "Smart Solar-Driven Energy Management for EV Charging Stations".**
> Rewritten to follow the template's three steps: (1) results description and interpretation, (2)
> discussion and analysis linked to the literature and objectives, and (3) figures/tables. Built on
> the actual simulation output (Results_Comparison.xlsx) and the four figures from
> `run_thesis_sim.m`. Written to be descriptive so you can trim. **[Insert …]** markers show exactly
> where each figure and table goes. Items in **[square brackets]** are notes for you to action.

This chapter presents the results of the simulation developed in Chapter 3 and interprets them in
detail. The model of the solar-driven electric vehicle (EV) charging station was run as four
controlled studies, each changing a single independent variable while holding the others fixed, so
that the influence of that variable on the station's performance could be isolated and read directly
from the figures and tables. The four studies correspond exactly to the design of experiment in
Table 3.2: the level of solar irradiance (Section 4.1), the energy-management system (EMS) itself
(Section 4.2), the structure of the electricity tariff (Section 4.3), and the level of EV charging
demand (Section 4.4). For each run the model recorded the solar energy supplied directly to the
vehicles, the energy supplied from the battery, the energy imported from the grid, the daily charging
cost, the peak grid power, the carbon dioxide avoided and the solar self-sufficiency. The chapter
first sets out the complete results (Table 4.1), then works through the four studies, and finally
discusses the findings against the project objectives and the literature, including an explicit
treatment of the errors and limitations of the simulation.

**[Insert Table 4.1 here — master results table for all six runs.]**

| Run | PV→EV (kWh) | Batt→EV (kWh) | Grid (kWh) | Cost (AUD) | Peak grid (kW) | CO₂ avoided (kg) | Self-sufficiency (%) |
|---|---|---|---|---|---|---|---|
| Sunny, EMS on (base case) | 24.31 | 13.80 | 45.89 | 19.66 | 14 | 29.73 | 45.4 |
| Cloudy, EMS on | 9.14 | 13.80 | 61.06 | 22.93 | 14 | 17.89 | 27.3 |
| Sunny, EMS off | 24.31 | 0 | 59.69 | 25.87 | 14 | 18.96 | 28.9 |
| Sunny, flat tariff | 24.31 | 0 | 59.69 | 15.49 | 14 | 18.96 | 28.9 |
| Sunny, low demand | 14.52 | 13.80 | 13.68 | 6.13 | 7 | 22.09 | 67.4 |
| Sunny, high demand | 26.12 | 13.80 | 86.08 | 34.86 | 21 | 31.13 | 31.7 |

Throughout this chapter the base case is a sunny day with nominal EV demand (84 kWh over the day),
the time-of-use tariff and the smart EMS enabled. Before any result is interpreted it is worth noting
the internal-consistency check carried out in every run: the instantaneous energy balance (Eq. 3.5)
was satisfied at every time step, with a maximum absolute error of zero across all six runs. This
confirms that the power-flow accounting neither creates nor loses energy, so the patterns described
below are properties of the model's behaviour rather than artefacts of numerical error.

---

### 4.1 Effect of solar irradiance

#### Step 1 — Description and interpretation

This study compared a sunny day with a cloudy day, the cloudy irradiance profile being set to 35% of
the sunny one, with the EMS enabled and all other inputs unchanged. The outcome is shown visually in
**[Insert Figure 4.1 here — bar chart of PV-to-EV energy and grid import, sunny vs cloudy]** and
numerically in the first two rows of Table 4.1.

The dominant trend is a strong, near-proportional fall in solar contribution as irradiance drops. The
solar energy delivered directly to the vehicles fell from 24.31 kWh on the sunny day to 9.14 kWh on
the cloudy day — a reduction of roughly 62%. Figure 4.1 shows this as a clear shift in the balance of
supply: the PV-to-EV bar shrinks markedly while the grid-import bar grows. Quantitatively, grid import
rose from 45.89 kWh to 61.06 kWh (an increase of about 33%), solar self-sufficiency fell from 45.4% to
27.3%, and the carbon dioxide avoided fell from 29.73 kg to 17.89 kg. The daily charging cost rose
comparatively gently, from AUD 19.66 to AUD 22.93 (about 17%), because the additional grid energy was
drawn partly outside the most expensive tariff window.

A more subtle and important pattern is that the battery delivered the same 13.80 kWh to the vehicles on
both days (Table 4.1, column 2). At first sight this is surprising, since the cloudy day generates far
less solar energy. The explanation lies in the daily profile: the long midday period carries no
charging demand, so even the reduced cloudy generation has time to charge the battery toward its upper
limit before the evening. The stored energy available for the evening peak is therefore largely
preserved under cloud, and only the *direct* solar contribution collapses. In effect, the storage
decouples the evening dispatch from the day's weather, which is precisely the buffering role the
literature assigns to storage in an intermittent solar system.

#### Step 2 — Discussion and analysis

This behaviour follows directly from the PV model adopted in Chapter 3 (Eq. 3.1), in which generation
scales with incident irradiance; a day at 35% irradiance therefore yields correspondingly less power,
and the shortfall is transferred to the grid. The result is consistent with the wider finding that the
performance of a solar charging station is governed first by its solar resource (Alrubaie et al. 2023;
Rashid et al. 2024), and it quantifies that sensitivity for the modelled system. The preservation of
battery output under cloud reinforces the argument, made by Kumar et al. (2019) and in review work
(Alrubaie et al. 2023), that storage is not optional but essential to a solar station, because it is
what sustains supply when generation is low. In terms of the project objectives, this study
establishes the baseline weather-sensitivity of the station and shows that storage partially insulates
it — a necessary piece of context for interpreting the EMS benefit that follows.

---

### 4.2 Effect of the energy-management system

#### Step 1 — Description and interpretation

This is the central study of the thesis and the one that most directly answers the research question.
It compared the station with the EMS switched off against the EMS switched on, on the same sunny day,
with the same demand and the same time-of-use tariff, so that any difference can be attributed to the
controller alone rather than to conditions. The time-resolved behaviour is shown in **[Insert Figure
4.2 here — grid power over the day, EMS off vs EMS on, with the tariff overlaid]** and the daily totals
in the "Sunny, EMS off" and "Sunny, EMS on" rows of Table 4.1.

Every energy and cost metric improved when the EMS was enabled. The daily charging cost fell from AUD
25.87 to AUD 19.66, a reduction of 24.0%. Grid import fell from 59.69 kWh to 45.89 kWh, a reduction of
23.1%. Solar self-sufficiency rose from 28.9% to 45.4%, and the carbon dioxide avoided rose from 18.96
kg to 29.73 kg, an increase of about 57%. The single cause of all these changes is visible in Table
4.1: the battery delivered 13.80 kWh to the vehicles with the EMS on, against zero with it off. With
the EMS off the surplus midday solar is exported and the grid later covers the evening demand in full;
with the EMS on that surplus is captured in the battery and released during the peak-tariff window to
displace the most expensive grid imports.

Figure 4.2 reveals the *timing* behind the totals, and two distinct phases can be read from it. In the
morning the two grid-power traces lie on top of one another, because the tariff has not yet entered its
peak band and the controller deliberately holds the battery in reserve. During the evening peak
(16:00–21:00, shown by the raised tariff trace) the two curves separate clearly: the EMS-on grid draw
is suppressed while the battery discharges, and only after the stored energy is exhausted do both
curves climb to the full demand. The visible gap between the two curves during the early peak is, in
effect, a picture of the cost saving.

A further pattern must be read honestly from the same figure: the *peak* grid power did not change,
remaining at 14 kW with the EMS both off and on (Table 4.1, column 5). The curves converge again at the
top of the peak. This is the most important nuance in the results and is interpreted fully in the
limitations discussion below; in short, the battery shaves the early part of the peak but is energy-
limited and cannot hold the grid down for the whole evening.

#### Step 2 — Discussion and analysis

The direction and magnitude of these results are consistent with the coordinated-control literature.
The cost reduction achieved by holding stored solar energy and releasing it during the expensive
window is the same arbitrage mechanism that optimisation-based controllers exploit (Cheikh-Mohamad et
al. 2023), and the suppression of grid draw during the peak is the same qualitative behaviour reported
for a model-predictive controller implemented on a real grid-connected charging microgrid (Hermans et
al. 2024). That a transparent, computationally light rule-based controller captures a 24% cost
reduction and a 23% reduction in grid energy is itself a meaningful finding, because it supports the
view in the rule-based literature that simple priority logic can deliver much of the available benefit
without the complexity of an optimiser (Danielsson et al. 2025). Against the project's main aim — to
determine whether a smart solar-driven EMS improves the cost, grid dependency and emissions of an EV
charging station — this study answers in the affirmative and quantifies the improvement, while the
unchanged peak identifies the one objective the simple controller does not meet.

---

### 4.3 Effect of tariff structure

#### Step 1 — Description and interpretation

This study compared the time-of-use tariff against a flat tariff set at the same daily average price,
with the EMS enabled and the sunny base-case demand, to isolate the effect of the price signal on the
controller's behaviour. The result is shown in **[Insert Figure 4.3 here — battery state-of-charge over
the day, time-of-use vs flat tariff]** and in the "Sunny, EMS on" and "Sunny, flat tariff" rows of
Table 4.1.

The clearest pattern is in the battery dispatch. Under the time-of-use tariff the battery delivered
13.80 kWh to the vehicles; under the flat tariff it delivered nothing at all. Figure 4.3 explains this
vividly: under both tariffs the state-of-charge traces are identical through the day, rising from the
initial 50% to the 95% limit as the battery fills on midday solar surplus, but in the late afternoon
the two traces diverge decisively. Under the time-of-use tariff the state of charge falls steeply from
95% to its 20% floor as the battery discharges into the evening peak; under the flat tariff the state
of charge simply remains flat at 95% for the rest of the day. The controller, whose rule discharges the
battery only when the price exceeds the peak threshold, never acts under a flat price because that
threshold is never crossed.

The cost figures associated with this study must be interpreted carefully to avoid a misleading
conclusion. The flat-tariff bill (AUD 15.49) is lower in absolute terms than the time-of-use bill with
the EMS (AUD 19.66). This does **not** mean the flat tariff is "better" or that the EMS underperforms;
it reflects only that the time-of-use tariff charges a high price during the evening peak that the flat
tariff, by construction, does not. The legitimate measure of the controller's value is the EMS-off
versus EMS-on comparison under the *same* time-of-use tariff in Section 4.2 (a 24% saving). What this
study adds is the conditionality of that saving.

#### Step 2 — Discussion and analysis

The finding is that the value of the smart EMS is created entirely by the price signal: a time-varying
tariff is what gives stored energy something worth displacing, and without it the storage sits idle.
This is the premise on which the time-of-use pricing literature for EV charging rests (Jeon et al.
2020; Yang et al. 2024), and the result demonstrates the mechanism explicitly — the controller's
behaviour switches entirely on the presence or absence of a peak. It also connects to the broader
observation that well-designed tariffs are themselves a demand-response instrument that shifts load
away from peaks (Jung & Sundström 2023). In terms of the objectives, this study qualifies the headline
result of Section 4.2: the cost and emissions benefits of the EMS are real but are contingent on the
tariff environment, and would not arise for a customer on a flat tariff. This is an important piece of
critical context for any recommendation to deploy such a controller.

---

### 4.4 Effect of EV demand level

#### Step 1 — Description and interpretation

This study varied the EV demand across three levels — low (half of nominal, 42 kWh/day), nominal (84
kWh/day) and high (one-and-a-half times nominal, 126 kWh/day) — with the EMS enabled, the sunny profile
and the time-of-use tariff. The trends are shown in **[Insert Figure 4.4 here — grid import and
self-sufficiency versus daily EV demand]** and in the relevant rows of Table 4.1.

Two opposing trends emerge clearly as demand rises. First, the station's reliance on the grid increases
steeply: grid import rose from 13.68 kWh at low demand, to 45.89 kWh at nominal demand, to 86.08 kWh at
high demand. Second, and as the mirror image, solar self-sufficiency falls — from 67.4% to 45.4% to
31.7% — which Figure 4.4 shows as the descending self-sufficiency curve crossing the rising grid-import
curve. The peak grid power rose in step with demand, from 7 kW to 14 kW to 21 kW, and the daily cost
rose from AUD 6.13 to AUD 19.66 to AUD 34.86. By contrast, the carbon dioxide avoided rose only modestly
(22.09, 29.73 and 31.13 kg), flattening out as demand increased.

The pattern is explained by the fixed size of the solar and storage resources. At low demand the fixed
solar and battery output meets most of a small load, giving high self-sufficiency; as demand grows, the
same fixed output represents a progressively smaller fraction of the load, so the grid covers an
increasing share. The flattening of the avoided emissions follows from the same cause: the avoided
emissions are bounded by the finite amount of solar and stored energy the station can supply, so they
cannot keep rising once demand exceeds what that fixed resource can serve.

#### Step 2 — Discussion and analysis

This study indicates that, for a given solar and storage capacity, there is a demand level beyond which
self-sufficiency declines sharply and the station effectively reverts to grid dependence. That has a
direct practical implication for sizing, and it links to the storage- and PV-sizing literature, where
the matching of capacity to demand under realistic conditions is treated as a central design problem
(Rehman et al. 2022). The rising peak grid power with demand is the same storage-limitation effect
identified in Section 4.2, seen here from a different angle: as the evening block grows, the fixed
battery covers a smaller part of it and the uncovered remainder sets a higher peak. Against the
objectives, this study shows that the benefits demonstrated in Sections 4.2 and 4.3 are strongest at
moderate demand and weaken as demand outgrows the installed solar and storage — useful, honest guidance
for translating the model into a real installation.

---

### 4.5 Errors and limitations of the simulation

In keeping with good practice, the sources of error and the limitations of the study are identified
explicitly here so that the results above are read with appropriate caution.

**Numerical and modelling error.** The energy-balance check (Eq. 3.5) returned a maximum error of zero
across all runs, so there is no detectable numerical error in the power-flow accounting; the model
conserves energy exactly at the 15-minute resolution used. The principal *modelling* simplifications
are that the PV array is represented by a temperature-corrected linear model rather than a device-level
circuit, the maximum-power-point tracker is treated as a fixed efficiency, and the battery is modelled
as an energy reservoir with a constant round-trip efficiency rather than with voltage- and
temperature-dependent dynamics. These choices are appropriate for an energy-management study and follow
common practice (Skoplaki & Palyvos 2009; Saleem et al. 2024), but they mean the absolute energy and
cost figures should be read as representative rather than exact.

**The peak-power limitation.** The most significant limitation, visible in Sections 4.2 and 4.4, is that
the EMS did not reduce the instantaneous peak grid power, which remained governed by the evening demand
once the battery was exhausted. This is a genuine, interpretable result rather than a control fault:
the evening demand block represents about 56 kWh of energy, whereas a 20 kWh battery operating between
its 20% and 95% limits can deliver only about 14 kWh before it is empty. It therefore shaves the early
part of the peak but cannot hold the grid down for the full evening, so the instantaneous maximum
returns to the demand level. A reduction in the peak itself would require either substantially larger
storage or a predictive controller able to position the stored energy more strategically — the kind of
model-predictive control that achieved a large measured peak reduction in a station with adequate
storage (Hermans et al. 2024). This limitation is the principal driver of the future-work
recommendations in Chapter 5.

**Deterministic inputs.** The study is deterministic: each scenario uses a single fixed profile for
irradiance, temperature, demand and price, so the results are single representative values rather than
distributions. This is consistent with the controlled, one-variable-at-a-time aim of the design of
experiment, and it is what allows each effect to be isolated cleanly. It does mean, however, that the
results do not capture the day-to-day variability that a stochastic study would reveal, such as random
cloud cover or random EV arrivals; recent work that performs such stochastic analysis (Danielsson et
al. 2025; Saleem et al. 2024) shows the value of that extension, which is noted as future work.

**Input-data representativeness.** The tariff rates and the grid emission factor are regionally
appropriate but representative: the tariff follows the Victorian time-of-use structure and the emission
factor is the Victorian Scope 2 value (DCCEEW 2025; Essential Services Commission 2025). Confirming the
exact published rates would make the absolute cost figures exact, although the percentage results,
being ratios, would be largely unaffected.

---

### 4.6 Summary: meeting the objectives and answering the research questions

Read together, the four studies give a coherent and critically interpreted account of the modelled
station, and they map directly onto the project's aim and research questions. The central finding,
from Section 4.2, is that enabling the smart solar-driven EMS reduced daily charging cost by 24%,
reduced grid import by 23%, raised solar self-sufficiency from 29% to 45% and increased the carbon
dioxide avoided by about 57%, all on the same day and demand, so the improvement is attributable to the
controller itself. This directly answers the main research question — whether a smart solar-driven EMS
improves the cost, grid dependency and emissions performance of an EV charging station — in the
affirmative, and meets the corresponding objectives. Section 4.1 quantified the station's sensitivity
to its solar resource; Section 4.3 showed, as a piece of independent critical analysis, that the EMS's
value is created by the time-of-use price signal and vanishes under a flat tariff; and Section 4.4
showed how self-sufficiency declines once demand outgrows the fixed solar and storage capacity.

The one objective the simple controller did not meet — a reduction in the instantaneous peak grid power
— is reported honestly as a storage-limited result rather than concealed, and it is the clearest
signpost to the next stage of work. The benefits that were achieved were obtained with a transparent,
computationally light rule-based controller, which supports the thesis premise that such a controller
delivers meaningful value, while the peak result identifies precisely where a larger battery or a
predictive controller would be required. The validation checks from Chapter 3 held throughout: the
energy balance closed exactly and the battery operated strictly within its limits in every run.

---

### How this chapter connects to the rest of the thesis

- **Chapter 1 (introduction/objectives):** Section 4.6 maps each study onto the aim and research
  questions, showing which objective each result addresses.
- **Chapter 2 (literature):** the EMS benefit (4.2) and its tariff-dependence (4.3) confirm, for the
  modelled system, the cost and peak-shaving roles reported by Cheikh-Mohamad et al. (2023), Hermans et
  al. (2024), Jeon et al. (2020) and Yang et al. (2024); the storage-limited peak (4.2, 4.4) connects to
  the sizing literature (Rehman et al. 2022); the deterministic-input limitation (4.5) connects to the
  stochastic studies (Danielsson et al. 2025; Saleem et al. 2024).
- **Chapter 3 (methodology):** each study is one row of the design of experiment (Table 3.2), reported
  with the metrics of Eq. 3.6–3.8; the zero energy-balance error confirms the validation gate (Eq. 3.5).
- **Chapter 5 (conclusions):** Section 4.6's gains become the main conclusions, and the peak limitation
  (4.2, 4.4, 4.5) becomes the central recommendation — larger storage or a predictive (MPC) controller —
  with the demand study informing sizing guidance.

---

> **Step 3 — figures and tables to insert (summary of placement):**
> - **Table 4.1** — master results table — at the top of the chapter (already positioned above).
> - **Figure 4.1** — PV-to-EV vs grid import, sunny vs cloudy — in Section 4.1, Step 1.
> - **Figure 4.2** — grid power over the day, EMS off vs on, tariff overlaid — in Section 4.2, Step 1
>   (apply the colour/white-background fix so EMS-on is clearly distinguishable).
> - **Figure 4.3** — battery state-of-charge, time-of-use vs flat — in Section 4.3, Step 1.
> - **Figure 4.4** — grid import and self-sufficiency vs demand — in Section 4.4, Step 1.
> - All numbers are taken directly from your Results_Comparison.xlsx; re-running with new parameters
>   means updating Table 4.1 and the percentages in the text.
