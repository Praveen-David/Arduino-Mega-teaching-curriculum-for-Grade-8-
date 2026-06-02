# Capstone-C: Project Planning Template

## Arduino Mega Capstone — Planning Template
### Complete this template in Session 30. Keep it in your project binder.

> One copy per student, even when working in a team. Everyone should understand the full plan.

---

**Date:** _____________ **Session:** 30

---

## Section 1: Team & Project Identity

| Field | Fill In |
|-------|---------|
| **Project Title** (make it catchy!) | |
| **Team Name** | |
| **Team Member 1** | |
| **Team Member 2** | |
| **Team Member 3** (if applicable) | |
| **Project Manager** (keeps team on task) | |
| **Chief Builder** (leads circuit construction) | |
| **Lead Coder** (leads code writing) | |
| **Science Officer** (tracks hypothesis, data, science link) | |

---

## Section 2: The Scientific Question

Your science question must be:
- **Measurable** — it involves numbers, not just yes/no
- **Specific** — it says exactly what you are investigating
- **Testable** — you can actually run an experiment with your kit

**Draft your science question here:**

> ____________________________________________________________
> ____________________________________________________________
> ____________________________________________________________

**Check:** Does your question have a clear independent variable (what you change) and a dependent variable (what you measure)? If not, revise it before moving on.

---

## Section 3: Background Information

What do you already know about the science concept your project investigates? Write 3–5 sentences of background knowledge (from class, textbooks, or the internet):

____________________________________________________________
____________________________________________________________
____________________________________________________________
____________________________________________________________
____________________________________________________________

**Connection to Grade 8 curriculum** — which unit/topic does this connect to?

____________________________________________________________

---

## Section 4: Hypothesis

Write your hypothesis in the full "If… then… because…" format.

**IF** *(independent variable / condition you set)*:

____________________________________________________________

**THEN** *(dependent variable / predicted measurement result)*:

____________________________________________________________

**BECAUSE** *(scientific reasoning — link to the physics/biology/chemistry)*:

____________________________________________________________
____________________________________________________________

---

## Section 5: Variables

| Variable Type | Name/Description | How It Is Measured | Units |
|---------------|-----------------|-------------------|-------|
| **Independent** (what you change) | | | |
| **Dependent** (what you measure) | | | |
| **Controlled Variable 1** (kept constant) | | kept constant | — |
| **Controlled Variable 2** (kept constant) | | kept constant | — |
| **Controlled Variable 3** (kept constant) | | kept constant | — |

---

## Section 6: Materials List

List every component you plan to use. Include quantities and specific values where needed.

| # | Component | Quantity | Value / Specification | Notes |
|---|-----------|----------|----------------------|-------|
| 1 | Arduino Mega 2560 | 1 | — | Main microcontroller |
| 2 | USB-A to USB-B cable | 1 | — | Power + programming |
| 3 | Breadboard (full size) | 1 | 830 tie-points | |
| 4 | Jumper wires (M-M) | as needed | assorted colours | |
| 5 | Jumper wires (M-F) | as needed | assorted colours | |
| 6 | | | | |
| 7 | | | | |
| 8 | | | | |
| 9 | | | | |
| 10 | | | | |
| 11 | | | | |
| 12 | | | | |

**Teacher approval of materials list:** ______________________ Date: ___________

---

## Section 7: Circuit Plan

### 7A — Pin Assignment Table

List every component and which pin it connects to. Use this as your wiring reference during Build Day 1.

| Component | Pin / Rail | Wire Colour | Notes |
|-----------|-----------|-------------|-------|
| 5V power rail | 5V (Mega) | **red** | To breadboard (+) rail |
| GND rail | GND (Mega) | **black** | To breadboard (−) rail |
| Sensor 1: _____________ | | | |
| Sensor 2: _____________ | | | |
| Output 1: _____________ | | | |
| Output 2: _____________ | | | |
| LCD I2C SDA | **Pin 20** | blue | I2C data |
| LCD I2C SCL | **Pin 21** | yellow | I2C clock |
| LCD I2C VCC | 5V rail | **red** | |
| LCD I2C GND | GND rail | **black** | |
| Additional component: | | | |
| Additional component: | | | |

**Wire colour conventions (always follow these):**
- **Red = 5V / power**
- **Black = GND / ground**
- **Other colours = signal wires** (choose different colours for different signals)

---

### 7B — Circuit Sketch

Draw your circuit plan below. Label every component. Use red for 5V wires and black for GND wires.

```
  ┌───────────────────────────────────────────────────────────────┐
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │              [ Draw your circuit sketch here ]                │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  │                                                               │
  └───────────────────────────────────────────────────────────────┘
```

*Tip: Use coloured pencils — red for 5V, black for GND, different colours for each signal wire.*

---

## Section 8: Code Plan (Pseudocode)

Write your code plan in plain English. This becomes your guide when you type real code in Session 32.

**Libraries needed:**
- `#include <________________>` (for: )
- `#include <________________>` (for: )
- `#include <________________>` (for: )

**Pin definitions (list the `#define` statements you will need):**
```
#define [SENSOR1_PIN]    [pin number]   // description
#define [THRESHOLD_1]    [value]        // description
#define [OUTPUT_PIN]     [pin number]   // description
```

---

**SETUP section (what happens once at the start):**

```
SETUP:
  - Begin Serial at 9600 baud
  - Initialise _______________________ (sensor/library)
  - Initialise _______________________ (sensor/library)
  - Set pin _____ as OUTPUT (for _______________________)
  - Set pin _____ as OUTPUT (for _______________________)
  - Display startup message on LCD: "________________"
  - Print CSV header row to Serial Monitor
```

---

**MAIN LOOP section (what repeats over and over):**

```
LOOP (repeat forever):
  Step 1 — Read sensor(s):
    - Read ________________________ → store in variable _______
    - Read ________________________ → store in variable _______

  Step 2 — Control logic (if/else):
    - IF _________________________ THEN _______________________
    - ELSE _______________________________________________________

  Step 3 — Update LCD display:
    - Row 1: _________________________________________________
    - Row 2: _________________________________________________

  Step 4 — Log data to Serial Monitor:
    - Print: time, ____________, ____________, ____________

  Step 5 — Wait _____ milliseconds before next reading
```

---

## Section 9: Data Collection Plan

**What data will you record?**

| Column Heading | Variable | Unit | How Often |
|----------------|----------|------|-----------|
| Time | loop counter × delay | seconds | every reading |
| | | | |
| | | | |
| | | | |
| | | | |

**How many trials?** _____ **How long per trial?** _____ minutes

**How will you change the independent variable between trials?**
____________________________________________________________

**How will you record data?** (circle all that apply): Serial Monitor / Paper table / Both

**Your data table template** (draw the table you will use during data collection):

| Condition | Trial | __________ | __________ | __________ | State |
|-----------|-------|-----------|-----------|-----------|-------|
| Baseline | 1 | | | | |
| Baseline | 2 | | | | |
| Baseline | 3 | | | | |
| Condition 1 | 1 | | | | |
| Condition 1 | 2 | | | | |
| Condition 1 | 3 | | | | |

---

## Section 10: Safety Plan

List at least **3 safety considerations** specific to your project:

**1.** ____________________________________________________________

**2.** ____________________________________________________________

**3.** ____________________________________________________________

**Which components in your project are polarized?** (must be connected the right way)
____________________________________________________________

**Any components that could get hot or move unpredictably?**
____________________________________________________________

**Our "if something goes wrong" plan:**
____________________________________________________________

---

## Section 11: Timeline & Milestones

| Session | Date | Our Goal for That Day | Completed? |
|---------|------|----------------------|------------|
| Session 31 — Build Day 1 | | Build the hardware circuit | |
| Session 32 — Build Day 2 | | Write and test the code | |
| Session 33 — Build Day 3 | | Full integration test | |
| Session 34 — Build Day 4 | | Run the experiment; collect data | |
| Session 35 | | Presentation prep; peer feedback | |
| Session 36 | | **FINAL PRESENTATIONS** | |

**What is the single most important thing to complete by Session 33?**
____________________________________________________________

**If we fall behind schedule, what is our minimum viable project** (the core thing that must work)?
____________________________________________________________

---

## Section 12: Team Agreement

All team members sign below to confirm they understand the plan and have contributed to writing it:

**Team Member 1:** _________________________ Date: _____________

**Team Member 2:** _________________________ Date: _____________

**Team Member 3:** _________________________ Date: _____________

**Teacher reviewed and approved:** _________________________ Date: _____________

**Teacher comments / feedback on plan:**
____________________________________________________________
____________________________________________________________
____________________________________________________________

---

*Keep this planning template in your project binder. You will refer to it during every build session.*
