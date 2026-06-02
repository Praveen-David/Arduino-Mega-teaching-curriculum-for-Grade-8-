# Week 17, Session 34 — Build Day 4: Data Collection & Refinement

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Build Day — Experiment / Data Collection / Iteration

## Learning Objectives
- **Execute** a planned data-collection experiment using their capstone device, following the scientific method.
- **Record** quantitative data in an organised table and identify trends or patterns.
- **Evaluate** the data against their hypothesis — does the evidence support, partially support, or refute the prediction?
- **Iterate** on the device (code or hardware adjustments) based on data analysis findings.

**Science Curriculum Link:** Scientific Method — Data Collection & Analysis — Students run the "experiment" phase of their project, connecting directly to Grade 8 science process skills: controlled experimentation, data recording, pattern identification, and drawing evidence-based conclusions. This is the session where the project becomes a genuine science investigation.

## Materials Checklist (per team)
- [ ] Arduino Mega 2560 + USB cable
- [ ] Fully integrated project (from Sessions 31–33)
- [ ] Laptop with Arduino IDE and final project code
- [ ] Data collection table (from Planning Template, or new table if revised)
- [ ] Build Log template (Day 4 entry)
- [ ] A clock / timer visible to all students (phone, board clock, or teacher's timer on projector)
- [ ] Optional: graph paper for hand-drawn data plots
- [ ] Optional: thermometer (to verify DHT11 readings independently)
- [ ] Optional: ruler (to verify HC-SR04 distance readings independently)
- [ ] Notebooks / project binders

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

**Ask the class — hands up:**
> "Scientists often collect data and find something completely different from what they predicted. Is that a success or a failure?"

Take 4–5 responses. Guide toward: "It's a success if it teaches you something new. A hypothesis being wrong is still good science — as long as you recorded your method and data honestly."

**Quote to share (write on board):**
> *"If your experiment needs statistics to tell you whether you have a result, you should have done a better experiment." — Ernest Rutherford*

**Bridge (say this):**
> "That quote is about clarity — if your device works, the data will tell a clear story. Today we run the experiment. Collect as much real, clean data as you can. Then we analyse it. The numbers will show you whether your hypothesis was right — and that is the most exciting moment in science."

---

### 5–15 min — Direct Instruction: Running a Fair Experiment

**Key concepts to cover (5 minutes, keep it tight):**

**Controlled variables matter.**
> "If you're testing whether temperature affects how your servo responds, keep everything else the same — same room, same time gap between readings, same threshold settings. If you change two things at once, you can't know which one caused the result."

**Multiple trials = more reliable data.**
> "Scientists don't run an experiment once. They run it 3–5 times and look for consistency. Today, aim for at least 5 readings per condition."

**Record what actually happened — not what you wanted to happen.**
> "If the data doesn't match your hypothesis, record it honestly. Then try to explain WHY it's different. That explanation could be the most scientifically interesting part of your project."

**Data collection checklist (write on board):**
1. Set your Serial Monitor baud rate to 9600 (matches your `Serial.begin(9600)`)
2. Click "Clear" on the Serial Monitor before each new trial
3. Record the time of each reading (the loop counter or a clock)
4. Run for at least 5 minutes of stable data before changing a condition
5. Change ONE variable (e.g., temperature), record for another 5 minutes
6. Copy key data from Serial Monitor into your paper data table

**For projects without a clear "condition change" (continuous monitoring projects):**
> "Record at regular intervals over a 15-minute period. Look for trends over time — does the system maintain stable conditions? What is the range of variation?"

---

### 15–35 min — Hands-On: Data Collection Experiment

**Teams run their experiments. The teacher facilitates, does not intervene unless there is a safety issue or a fundamental problem.**

**Session structure teams should follow:**

| Time | Activity |
|------|----------|
| 0–2 min | Final hardware/code check — upload code, confirm Serial Monitor working |
| 2–7 min | **Baseline trial** — record 5 readings under "normal" conditions |
| 7–12 min | **Condition 1 trial** — change the independent variable; record 5 readings |
| 12–17 min | **Condition 2 trial** (if applicable) — change again; record 5 readings |
| 17–20 min | **Analysis** — copy data to paper table, look for patterns |

**If integration problems from Session 33 are still outstanding**, allocate the first 7 minutes to fixing those before data collection starts. Teams cannot collect meaningful data from a broken system.

**What to look for while circulating:**
- Are teams recording specific numbers, or vague descriptions? (Numbers only!)
- Are they recording units with every value? (Degrees C, not just "warm")
- Are they changing one variable at a time?
- Are they cross-checking sensor readings? (e.g., does the DHT11 temperature match a hand-held thermometer?)

**Teacher prompt for teams who are bored during stable readings:**
> "While the data is collecting, start writing your analysis. What pattern do you see forming? Does it support your hypothesis? If so, why? If not, what might explain the difference?"

---

### 35–42 min — Testing & Debugging: Iteration

Once data is collected, teams make **one final refinement** to improve their project for the presentation. This could be:
- Adjusting a threshold value based on what the data revealed
- Adding a label to the LCD display that was missing
- Making the Serial log format cleaner (better labels, consistent spacing)
- Fixing a cosmetic issue (loose wires, untidy breadboard layout)
- Improving the data collection interval (too fast → noisy, too slow → misses changes)

**Guide teams to prioritise improvements that help the SCIENCE, not just aesthetics.**

Say: "Ask yourself: does this change make my project better at answering my science question? If yes, do it. If it's just cosmetic, leave it for the presentation poster."

---

### 42–45 min — Reflection / Exit Ticket

**In project binders:**

1. Write 2 data points that SUPPORT your hypothesis:
   `____________________________________________________________`

2. Write 1 data point or observation that DOES NOT fully support your hypothesis (or "all data supports it" if genuinely true — be honest):
   `____________________________________________________________`

3. One refinement you made to your project today and why:
   `____________________________________________________________`

4. Rate your project on a scale of 1–5 for "ready to present" (1 = lots more work needed, 5 = ready now):
   Circle: **1 — 2 — 3 — 4 — 5**

---

## 3. Circuit Diagram

The circuit is finalised from Sessions 31–33. Any last refinements made today should be minor.

```
  BUILD DAY 4: CIRCUIT COMPLETE — DATA COLLECTION MODE
  ======================================================

  The circuit should be stable and unchanged from Session 33.

  DATA COLLECTION SETUP:
  ┌────────────────────────────────────────────────────────┐
  │                                                        │
  │   [Arduino Mega]                                       │
  │       │ USB ──────────────────────► [Laptop]           │
  │       │                                Arduino IDE     │
  │       │                                Serial Monitor  │
  │       │                                (data recording)│
  │       │                                                │
  │   [Sensors] ─────────────────────────────────────────  │
  │       │                                                │
  │       ▼                                                │
  │   SERIAL LOG FORMAT (example):                        │
  │   Time(s), Temp(C), Humidity(%), Light, Dist(cm)      │
  │   0,     23.5,     45.0,        512,   35             │
  │   2,     23.6,     45.0,        508,   35             │
  │   4,     28.2,     43.1,        505,   35  ← threshold│
  │   6,     28.5,     43.0,        503,   35             │
  │                                                        │
  │   TIP: Copy Serial Monitor output to a text file      │
  │   (Ctrl+A, Ctrl+C, paste to Notepad/TextEdit)        │
  │   This is your official data record.                  │
  └────────────────────────────────────────────────────────┘

  INDEPENDENT VERIFICATION:
  - Use a room thermometer to cross-check DHT11 temp readings
  - Use a ruler to cross-check HC-SR04 distance readings
  - Record both in your data table (DHT11 reading vs. thermometer reading)
```

[Google image search: "Arduino Serial Monitor data collection experiment logging"]

---

## 4. Arduino Code

The project code is complete from Sessions 32–33. The additions below focus on **data quality improvements** — making the Serial log more useful for analysis.

```cpp
// =============================================================
// CAPSTONE PROJECT — BUILD DAY 4: DATA COLLECTION ENHANCEMENTS
// Add these improvements to your final integrated code.
// =============================================================

// ---- IMPROVEMENT 1: CSV FORMAT WITH HEADER ROW --------------
// In your setup(), print the column headers first.
// This makes copying to Excel/Sheets much easier.

void setup() {
  Serial.begin(9600);
  // ... other initialisation code ...

  // Print CSV header row
  Serial.println("Time_s,Temp_C,Humidity_%,Light_0to1023,Distance_cm,State");
  // "State" column will show the system's condition: NORMAL/WARM/HOT
}

// ---- IMPROVEMENT 2: ADD A STATE LABEL TO EACH ROW -----------
// Replace the simple Serial.print loop with a labelled version:

String systemState = "NORMAL";   // global variable for current state

void loop() {
  // ... sensor readings and control logic ...

  // Determine state label
  if (temperature > TEMP_HIGH) {
    systemState = "HOT";
  } else if (temperature > TEMP_HIGH - 3) {
    systemState = "WARM";          // within 3°C of threshold = "WARM"
  } else {
    systemState = "NORMAL";
  }

  // Print CSV-formatted data row
  Serial.print(loopCounter * 2);  // time in seconds
  Serial.print(",");
  Serial.print(temperature, 1);   // 1 decimal place
  Serial.print(",");
  Serial.print(humidity, 0);
  Serial.print(",");
  Serial.print(lightLevel);
  Serial.print(",");
  Serial.print(distanceCm);
  Serial.print(",");
  Serial.println(systemState);    // state label at end of row

  loopCounter++;
  delay(2000);
}

// ---- IMPROVEMENT 3: MANUAL TRIGGER BUTTON FOR TRIAL MARKER --
// Connect a pushbutton to pin 2.
// When pressed, print a "--- TRIAL MARKER ---" line in the log.
// This helps you identify when you changed a condition.

#define BUTTON_PIN 2

void setup() {
  // ... other setup code ...
  pinMode(BUTTON_PIN, INPUT_PULLUP);   // button with internal pull-up
}

void loop() {
  // Check if button was pressed (LOW when pressed with INPUT_PULLUP)
  if (digitalRead(BUTTON_PIN) == LOW) {
    Serial.println("--- TRIAL MARKER PRESSED ---");
    delay(300);                         // debounce
  }
  // ... rest of loop ...
}

// >>> TRY CHANGING THIS:
// Change the delay(2000) to delay(1000) for faster data collection,
// or delay(5000) for a slower environmental experiment over a longer
// time period. What interval makes most sense for your science question?

// =============================================================
// ===== CHALLENGE =====
//
// 1. Calculate and print a RUNNING AVERAGE of temperature
//    over the last 10 readings. Does this make the data
//    look smoother than individual readings?
//
// 2. Add MIN and MAX tracking: keep variables for the highest
//    and lowest temperature seen since startup, and print them
//    to the LCD as a ticker.
//
// 3. Save data in a format that can be graphed in Google Sheets:
//    copy your Serial Monitor output and paste into a spreadsheet.
//    Use Insert → Chart to create a line graph. Include this
//    graph in your final presentation.
// =============================================================
```

---

## 5. Student Worksheet

---
### Worksheet — Session 34: Build Day 4 — Data Collection & Refinement

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### What I Already Know — Warm-Up Questions

1. What are **controlled variables** and why do they matter in an experiment?
   `____________________________________________________________`

2. What is the difference between **accurate** data and **precise** data?
   `____________________________________________________________`

3. If your hypothesis says "IF temperature rises above 28°C THEN the servo opens," but the servo opens at 26°C in testing — does this support or refute your hypothesis? Explain:
   `____________________________________________________________`

---

#### Our Science Question (copy from Planning Template)

`____________________________________________________________`
`____________________________________________________________`

#### Our Hypothesis (copy from Planning Template)

IF `____________________________________________________________`
THEN `____________________________________________________________`
BECAUSE `____________________________________________________________`

---

#### Build It — Data Collection Procedure

Describe your exact procedure in numbered steps (another team should be able to repeat your experiment):

1. `____________________________________________________________`
2. `____________________________________________________________`
3. `____________________________________________________________`
4. `____________________________________________________________`
5. `____________________________________________________________`

**Independent variable:** `_______________________________________`
**Dependent variable:** `_______________________________________`
**Controlled variables (at least 3):**
- `____________________________________________________________`
- `____________________________________________________________`
- `____________________________________________________________`

---

#### Predict!

Before running your experiment:

1. At what temperature / light level / distance do you predict the system will change state?
   `____________________________________________________________`

2. How do you expect your data to trend over time?
   `____________________________________________________________`

---

#### Observe / Data Table — Raw Data

**Trial 1 — Baseline conditions (normal / unmodified environment):**

| Reading # | Time (s) | Value 1 (______) | Value 2 (______) | Value 3 (______) | System State |
|-----------|----------|------------------|------------------|------------------|--------------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

**Trial 2 — Modified condition ([describe what you changed]):**

| Reading # | Time (s) | Value 1 (______) | Value 2 (______) | Value 3 (______) | System State |
|-----------|----------|------------------|------------------|------------------|--------------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

**Trial 3 (if time allows) — [describe condition]:**

| Reading # | Time (s) | Value 1 (______) | Value 2 (______) | Value 3 (______) | System State |
|-----------|----------|------------------|------------------|------------------|--------------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

---

#### Analysis — What Does the Data Show?

1. **Calculate the average** for your dependent variable under each condition:
   - Trial 1 average: _____________
   - Trial 2 average: _____________
   - Trial 3 average: _____________

2. **Describe the trend** you see in the data (increase, decrease, stable, variable):
   `____________________________________________________________`

3. At what value did the system **change state** (cross the threshold)? Does this match the threshold in your code?
   Expected threshold: _______ Observed threshold: _______

4. Did the system respond **consistently** — i.e., did it respond the same way every time the condition was met?
   `____________________________________________________________`

---

#### Conclusion

Answer your science question using your data:

> "Based on the data collected, [describe what the data shows — use specific numbers]..."

`____________________________________________________________`
`____________________________________________________________`

**Does this SUPPORT, PARTIALLY SUPPORT, or REFUTE your hypothesis?** (circle one)

Explain: `____________________________________________________________`
`____________________________________________________________`

**One source of error** in your experiment and how it might have affected the results:
`____________________________________________________________`

**If you were to run this experiment again, what would you change?**
`____________________________________________________________`

---

#### What Did You Notice?

1. What was the most interesting result you collected today?
   `____________________________________________________________`

2. Was there anything in the data that surprised you?
   `____________________________________________________________`

3. What does the data tell you about your project that you didn't know before today?
   `____________________________________________________________`

---

#### Build Log Entry — Day 4

**Goal for today:** Run the data collection experiment

**Data summary:** (briefly — what did the data show?)
`____________________________________________________________`

**One refinement made today:**
`____________________________________________________________`

**What does this project do well?**
`____________________________________________________________`

**What would you improve with more time?**
`____________________________________________________________`

---

#### Science Connection

> "Every time you connect a sensor to an Arduino and record its readings, you are building what scientists call an 'instrument' — a device that extends human perception. The thermometer extended our ability to feel temperature precisely. The telescope extended our sight to distant stars. Your device extends perception in its own small way, and the data you collected today is genuine scientific evidence."

What new knowledge does your data provide that you couldn't have known just by observing with your senses?
`____________________________________________________________`
`____________________________________________________________`

---

#### Challenge Extension

Copy your Serial Monitor output (select all, copy) and paste it into a Google Sheets or Excel spreadsheet. Use the data to:
1. Create a **line graph** of your primary sensor reading over time.
2. Add a **horizontal dashed line** at your threshold value.
3. Label the points where the system changed state.
4. Save or screenshot this graph for inclusion in your final presentation.

---

## 6. Safety Notes

- **Sustained operation today.** The device runs for longer periods than in previous sessions. Someone must monitor the board continuously. No unattended powered boards.
- **Environmental tests:** If teams are testing temperature response using heat sources (e.g., a lamp), ensure: no flammable materials are near the heat source; no one holds hot objects near the circuit; all heat tests use modest sources (desk lamp, warm water in a cup near — not touching — the sensor).
- **Do not blow on or breathe directly on the DHT11** to raise humidity — this can introduce moisture damage. A warm cupped hand held near the sensor is safer.
- **Avoid mechanical stress on wires during a run.** Moving the breadboard or pulling wires during data collection can cause intermittent connections that corrupt data.

| Symptom | Action |
|---------|--------|
| Data suddenly shows impossible values (9999, -999, NaN) | Likely a loose wire; power down, reseat the sensor connection, restart data collection |
| System behaviour changes mid-trial for no apparent reason | Note the time, check for loose wires, restart the trial |
| LDR reading doesn't change across conditions | Check that the LDR is in the circuit correctly (voltage divider with 10 kΩ to GND required) |
| Servo stops responding during a long run | Possible I2C bus conflict slowing things down; reduce LCD update frequency |

---

## 7. Assessment Rubric

**Formative Check (major input to final capstone grade — Science Concept & Engineering Design criteria):**

| Look-For | Observed? | Notes |
|----------|-----------|-------|
| Experiment follows the stated procedure (controlled variables held constant) | | |
| Data table contains specific numbers with units, not vague descriptions | | |
| At least 5 readings per condition collected | | |
| Analysis compares data to hypothesis with specific values cited | | |
| Conclusion is evidence-based (references actual data) | | |
| At least one refinement made based on data analysis | | |
| Build Log Day 4 entry is the most detailed entry (this is the experiment session) | | |

**Grading note:** The data collected today, and the analysis written in the worksheet, form the core evidence for the **Science Concept** criterion in `Capstone-F-Final-Presentation-Rubric.md`. The most common reason for low scores: vague analysis ("the data showed the system worked") vs. specific analysis ("the servo opened at 27.8°C on all 5 trials, which is 0.2°C below the 28°C threshold in the code, possibly due to DHT11 calibration offset of ±1°C").

---

## 8. Differentiation

### Support
- Provide a **pre-formatted data table** with column headers already filled in (matching the team's sensors) — students only fill in the numbers.
- Provide **sentence starters for the analysis section:**
  - "The data shows that as [independent variable] increased, [dependent variable]..."
  - "My hypothesis was [supported/refuted] because..."
  - "A possible source of error was..."
- For teams whose device is still not fully working: allow them to run a **partial data collection** (manual readings from Serial Monitor, even if the outputs aren't responding correctly) so they have at least some data to analyse and present.
- Guide teams to do a **simpler experiment**: instead of a full multi-condition trial, just record 10 consecutive readings and describe any pattern they see.

### Extension
- Challenge the team to calculate the **standard deviation** of their data set (or teach them range as a simpler measure of variation) — this introduces statistical literacy.
- Ask the team to run a **"failure mode test"**: what happens if you disconnect one sensor? How does the system behave? Is this what you expected? This tests robustness — a key engineering concept.
- Have the team research how their sensor's **accuracy specification** (e.g., DHT11: ±2°C) affects the confidence they can have in their conclusions. If their threshold is 28°C but the sensor has ±2°C error, what does that mean for their results?

### Visual / Kinesthetic Accommodations
- Allow students to **annotate a printout of their Serial Monitor output** with a highlighter to identify when state changes occurred — this is a physical, visual way to "see" the data before putting it in a table.
- Provide **graph paper** and help students plot a simple line graph by hand during the analysis section — seeing the data visually often reveals patterns that numbers in a table don't.
- For students who struggle with writing the conclusion: allow them to **explain it verbally** to the teacher or a partner who scribes it for them. The understanding matters more than the writing in this context.
