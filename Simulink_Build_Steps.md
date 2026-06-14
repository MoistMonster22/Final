# Simulink Build Steps (R2026a)
## EV_EMS_Model — the 5-block model that runs your thesis simulation

You only build this **once**. It is deliberately tiny: all the engineering logic lives in
`ems_core.m`, so the Simulink model just feeds data in, runs the brain each time step, loops
the battery state back, and logs the results. Budget ~30 minutes.

---

## Step 0 — Folder setup (2 min)

1. Make one folder, e.g. `EV_EMS_Project`.
2. Put **both** `ems_core.m` and `run_thesis_sim.m` inside it.
3. Open MATLAB and set that folder as the **Current Folder** (the address bar at the top).
   - Quick check: type `which ems_core` in the Command Window. It must print the path to
     your file. If it says "not found", you're in the wrong folder.

---

## Step 1 — Create the model (1 min)

1. MATLAB Home tab → **Simulink** → **Blank Model**.
2. **Save** it into your project folder as **`EV_EMS_Model.slx`** (exact name — the script
   looks for it).

---

## Step 2 — Set the solver (1 min)

1. In the model window: **Modeling** tab → **Model Settings** (the gear icon).
2. Under **Solver**:
   - **Type:** `Fixed-step`
   - **Solver:** `discrete (no continuous states)`
   - **Fixed-step size:** `0.25`
   - **Stop time:** `23.75`
3. **OK**.

---

## Step 3 — Add the 5 blocks (10 min)

Open the **Library Browser** (Simulink tab → Library Browser), or just double-click on blank
canvas and type the block name. Drag these in:

| # | Block (search this) | Library |
|---|---|---|
| 1 | **From Workspace** | Simulink / Sources |
| 2 | **Constant** | Simulink / Sources |
| 3 | **MATLAB Function** | Simulink / User-Defined Functions |
| 4 | **Unit Delay** | Simulink / Discrete |
| 5 | **To Workspace** | Simulink / Sinks |

Now set each block's parameters (double-click to open):

**1. From Workspace**
- **Data:** `Uts`
- **Sample time:** `0.25`
- Untick **Interpolate data**.
- **Form output after final data value by:** `Holding final value`

**2. Constant**
- **Constant value:** `MODE`
- (Signal Attributes tab, if present) leave defaults.

**3. MATLAB Function** — double-click to open the editor, delete everything, paste exactly:
```matlab
function [SoC_new, LOG] = EMS(U, MODE, SoC_prev)
    [SoC_new, LOG] = ems_core(U, MODE, SoC_prev);
end
```
Then close the editor. The block will automatically grow three input ports
(`U`, `MODE`, `SoC_prev`) and two output ports (`SoC_new`, `LOG`).

**4. Unit Delay**
- **Initial condition:** `SoC0`
- **Sample time:** `0.25`

**5. To Workspace**
- **Variable name:** `LOGOUT`
- **Save format:** `Array`
- **Sample time:** `-1`

---

## Step 4 — Wire it up (5 min)

Draw these connections (click an output port, drag to an input port):

1. **From Workspace** → MATLAB Function **`U`**
2. **Constant** → MATLAB Function **`MODE`**
3. **Unit Delay** output → MATLAB Function **`SoC_prev`**
4. MATLAB Function **`SoC_new`** → **Unit Delay** input
5. MATLAB Function **`LOG`** → **To Workspace**

The SoC feedback (3 and 4) forms a loop through the Unit Delay — that's correct and is what
lets the battery "remember" its charge from one step to the next. The Unit Delay breaks the
loop so there is no algebraic-loop error.

**Save the model** (Ctrl+S).

---

## Step 5 — Run everything (2 min)

1. Open `run_thesis_sim.m`.
2. (Optional now, required for the final report) replace the placeholder `EF` value, and
   later the tariff bands, with cited Australian figures.
3. Press **Run**.

You should see, in the Command Window, a six-row results summary, then a comparison table,
and four figure windows. The script also saves to your folder:
- `Results_Comparison.xlsx`
- `Fig_4_1_irradiance_effect.png`
- `Fig_4_2_EMS_effect.png`
- `Fig_4_3_tariff_effect.png`
- `Fig_4_4_demand_effect.png`

The **`balErr`** column in the summary should be ~0 (e.g. < 0.01). That is your energy-balance
validation: it confirms EV demand always equals PV + battery + grid. If it's ~0, the model is
internally consistent.

---

## Troubleshooting (most common first)

| Symptom | Cause / Fix |
|---|---|
| `Undefined function or variable 'ems_core'` | `ems_core.m` not in the current folder. `cd` to the project folder; run `which ems_core` to confirm. |
| From Workspace error: `Uts` undefined | Don't run the **model** on its own. Run **`run_thesis_sim.m`** — it creates `Uts`, `MODE`, `SoC0` before each simulation. |
| `MODE` / `SoC0` undefined | Same as above — these are set by the script via `assignin`. Run the script, not the bare model. |
| Algebraic loop error | The Unit Delay isn't in the feedback path. Make sure `SoC_new` goes **into** Unit Delay and Unit Delay output goes **into** the function's `SoC_prev` port — not a direct wire. |
| Dimension mismatch on `U` | `Uts` must come from a 96×4 matrix. The script builds it as `timeseries([G T EV Price], t)` — don't change that order. |
| `LOGOUT` is empty or wrong shape | To Workspace: **Save format = Array**, **Decimation = 1**, **Limit data points to last = inf**. |
| `simOut.LOGOUT` errors | Your To Workspace variable must be named exactly `LOGOUT`. The script also handles the timeseries case automatically. |
| Figures look flat / no PV | Check the input plots: run `plot(t,[G_sunny T EV_nom Price_tou])` after the script — PV should be zero at night and peak near noon. |

If you hit an error not in this table, copy the **full red error text** and send it to me — it
usually points to one specific block setting.

---

## What each block represents (for your Chapter 3 write-up)

- **From Workspace** = the input data layer (irradiance, temperature, EV demand, tariff).
- **MATLAB Function (`ems_core`)** = the PV model, the EMS controller, and the battery SoC
  update — the engineering core.
- **Unit Delay** = the battery's memory (state of charge carried between time steps).
- **To Workspace** = the logging/measurement layer.
- **Constant (MODE)** = the experimental switch that turns the smart controller off (0) or on
  (1) — this is the independent variable in your key Study 4.2.

This block-to-function mapping is exactly what you describe in **Chapter 3.1 (flowchart)** and
**3.2.1 (methodology approaches)**, and it's why the model is easy to validate: one controller
function, one feedback state, one balance check.
