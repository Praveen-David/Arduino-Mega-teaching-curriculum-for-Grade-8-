# Week 15, Session 30 — Project Planning & Hypothesis

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Planning / Scientific Method (no circuit power-on today)

## Learning Objectives
- **Write** a testable hypothesis in the "If… then… because…" format that links their device to a science concept.
- **Identify** the independent, dependent, and controlled variables in their capstone experiment.
- **Complete** the full Planning Template including materials list, circuit plan sketch, pseudocode outline, and safety plan.
- **Explain** how the scientific method applies to an engineering design project.

**Science Curriculum Link:** Scientific Method & Experimental Design — Students apply formal hypothesis writing and variable identification (Grade 8 science core skill) to a real engineering challenge, reinforcing that good science requires deliberate planning before any experiment begins.

## Materials Checklist (per team)
- [ ] Completed exit ticket from Session 29 (reference for today)
- [ ] Printed copy of `Capstone-C-Planning-Template.md` (1 per student — all fill in their own)
- [ ] Printed copy of `Capstone-B-Project-Idea-Bank.md` (for reference)
- [ ] Pencils and rulers (for circuit sketch)
- [ ] Coloured pencils or markers (red = 5V, black = GND, other = signal wires)
- [ ] Component inventory card (to confirm parts available)
- [ ] Notebooks / project binders
- [ ] Whiteboard / large paper for teacher to model hypothesis writing
- [ ] Optional: Tinkercad Circuits open on the computer for circuit planning

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Write this on the board:

> "A plant-watering system turns a pump on when soil is dry."
> — Is this a science project or just a gadget?

Take 3 quick answers. Expected student responses: "just a gadget," "depends on what question you're asking," "both if you measure something."

**Bridge (say this):**
> "The gadget becomes a science project the moment you ask a testable question and collect data to answer it. Today, we turn your project idea into a real science experiment — on paper, before you touch a single wire."

---

### 5–15 min — Direct Instruction: Scientific Method for Engineers

Write the scientific method steps on the board:
1. **Observation / Background** — what do you already know?
2. **Question** — what do you want to find out? (must be measurable)
3. **Hypothesis** — your prediction (If… then… because…)
4. **Variables** — independent (what you change), dependent (what you measure), controlled (what you keep the same)
5. **Experiment / Procedure** — how will you test it?
6. **Data Collection** — what numbers will you record?
7. **Analysis & Conclusion** — what do the results tell you?

**Hypothesis formula (write on board, model with an example):**

```
IF [independent variable is changed / condition is set],
THEN [dependent variable result],
BECAUSE [science reasoning].
```

**Example (model this for students):**

Project: Smart Thermostat
> "IF the ambient temperature rises above 28°C, THEN the fan motor speed will increase proportionally, BECAUSE the DHT11 sensor reading will trigger higher PWM values in the code, demonstrating that heat energy in the air can be measured and converted to a mechanical response."

Point out: the hypothesis connects the sensor → the code → the science concept. That is the target.

**Variables — model with the same example:**
- Independent: ambient temperature
- Dependent: fan motor speed (measured as PWM value or RPM count)
- Controlled: room humidity, distance from heat source, code threshold values

**Common mistake to address:**
> "A hypothesis is NOT 'I think my project will work.' That's a hope, not a hypothesis. A hypothesis predicts *what will happen to the data* under a specific condition."

Allow 2 minutes for student questions.

---

### 15–35 min — Hands-On: Complete the Planning Template

**Distribute `Capstone-C-Planning-Template.md`.** Students work in teams but each person fills in their own copy (for individual accountability).

Walk through each section with the class, then give work time:

**Section 1–3 (5 min): Title, team, science question**
- Circulate. Science questions must be *measurable* — redirect vague questions ("Does my greenhouse work?" → "How does increasing ambient light intensity affect the internal temperature of a closed greenhouse model?").

**Section 4 (5 min): Hypothesis & variables**
- Most important section. Check every team before they move on.
- Common issue: students conflate the independent and dependent variables. Remind them: "Independent = what you set or change; Dependent = what you measure in response."

**Section 5 (5 min): Materials list**
- Teams list every component they plan to use with quantities and pin assignments.
- Cross-reference against the kit inventory. Flag if a component is missing.

**Section 6 (5 min): Circuit plan sketch**
- Rough pencil sketch with a pin table. Does NOT need to be perfect — it is a plan.
- Remind: red = 5V, black = GND. Label every component.
- Tinkercad Circuits can be used for teams who prefer digital planning.

**Section 7 (5 min): Code plan (pseudocode)**
- English-language pseudocode, not actual Arduino code yet.
- Example format:
  ```
  SETUP: initialise LCD, DHT11, servo, LED
  LOOP:
    read DHT11 temperature
    IF temp > threshold THEN move servo to open position, turn on LED
    ELSE close servo, LED off
    display temp on LCD every 2 seconds
    print to Serial every 5 seconds
  ```

**Section 8–9 (5 min): Data plan & safety plan**
- Data plan: what will they record, how often, and in what format?
- Safety plan: every team writes at least 2 safety considerations for their specific build.

Teacher circulates continuously. Use the "help queue" — do not let teams wait more than 2 minutes without checking on them.

---

### 35–42 min — Testing & Debugging (Plan Validation)

Each team passes their planning template to the team on their left for a **60-second quick review**:
- Does the hypothesis have all three parts (If / Then / Because)? ✔/✗
- Can you identify the independent and dependent variable? ✔/✗
- Does the materials list seem complete? ✔/✗

Reviewing team writes one suggestion on a sticky note and sticks it to the plan.

Plans are returned. Teams read the feedback and make any immediate updates.

**Teacher checks:** Walk the room. Any team whose hypothesis is still vague or whose circuit plan is missing critical components should be flagged for a 1:1 conversation at the start of Session 31.

**What success looks like:**
- Every team has a complete planning template with a testable hypothesis.
- Independent and dependent variables are correctly identified.
- A rough circuit sketch with labeled components exists.
- A pseudocode outline covers the main logic of the project.

---

### 42–45 min — Reflection / Exit Ticket

**Written exit ticket (in project binders):**

1. Write your hypothesis in the "If… then… because…" format:
   `____________________________________________________________`
   `____________________________________________________________`

2. Your independent variable: _________________ Dependent variable: _________________

3. What is ONE change you made to your plan based on the peer review?
   `____________________________________________________________`

4. What is the most important thing you need to figure out before Build Day 1?
   `____________________________________________________________`

**Teacher action:** Collect or photograph planning templates at the end of Session 30. Review them before Session 31. Teams whose plans have significant problems should be pulled aside at the start of Session 31 for a quick redirect conversation.

---

## 3. Circuit Diagram

No circuit is powered on today. Students produce a **hand-drawn or Tinkercad circuit plan** as part of the planning template.

```
  [ Today: PLAN your circuit on paper. ]
  [ No wiring. No power. Boards stay in the box. ]

  ╔══════════════════════════════════════════════════╗
  ║  CIRCUIT PLANNING GUIDE (use this as a model)   ║
  ║                                                  ║
  ║  1. Draw your Arduino Mega outline               ║
  ║  2. List every component you will use            ║
  ║  3. Assign each one a pin (check the pin card)   ║
  ║  4. Sketch wires: RED = 5V, BLACK = GND          ║
  ║  5. Check: does every sensor/module have power   ║
  ║     AND ground AND a signal wire?                ║
  ║  6. Check: are any PWM pins needed for ~outputs? ║
  ║  7. Are SDA (pin 20) / SCL (pin 21) used for LCD?║
  ╚══════════════════════════════════════════════════╝

  PIN PLANNING TABLE (teams fill in their own version):
  ┌──────────────────┬──────────┬──────────────────────┐
  │ Component        │ Pin #    │ Wire Color / Notes   │
  ├──────────────────┼──────────┼──────────────────────┤
  │ Sensor 1: _____  │ ______   │ ____________________  │
  │ Sensor 2: _____  │ ______   │ ____________________  │
  │ Output 1: _____  │ ______   │ ____________________  │
  │ Output 2: _____  │ ______   │ ____________________  │
  │ LCD SDA          │ 20       │ blue (I2C data)       │
  │ LCD SCL          │ 21       │ yellow (I2C clock)    │
  │ 5V rail          │ 5V       │ red                   │
  │ GND rail         │ GND      │ black                 │
  └──────────────────┴──────────┴──────────────────────┘
```

[Google image search: "Arduino Mega 2560 breadboard planning template"]

---

## 4. Arduino Code

No code is written or uploaded in this session. Students write **pseudocode** on their planning template.

```cpp
// =============================================================
// SESSION 30: PROJECT PLANNING & HYPOTHESIS
// No uploadable code today — you are writing your CODE PLAN.
//
// SCIENCE FOCUS: Scientific Method
// A hypothesis predicts what your DATA will show.
// Before you code, plan what the code needs to DO.
//
// PSEUDOCODE FORMAT — write this in plain English first:
// =============================================================

/*
  EXAMPLE PSEUDOCODE (do not copy — write your own!):

  LIBRARIES: include DHT.h, Servo.h, LiquidCrystal_I2C.h

  CONSTANTS:
    define pin numbers for each component
    define threshold values (e.g., TEMP_HIGH = 28)

  SETUP:
    begin Serial at 9600 baud
    begin LCD
    initialise DHT sensor
    attach servo to its pin
    set LED pins as OUTPUT

  MAIN LOOP (repeat forever):
    1. Read sensor(s) — store in variables
    2. Compare values to thresholds using if/else
    3. Control outputs based on comparisons (servo, LED, buzzer)
    4. Update LCD display with current readings
    5. Print data to Serial Monitor (for logging)
    6. Wait a short delay before repeating
*/

// >>> YOUR TEAM'S PSEUDOCODE (write it in your planning template):
// =============================================================
```

---

## 5. Student Worksheet

---
### Worksheet — Session 30: Project Planning & Hypothesis

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### What I Already Know — Warm-Up Questions
*(Answer individually)*

1. What is the difference between the **independent variable** and the **dependent variable** in an experiment?
   `____________________________________________________________`
   `____________________________________________________________`

2. Write the scientific method steps in order (from memory):
   1. _________________________ 2. _________________________
   3. _________________________ 4. _________________________
   5. _________________________ 6. _________________________

3. In pseudocode, what does an `if/else` statement do? Describe it in everyday English:
   `____________________________________________________________`

---

#### Our Science Question

Write the question your project will investigate. It must be:
- Measurable (involves numbers)
- Specific (not "does it work?" but "how does X affect Y?")

**Our science question:**
`____________________________________________________________`
`____________________________________________________________`

---

#### Our Hypothesis

Fill in the formula:

**IF** `__________________________________________________`

**THEN** `__________________________________________________`

**BECAUSE** `__________________________________________________`
`__________________________________________________`

---

#### Variables

| Variable Type | Description | How We Will Measure It |
|---------------|-------------|----------------------|
| Independent (what we change) | | |
| Dependent (what we measure) | | |
| Controlled Variable 1 | | kept constant |
| Controlled Variable 2 | | kept constant |
| Controlled Variable 3 | | kept constant |

---

#### Materials List

| Component | Quantity | Where Connected (pin / rail) |
|-----------|----------|------------------------------|
| Arduino Mega 2560 | 1 | — |
| Breadboard | 1 | — |
| | | |
| | | |
| | | |
| | | |
| | | |
| | | |

---

#### Circuit Sketch

*(Draw your rough circuit plan here — label every component, color wires red/black/other)*

**Sketch area (approx. 12 cm × 10 cm):**

```
  ┌─────────────────────────────────────────────────────────┐
  │                                                         │
  │                                                         │
  │                                                         │
  │               [ draw your circuit here ]                │
  │                                                         │
  │                                                         │
  │                                                         │
  └─────────────────────────────────────────────────────────┘
```

---

#### Pseudocode Plan

Write your code plan in plain English (use the format from the teacher's example):

```
LIBRARIES:

CONSTANTS / PINS:

SETUP:

MAIN LOOP:
  1.
  2.
  3.
  4.
  5.
```

---

#### Data Collection Plan

| Question | Your Answer |
|----------|-------------|
| What data will you record? | |
| How often will you take readings? | |
| How many trials / how long will you run the test? | |
| How will you record data? (Serial Monitor / table on paper / both) | |
| What format will your data table have? (draw it below) | |

**Data table sketch:**

| Column 1: | Column 2: | Column 3: | Column 4: |
|-----------|-----------|-----------|-----------|
| | | | |
| | | | |

---

#### Safety Plan

List at least **two** safety considerations specific to YOUR project build:

1. `____________________________________________________________`
2. `____________________________________________________________`
3. *(bonus)* `____________________________________________________________`

---

#### Predict!

1. What do you predict the data will show? (Connect to your hypothesis):
   `____________________________________________________________`

2. What is one result that would *surprise* you? What might cause it?
   `____________________________________________________________`

3. If your device doesn't work on presentation day, what parts of the experiment would still have scientific value?
   `____________________________________________________________`

---

#### What Did You Notice? (After peer review)

1. What feedback did the other team give you?
   `____________________________________________________________`

2. What change did you make to your plan as a result?
   `____________________________________________________________`

3. What is the biggest unknown you still need to resolve?
   `____________________________________________________________`

---

#### Science Connection

> "Your planning template is exactly what a professional engineer writes before starting a project. Real-world examples include NASA's James Webb Space Telescope requirements document, a pharmaceutical trial protocol, or a civil engineer's bridge design specification. In every case, the plan comes before the build — because fixing a mistake on paper is 1000× faster than fixing it in metal."

How does writing a plan before building help reduce errors?
`____________________________________________________________`
`____________________________________________________________`

---

#### Challenge Extension
*(For teams who finish early)*

Search for the **datasheet** (technical specification sheet) for one of the sensors in your project (e.g., DHT11, HC-SR04). Find and record:
- The sensor's measurement range (minimum and maximum values)
- Its accuracy (±)
- Its operating voltage

How will these limits affect what data your project can collect?

---

## 6. Safety Notes

**Today's session has no electronics powered on.** Use this time to plan safe build practices:

- **Wire planning:** When sketching circuits, actively think about where shorts could occur. Remind students: 5V must never connect directly to GND.
- **Polarity check in plan:** While drawing, label the + and − pins on polarized components (LEDs, electrolytic capacitors, DHT11 VCC/GND).
- **Servo power note:** If the project uses a servo, note in the safety plan that the servo GND must connect to the Arduino GND, not be left floating.
- **DHT11 reminder:** DHT11 requires a 10 kΩ pull-up resistor on the data line in some configurations — plan for this in the circuit sketch.
- **Build → Check → Power on mantra:** Reinforce this today so it is automatic on build days.

| Situation | Action |
|-----------|--------|
| Circuit plan shows 5V going directly to GND (student error in sketch) | Catch it now — draw an X through it and correct before Build Day 1 |
| Materials list includes a component not in the kit | Teacher checks spares bin; if unavailable, team adjusts plan now |
| Team is behind on planning — template not complete | Provide a partially pre-filled template as a scaffold (see Differentiation) |

---

## 7. Assessment Rubric

**Formative Check (not graded in isolation — contributes to capstone planning score) — Teacher Look-Fors:**

| Look-For | Observed? | Notes |
|----------|-----------|-------|
| Hypothesis follows "If… then… because…" with all three parts present | | |
| Independent and dependent variables correctly identified | | |
| Materials list is complete and realistic (no impossible components) | | |
| Circuit sketch has labeled components and correct power/GND rails | | |
| Pseudocode covers the main logical steps of the project | | |
| Safety plan identifies at least 2 project-specific considerations | | |

**Grading note:** The **completed Planning Template** (Capstone-C) is a graded deliverable. It is evaluated using the rubric in `Capstone-F-Final-Presentation-Rubric.md`, Section: Engineering Design. Collect templates at the end of Session 30 (or photograph) to give feedback before Build Day 1.

---

## 8. Differentiation

### Support
- Provide a **partially pre-filled Planning Template** with the hypothesis formula already written in outline form and example text shown in grey/italics.
- Offer a **variable sort activity** — give students cards with examples of variables and ask them to sort into "independent," "dependent," and "controlled" piles before writing their own.
- Allow teams to use a project from the Idea Bank and adapt it, rather than designing from scratch — this gives them a working circuit sketch to modify rather than create from nothing.
- Pair students strategically: a student strong in science writes the hypothesis section while a student strong in electronics draws the circuit sketch — then they explain their sections to each other.

### Extension
- Challenge the team to write **two hypotheses** — one for a planned experiment and one for a "what if?" scenario (e.g., "What if the temperature exceeds 40°C — what would we observe?").
- Ask the team to identify **potential sources of error** in their experiment and describe how they will minimise each one.
- Have the team research the scientific principle behind their sensor (e.g., the physics of how an LDR changes resistance with light intensity) and add a "Background Science" section to their planning template.

### Visual / Kinesthetic Accommodations
- Provide a **printed pin card** with the Mega pinout — students can physically match components to pins by circling them on the card.
- Allow students to lay out components on the (unpowered) breadboard while planning the circuit sketch — "feel the layout" before drawing it.
- Post a large "IF… THEN… BECAUSE…" poster on the wall with a filled-in example for reference throughout the session.
- For students with writing difficulties, allow voice-recorded pseudocode (they narrate their logic into their phone/a recorder).
