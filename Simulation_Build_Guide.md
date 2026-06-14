# Simulation Build Guide
## Smart Solar-Driven Energy Management for EV Charging Stations

A practical, novice-friendly plan for building the MATLAB/Simulink model behind your
final thesis. Effective and defensible — without being over-engineered.

---

## 1. The Guiding Principle: Controlled Comparison

Your preliminary results couldn't prove the EMS added value because they changed two
things at once (weather **and** the controller). The fix is the foundation of this whole
design:

> **Build ONE base model. Run it as a series of one-variable-at-a-time experiments.
> Change a single input, hold everything else constant, and measure the effect.**

This is standard scientific method, it is easier to build than five separate scenarios,
and it maps directly onto the Chapter 4 template ("Effect of Key Investigation Parameter X
on the dependent variable"). Your simulation structure *becomes* your Chapter 4 structure.

---

## 2. System Architecture at a Glance

```
  INPUTS (96 points = 24h at 15-min steps)        SIMULINK MODEL              OUTPUTS (MATLAB)
  ────────────────────────────────────            ──────────────              ───────────────
   Irradiance  G(t)  [W/m2] ─┐
   Temperature T(t)  [degC]  ├──►  PV Generation ──► P_PV(t) ─┐
                             │       (Eq. 1)                   │
   EV demand   P_EV(t) [kW] ─┼─────────────────────────────►  ├──► EMS         ──► power flows ──► Energy (kWh)
   Tariff      Price(t)[$/kWh]┘                                │   Controller   ──► SoC trace   ──► Cost (AUD)
                                                               │   (rules)      ──► EMS mode    ──► CO2 (kg)
   SoC(t-1) ◄── Unit Delay ◄────────────── Battery SoC ◄───────┘                                ──► Grid peak (kW)
                                              (Eq. 4)          ▲                                ──► Plots
                                                               │
                                            Power Balance Check (validation)
```

Six conceptual blocks: **PV generation → EMS controller → battery SoC → power-balance
check → logging → post-processing in MATLAB.** You do *not* need switching converters,
inverter harmonics, or a diode-level PV model. Those add complexity without changing the
energy-management story, and they are out of scope for a rule-based EMS study.

---

## 3. The Four Controlled Experiments (= Chapter 4)

Build the base model once, then run these. Each row is a Chapter 4 sub-section.

| Section | Change THIS (independent) | Keep these FIXED | Measure (dependent) | What it proves |
|---|---|---|---|---|
| **4.1** | Irradiance: sunny vs cloudy | demand, tariff, EMS=ON | PV-to-EV, grid import, CO2 | Solar's contribution & weather sensitivity |
| **4.2** | **EMS mode: OFF vs ON** | weather, demand, tariff | cost, grid peak, self-sufficiency | **The EMS earns its keep** |
| **4.3** | Tariff: flat vs time-of-use | weather, demand, EMS=ON | cost savings, dispatch timing | EMS does tariff arbitrage |
| **4.4** | EV demand: low vs high | weather, tariff, EMS=ON | grid import, PV self-sufficiency | How the system scales with load |

**Section 4.2 is the heart of the thesis.** Same sunny day, same EV demand, controller
off vs on. Because you isolate the controller, any difference in cost or grid peak is
*caused by the EMS* — which is exactly the claim a marker wants to see proven, not asserted.

Measure the EMS in **cost and peak-grid reduction**, not raw kWh. A tariff-aware battery
holds charge for the expensive evening peak; that shows up as dollars saved and a lower
grid spike, even when total kWh barely moves.

---

## 4. The Base Model Components

### 4.1 Input data (one setup script)

Make all four input profiles arrays of length 96 (24 h x 4 quarters). Use a single
`setup_data.m` so every run starts from identical, consistent inputs.

- **Irradiance G(t):** a bell curve peaking near solar noon; zero at night.
- **Temperature T(t):** sinusoid, warmest mid-afternoon.
- **EV demand P_EV(t):** block loads — e.g. a modest daytime block and a larger
  early-evening block that deliberately overlaps the peak tariff (this is what makes the
  EMS interesting).
- **Tariff Price(t):** off-peak / shoulder / peak bands.

> Use **real Australian profiles where you can** (see Sources, item S6/S7). Synthetic
> profiles are fine for development, but cite real tariff bands and a real grid emission
> factor in the final report so the numbers are credible.

### 4.2 PV generation (Eq. 1 — temperature-corrected)

```matlab
function P_PV = pv_generation(G, T, PV)
% Temperature-corrected PV model (Saleem et al. 2024)
% P_PV = P_rated * (G/G_ref) * [1 + alpha*(T - T_ref)] * eta_mppt
    temp_factor = 1 + PV.alpha * (T - PV.T_ref);
    P_PV = PV.P_rated * (G / PV.G_ref) .* temp_factor * PV.eta_mppt;
    P_PV = max(0, P_PV);   % no negative power at night
end
```

Typical values: `P_rated = 10` kW, `G_ref = 1000` W/m^2, `T_ref = 25` degC,
`alpha = -0.004` /degC, `eta_mppt = 0.95`.

### 4.3 Battery SoC (Eq. 4 — with limits and efficiency)

```matlab
function SoC_new = battery_soc(SoC_old, P_ch, P_dis, dt, B)
% Energy-balance SoC update with round-trip efficiency and limits
    SoC_new = SoC_old ...
        + (P_ch * B.eta_ch * dt / B.E_cap) ...
        - (P_dis * dt / (B.eta_dis * B.E_cap));
    SoC_new = max(B.SoC_min, min(B.SoC_max, SoC_new));  % clamp 0.2-0.95
end
```

Typical values: `E_cap = 20` kWh, `SoC_min = 0.2`, `SoC_max = 0.95`,
`eta_ch = eta_dis = 0.92` (≈85% round-trip — this is the loss that punished your old
"EMS" case, so model it honestly).

### 4.4 EMS controller (the brain — rule-based, two modes)

The controller takes `P_PV, P_EV, SoC, Price, mode` and decides the power flows. Keep it
to a clear priority order — rule-based is the right level of complexity here (MPC is over
the top for this project; mention it only as future work).

```
MODE = OFF (baseline):
  1. PV -> EV directly (up to demand)
  2. Grid -> EV for any shortfall
  3. Surplus PV -> battery if room; otherwise curtail
  (battery never discharges strategically)

MODE = ON (smart, tariff-aware):
  PEAK price:     discharge battery -> EV first, grid only as last resort
  OFF-PEAK price: charge battery (PV + cheap grid), save it for the peak
  SHOULDER price: PV -> EV, store meaningful PV surplus, grid for the rest
```

That contrast — battery idle vs battery dispatched on price — is the entire value
proposition, and it's what Section 4.2/4.3 quantify.

### 4.5 Power-balance check (your validation)

At every step assert `P_EV ≈ PV->EV + Batt->EV + Grid->EV` (tolerance ~0.1 kW). If it
ever fails, your logic has a leak. This doubles as the Chapter 3.2.4 "validation approach."

---

## 5. Output Metrics (what Chapter 4 reports)

Compute these in `post_process.m` for every run and assemble one comparison table:

- **Energy:** PV-to-EV, Battery-to-EV, Grid-to-EV, Grid-to-Battery (kWh) via `trapz`.
- **PV self-sufficiency (%):** PV-served demand ÷ total demand.
- **Cost (AUD):** sum of grid power × tariff × dt.
- **Cost saving vs baseline (%):** the headline EMS number.
- **Peak grid demand (kW):** max grid draw — shows peak-shaving.
- **CO2 avoided (kg):** (grid-only energy − actual grid energy) × emission factor.
- **SoC trace:** to show the battery stayed within 20–95%.

Report differences as **percentages and absolute values** — the rubric explicitly rewards
conclusions "supported by data, percentages, or statistics."

---

## 6. Build Sequence (do it in this order)

1. **Inputs first.** Write `setup_data.m`, plot all four profiles, eyeball them for
   realism (PV zero at night, demand overlaps peak tariff). Don't proceed until they look right.
2. **PV only.** Add the PV function in Simulink, log `P_PV`, confirm the bell curve.
3. **Add the battery loop.** Insert the SoC function + a Unit Delay block to feed SoC back.
   Run with a trivial charge/discharge rule; confirm SoC stays in bounds.
4. **Add the full EMS.** Implement OFF and ON modes; add the power-balance check.
5. **Run the four experiments** (Section 3), save each result.
6. **Post-process.** Build the comparison table and the figures, verify energy balance,
   hand-check one scenario's cost by calculator.

Stages 1–3 are where novices get stuck; once the SoC feedback loop runs cleanly, the rest
is fast. Build and test each stage before adding the next — never wire the whole thing and
debug at the end.

---

## 7. Sources for the Report (map each design choice to a citation)

Put this in **Chapter 3 (Methodology)** so every modelling decision is justified. These
are drawn from your existing reference list, so they're already in scope.

| Design decision | Cite | Justifies |
|---|---|---|
| Temperature-corrected PV model (Eq. 1), MPPT as efficiency | **Saleem et al. (2024)** | PV output as f(irradiance, temperature, MPPT) |
| BESS need + SoC limits (20–95%), round-trip efficiency | **Alrubaie et al. (2023)** | Storage smooths PV intermittency; SoC/SoC_min/SoC_max as EMS variables |
| Need for storage because PV is intermittent | **Kumar et al. (2019)** | ESS meets demand when PV is short, stores surplus when PV is high |
| MATLAB/Simulink as the simulation platform | **Kumar et al. (2019); Saleem et al. (2024)** | Both implement PV-BESS-EV systems in MATLAB/Simulink |
| Tariff-aware EMS dispatch, peak-shaving, cost optimisation | **Cheikh-Mohamad et al. (2023)** | EMS coordinates PV/storage/grid to minimise charging cost |
| Power-flow / energy-balance modelling, grid-interaction metrics | **Hermans et al. (2024)** | EV charging as a controllable load in a grid-connected microgrid |
| Rule-based vs optimisation-based EMS (and MPC as future work) | **Hermans et al. (2024); Saleem et al. (2024)** | Positions your rule-based controller and the MPC upgrade path |

**Two numbers you must source from current Australian data (don't invent them):**

- **S6 — Time-of-use tariff bands (AUD/kWh):** use a real Victorian retailer's TOU plan or
  AEMO/AER published rates, and cite it.
- **S7 — Grid emission factor (kg CO2/kWh):** use Australia's official National Greenhouse
  Accounts Factors for the relevant state, and cite it.

> I can look up current, citable figures for S6 and S7 if you'd like — just ask.

---

## 8. What to Keep Out (so it stays "not over the top")

Mention these only as **future work / recommendations** (Chapter 5), not in the model:
detailed diode-level PV physics, real P&O/INC MPPT converter, inverter/grid harmonics,
battery ageing and thermal models, V2G communication, stochastic EV arrivals, and MPC
optimisation. Listing them as future work actually scores points (it shows you know the
model's boundaries) — building them does not.
