# Week 9, Session 18 — Mini-Project: Sensor Dashboard

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 18 of 36
**Week:** 9

---

## Learning Objectives

By the end of this session, students will be able to:
- Independently wire and program a multi-sensor circuit using at least 2 sensors from Phase 2.
- Display clearly labeled, formatted sensor data on the Serial Monitor.
- Apply the scientific method by forming a hypothesis, collecting data, and writing a conclusion.
- Evaluate and present their sensor dashboard clearly to a peer pair.

---

## Science Curriculum Link

**Grade 8 Concept: Review of Sensing & Analog Signals + Scientific Method**

This capstone session for Phase 2 integrates every major concept from Sessions 9–17: analog vs digital signals, `analogRead()`, voltage dividers, light sensing (LDR), temperature/humidity (DHT11), distance measurement (HC-SR04), and audio output (buzzer). Students apply the **scientific method** — forming a hypothesis about sensor behavior, collecting data with their dashboard, analyzing results, and writing a conclusion. This mirrors real scientific instrumentation: a real weather station, environmental monitor, or data logger is exactly a "sensor dashboard."

---

## Materials Checklist (per pair)

Students select **at least 2** of the following sensors (teacher may specify minimum):

**Required components (all pairs):**
- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- Jumper wires (assorted colors)
- Computer with Arduino IDE installed and all libraries installed (DHT sensor library)

**Sensor menu (choose at least 2):**
- 1 × LDR + 10 kΩ resistor (light sensor — Session 11)
- 1 × DHT11 module (temperature & humidity — Session 13)
- 1 × HC-SR04 ultrasonic sensor (distance — Session 17)
- 1 × 10 kΩ potentiometer (manual input — Session 10)
- 1 × Passive buzzer + 100 Ω resistor (audio output — Session 15)
- 1 × LED + 220 Ω resistor (visual output — Phase 1)

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

*Show a real-world dashboard image on the projector: a car dashboard, a plane cockpit, or a weather station display. Or describe one:*

*"A car dashboard shows you: speed, fuel level, engine temperature, door-ajar warning, and more — all at a glance. Every one of those readings comes from a sensor. An engineer had to wire each sensor, write code to read it, and format the output so the driver can understand it instantly."*

*"Today YOU are that engineer. You're going to design and build your own sensor dashboard. It needs to read at least two sensors and display them in a clean, labeled format on the Serial Monitor. This is your Phase 2 mini-project — it will be graded."*

**Hand out the project brief (this session's worksheet) and give 2 minutes for pairs to:**
1. Choose which 2+ sensors they will use.
2. Write a hypothesis: *"I predict that [sensor X] will read [approximately Y] under [condition Z]."*
3. Plan their dashboard layout on paper (what will each printed line look like?).

---

### 5–15 min — Direct Instruction

**Dashboard design principles (brief):**

> "Before you wire anything — plan. Ask yourself three questions for each sensor:
> 1. What pin does this sensor connect to?
> 2. What function do I use to read it?
> 3. How do I want to display this data — what label, what units, how many decimal places?"

Draw a sample Serial Monitor output on the board:

```
============ SENSOR DASHBOARD ============
Light Level:   647  (63%)
Temperature:   23.5 °C  (74.3 °F)
Humidity:      48.2 %RH
Distance:      35.8 cm
------------------------------------------
```

> "Notice: every reading has a LABEL (what it is), a NUMBER (the value), and UNITS (what the number means). No unlabeled numbers — a reader should understand your output instantly, even if they've never seen your code."

**Code structure review:**

Remind students of the `#include`, `setup()`, `loop()` structure. Show on the board how to call multiple sensors in the same sketch:

```cpp
// In setup():  Serial.begin(9600); dht.begin(); pinMode(TRIG, OUTPUT); etc.

// In loop():
// 1. Read each sensor
// 2. Print all readings with labels
// 3. delay(1000)
```

**Timing hint:**
> "The DHT11 needs 2 seconds between reads. The HC-SR04 needs 200 ms. Your loop delay should be at least 2 seconds if you're using both."

---

### 15–35 min — Hands-On Build + Code

**Students work independently (with teacher circulating). The teacher does NOT provide step-by-step wiring — students choose and plan their own from their Session notes.**

**Suggested teacher circulation script:**

- *"Tell me your sensor plan. Which pins did you choose and why?"*
- *"Before you plug in, show me your partner's wire check."*
- *"What does your first Serial Monitor print line look like? Draw it."*
- *"If sensor X gave you a weird reading, how would you debug it?"*

**Milestone checkpoints (circulate with a clipboard):**

- **Minute 15:** Circuit plan written/drawn on worksheet ✓
- **Minute 20:** Circuit wired, partner-checked, USB plugged in ✓
- **Minute 25:** Code compiles and uploads without errors ✓
- **Minute 30:** Serial Monitor shows at least 2 labeled sensor readings ✓
- **Minute 33:** All required criteria met; challenge started (optional) ✓

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Serial Monitor displays labeled readings from 2+ sensors, updating every 1–2 seconds.
- Each reading has a label and units (e.g., "Temp: 22.5 °C" not just "22.5").
- No error messages or "NaN" values.
- Student can explain what each line of code does if asked.
- Student has filled in their data table with at least 5 timestamped readings.

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| DHT11 reads NaN | Check DATA pin is connected and matches `DHT dht(pin, DHT11)`. VCC = 5V. |
| HC-SR04 reads 0 | TRIG/ECHO may be swapped. TRIG = OUTPUT, ECHO = INPUT. |
| LDR values not changing | Check junction wire is connected to A0 (not 5V or GND). 10 kΩ to GND from junction. |
| Sketch won't compile | Check `#include <DHT.h>` is at top; library installed. Check for missing semicolons. |
| Serial Monitor shows garbage | Baud rate mismatch — set to 9600 in both `Serial.begin()` and the Serial Monitor dropdown. |
| Only one sensor works | Check that the second sensor's pins don't conflict with the first. Analogue pins (A0–A15) and digital pins (2–53) are all independent on the Mega. |
| Loop runs but readings freeze | DHT11 needs `delay(2000)` minimum. If using `millis()` for timing, check the interval logic. |

---

### 42–45 min — Reflection / Exit Ticket

**Peer review (2 minutes):**
Each pair opens the Serial Monitor on their screen. The pair to their left reads the output and writes one sentence on a sticky note: *"I can/cannot understand this dashboard because ___."* Sticky notes go on the presenting pair's workstation.

**Exit ticket (students write on their worksheet, Section 5):**
1. *"Describe your hypothesis, what you measured, and whether your data supported it."*
2. *"Name one source of error or limitation in your sensor dashboard."*
3. *"What would you add if you had 10 more minutes?"*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram (Sample Dashboard: LDR + DHT11 + HC-SR04)

```
   Arduino Mega 2560
   ┌──────────────────────────────────────────┐
   │                                          │
   │  5V ─────────────────────────────────────┼──── Red rail (+)
   │  GND ────────────────────────────────────┼──── Black rail (–)
   │                                          │
   │  A0 ─────────────────────────────────────┼──── LDR junction (LDR→5V, 10kΩ→GND)
   │                                          │
   │  Digital Pin 2 ──────────────────────────┼──── DHT11 DATA
   │                                          │
   │  Digital Pin 10 (OUTPUT) ────────────────┼──── HC-SR04 TRIG
   │  Digital Pin 11 (INPUT)  ────────────────┼──── HC-SR04 ECHO
   │                                          │
   │  Pin 9 (~) ──────────────────────────────┼──── [220 Ω] → LED (+) → LED (–) → GND
   └──────────────────────────────────────────┘
   
   LDR divider:   5V rail → [LDR] → A0 → [10 kΩ] → GND rail
   DHT11 module:  VCC=5V, DATA=Pin 2, GND=GND
   HC-SR04:       VCC=5V, TRIG=Pin 10, ECHO=Pin 11, GND=GND
```

### Fritzing-Style Build Description (Sample — Students adapt for their sensor choice)

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | **+ rail** | LDR leg 1 (row 5) | Red | LDR to 5V |
| 4 | LDR leg 2 (row 7) | 10 kΩ leg 1 (row 7) | — | LDR-R junction |
| 5 | 10 kΩ leg 2 | **– rail** | — | Fixed R to GND |
| 6 | Row 7 (junction) | Arduino **A0** | Yellow | LDR signal |
| 7 | DHT11 **VCC** | **+ rail** | Red | |
| 8 | DHT11 **GND** | **– rail** | Black | |
| 9 | DHT11 **DATA** | Arduino **Pin 2** | White | |
| 10 | HC-SR04 **VCC** | **+ rail** | Red | |
| 11 | HC-SR04 **GND** | **– rail** | Black | |
| 12 | HC-SR04 **TRIG** | Arduino **Pin 10** | White | |
| 13 | HC-SR04 **ECHO** | Arduino **Pin 11** | Blue | |
| 14 | Arduino **Pin 9** | 220 Ω → LED **+** | Orange | Alarm LED |
| 15 | LED **–** | **– rail** | Black | |

[Google image search: "Arduino Mega multi-sensor project LDR DHT11 HC-SR04"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 18: Mini-Project — Sensor Dashboard
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * A SENSOR DASHBOARD is a real-time display of multiple sensor
 * readings in one place. Every scientific instrument — from a
 * weather station to a Mars rover — is a sensor dashboard at heart.
 *
 * SENSORS DEMONSTRATED:
 *   LDR      → measures LIGHT INTENSITY (analog, 0–1023)
 *              Science: LDR resistance decreases with light (Session 11)
 *
 *   DHT11    → measures TEMPERATURE and HUMIDITY (digital protocol)
 *              Science: thermodynamics, heat transfer (Session 13)
 *
 *   HC-SR04  → measures DISTANCE (ultrasonic echolocation)
 *              Science: speed of sound, echo timing (Session 17)
 *
 * READING ALL AT ONCE:
 *   The Arduino Mega has 16 analog pins AND 54 digital pins —
 *   plenty of room for multiple sensors without conflict.
 *
 * This complete sketch demonstrates all three sensors together.
 * Students may simplify to any 2 sensors by commenting out
 * the sections they do not need.
 * ------------------------------------------------------------------
 *
 * LIBRARY REQUIRED: "DHT sensor library" by Adafruit
 *
 * Sensors:  LDR+10kΩ divider on A0
 *           DHT11 on Pin 2
 *           HC-SR04 TRIG on Pin 10, ECHO on Pin 11
 *           LED + 220 Ω on Pin 9 (alarm)
 * ============================================================
 */

// ---- Libraries ----
#include <DHT.h>    // Adafruit DHT sensor library (install via Library Manager)

// ============================================================
// ---- SENSOR CONFIGURATION — Edit to match YOUR sensors ----
// ============================================================

// --- LDR (Light Sensor) ---
const int LDR_PIN = A0;              // LDR voltage divider output

// --- DHT11 (Temperature & Humidity) ---
const int DHT_PIN = 2;               // DHT11 DATA pin
DHT dht(DHT_PIN, DHT11);             // Create DHT object

// --- HC-SR04 (Distance Sensor) ---
const int TRIG_PIN = 10;             // Trigger output
const int ECHO_PIN = 11;             // Echo input

// --- Output ---
const int LED_PIN  = 9;              // Alarm LED
const int ALARM_DISTANCE_CM = 20;    // LED on if closer than this

// ============================================================
// ---- Helper Function: Measure Distance with HC-SR04 ----
// ============================================================
float measureDistance_cm() {
  // Send a 10 µs trigger pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Measure echo duration (timeout = 30 ms)
  long duration = pulseIn(ECHO_PIN, HIGH, 30000);

  if (duration == 0) return -1.0;    // -1 means "out of range"

  // Calculate and return distance
  return (duration * 0.0343) / 2.0;
}

// ============================================================
// ---- Setup ----
// ============================================================
void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);

  // Print dashboard header
  Serial.println("╔══════════════════════════════════════╗");
  Serial.println("║      SENSOR DASHBOARD — PHASE 2      ║");
  Serial.println("╚══════════════════════════════════════╝");
  Serial.print("Started at: ");
  Serial.print(0);
  Serial.println(" seconds");
  Serial.println();
}

// ============================================================
// ---- Main Loop ----
// ============================================================
void loop() {
  // --- Read all sensors ---

  // 1. LDR — Light
  int   rawLight   = analogRead(LDR_PIN);
  int   lightPct   = map(rawLight, 0, 1023, 0, 100);
  float lightVolt  = (rawLight / 1023.0) * 5.0;
  String lightDesc;
  if      (rawLight > 700) lightDesc = "BRIGHT";
  else if (rawLight > 400) lightDesc = "MEDIUM";
  else if (rawLight > 150) lightDesc = "DIM";
  else                     lightDesc = "DARK  ";

  // 2. DHT11 — Temperature & Humidity
  float humidity    = dht.readHumidity();
  float tempC       = dht.readTemperature();
  float tempF       = tempC * 9.0 / 5.0 + 32.0;
  bool  dhtOK       = (!isnan(humidity) && !isnan(tempC));

  // 3. HC-SR04 — Distance
  float distance_cm = measureDistance_cm();

  // --- Alarm logic ---
  bool alarmOn = (distance_cm > 0 && distance_cm < ALARM_DISTANCE_CM);
  digitalWrite(LED_PIN, alarmOn ? HIGH : LOW);

  // --- Print dashboard ---
  Serial.println("──────────────────────────────────────");
  Serial.print("Time:          ");
  Serial.print(millis() / 1000);
  Serial.println(" s");

  Serial.print("Light:         ");
  Serial.print(rawLight);
  Serial.print(" (");
  Serial.print(lightPct);
  Serial.print("%)  ");
  Serial.println(lightDesc);

  if (dhtOK) {
    Serial.print("Temperature:   ");
    Serial.print(tempC, 1);
    Serial.print(" °C  /  ");
    Serial.print(tempF, 1);
    Serial.println(" °F");

    Serial.print("Humidity:      ");
    Serial.print(humidity, 1);
    Serial.println(" %RH");
  } else {
    Serial.println("Temperature:   ERROR — check DHT11 wiring");
    Serial.println("Humidity:      ERROR");
  }

  if (distance_cm > 0) {
    Serial.print("Distance:      ");
    Serial.print(distance_cm, 1);
    Serial.print(" cm");
    if (alarmOn) Serial.print("  *** ALARM: CLOSE! ***");
    Serial.println();
  } else {
    Serial.println("Distance:      OUT OF RANGE");
  }

  Serial.println();   // Blank line between readings

  // >>> TRY CHANGING THIS: 2000 ms = once every 2 seconds
  // DHT11 requires at least 2 seconds between readings
  delay(2000);
}

// ===== CHALLENGE =====
// 1. SERIAL PLOTTER COMPATIBILITY: Print only numbers separated by
//    commas for the Serial Plotter to graph all three sensors at once:
//    Serial.print(rawLight); Serial.print(",");
//    Serial.print(tempC); Serial.print(",");
//    Serial.println(distance_cm);
//
// 2. SMART ALERTS: Add meaningful alert thresholds:
//    - Print "TOO HOT!" if tempC > 30
//    - Print "TOO DARK!" if rawLight < 200
//    - Blink LED rapidly (10 Hz) if distance < 10 cm
//
// 3. CUSTOM DASHBOARD: Add a divider line that uses millis() to
//    print a separator every 10 readings.
//    Use: if (readingCount % 10 == 0) { Serial.println("===="); }
//
// 4. SAVE YOUR DATA: Copy the Serial Monitor output (Edit → Select All,
//    then paste into a Google Sheet). This creates a real data log!
```

---

## 5. Student Worksheet

---

### Session 18 Worksheet — Mini-Project: Sensor Dashboard

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**This is a GRADED project.** Read all sections before you start building.

---

#### Step 1: Project Plan (complete BEFORE wiring)

**Which sensors will you use? (Circle at least 2):**

LDR (light) &nbsp;&nbsp;&nbsp; DHT11 (temp/humidity) &nbsp;&nbsp;&nbsp; HC-SR04 (distance) &nbsp;&nbsp;&nbsp; Potentiometer &nbsp;&nbsp;&nbsp; Other: ______

**My sensor plan:**

| Sensor | Pin(s) | What it measures | Unit |
|--------|--------|------------------|------|
| | | | |
| | | | |
| | | | |

**My dashboard layout (sketch what the Serial Monitor output will look like):**

```
============ MY DASHBOARD ============
[Label 1]:  ______  [unit]
[Label 2]:  ______  [unit]
[Label 3]:  ______  [unit]  (optional)
--------------------------------------
```

---

#### Step 2: Hypothesis

Before testing, write a prediction using the format:

*"I predict that when [condition], the [sensor] will read approximately [value] because [reason based on science]."*

**Hypothesis 1:** ___________________________________________________________________________

**Hypothesis 2:** ___________________________________________________________________________

---

#### Step 3: What I Already Know (Quick Review)

Answer these without looking at your notes:

1. `analogRead()` returns values from _______ to _______.
2. The DHT11 sensor measures _______________________ and _______________________.
3. The HC-SR04 distance formula is: distance = _____________________________________________
4. Name one safety rule from Phase 2 that applies to your project today:

   ___________________________________________________________________________

---

#### Step 4: Build & Code Checklist

Work through these steps and check off each one:

**Circuit:**
- [ ] All sensors wired with USB unplugged
- [ ] Partner check completed — all wires verified against plan
- [ ] 5V and GND connected to breadboard rails
- [ ] Each sensor connected to the correct Arduino pin

**Code:**
- [ ] Library(ies) installed (DHT library if using DHT11)
- [ ] Pin numbers in code match actual wiring
- [ ] Each sensor reading assigned to a named variable with a comment
- [ ] Each Serial.print() line includes a label AND units
- [ ] Sketch compiles without errors (green bar, no red text)
- [ ] Sketch uploads successfully

**Testing:**
- [ ] Serial Monitor open at 9600 baud
- [ ] At least 2 sensor readings visible on Serial Monitor
- [ ] No "NaN" or "ERROR" messages (or they are explained)
- [ ] LED or buzzer responds to a condition (optional but encouraged)

---

#### Step 5: Data Table

Record 10 timed readings from your dashboard. Write what you were doing (Event) during each reading:

| # | Time (s) | Sensor 1: ___________ | Sensor 2: ___________ | Sensor 3 (optional): ________ | Event |
|---|----------|----------------------|-----------------------|-------------------------------|-------|
| 1 | | | | | Baseline |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |
| 6 | | | | | |
| 7 | | | | | |
| 8 | | | | | |
| 9 | | | | | |
| 10 | | | | | |

---

#### Step 6: Reflection & Conclusion

**Hypothesis check:**
Was your Hypothesis 1 supported by your data? Explain with evidence:

___________________________________________________________________________

Was your Hypothesis 2 supported? Explain:

___________________________________________________________________________

**Science connection:**
Choose ONE sensor you used. Explain the science behind how it works (in 3–5 sentences):

___________________________________________________________________________

___________________________________________________________________________

**Source of error:**
Name one source of error in your measurements and how you could reduce it:

___________________________________________________________________________

**Peer feedback received** (from the pair next to you):

___________________________________________________________________________

**What I would add / improve:**

___________________________________________________________________________

---

#### Step 7: Challenge Extension

If your dashboard is complete and working, try at least one extension:

- [ ] Add a **third sensor** to your dashboard.
- [ ] Add a **smart alert** (buzzer/LED) triggered by a threshold condition.
- [ ] Print data in a format that works with the Serial Plotter (comma-separated values).
- [ ] Copy the Serial Monitor output into a spreadsheet and create a line graph.

Describe what you attempted: ___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Multiple sensors = more wires | More complexity = more chance of mis-wiring. Follow Build → Check → Power on strictly. Partner check is mandatory before plugging in USB. |
| DHT11 polarity | VCC to 5V, GND to GND, DATA to the pin in your code. Reversed VCC/GND can damage the sensor. |
| HC-SR04 voltage | Confirm your specific HC-SR04 module is 5V-compatible (most are). |
| LDR circuit short | Verify LDR is between 5V and the junction, NOT shorting directly to GND. |
| Buzzer near ears | If adding a buzzer for alerts, keep tones brief and below 2000 Hz. |

**Build → Check → Power on.** This session has the most complex circuits of Phase 2. Take extra time on the partner check.

**If something goes wrong:**
- Start with ONE sensor working before adding the second. Test incrementally.
- If the board feels warm → unplug USB immediately, find and fix the short.
- "Port not found" after plugging in → try a different USB port; check the cable.

---

## 7. Assessment Rubric — FULL 4-LEVEL GRADED RUBRIC

**This is a graded session.** The rubric below will be used to evaluate your Phase 2 Mini-Project.

**Total points: 20 (5 criteria × 4 max points each)**

---

### Criterion 1: Circuit Construction

| Level | Score | Descriptor |
|-------|-------|------------|
| **4 — Exemplary** | 4 | Circuit is fully correct and independently wired. All sensors connected to correct pins with proper wire colors (red=5V, black=GND). Pull-downs, voltage dividers, and current-limiting resistors all present and correctly valued. No loose or misrouted wires. A third sensor or additional output was added independently. |
| **3 — Proficient** | 3 | Circuit is correctly wired for both required sensors. Wire colors are consistent. All resistors are present and correctly placed. Minor cosmetic issue (e.g., jumper slightly loose) but no functional errors. Power-on check done before USB connected. |
| **2 — Developing** | 2 | Circuit mostly correct but has 1–2 errors that required teacher assistance to identify and fix (e.g., LDR junction mis-routed, TRIG/ECHO swapped). After correction, circuit functions. Partner check was not completed independently. |
| **1 — Beginning** | 1 | Circuit has multiple wiring errors; does not produce correct readings without significant teacher assistance. Pull-down resistor, voltage divider, or current-limiting resistor missing or incorrectly placed. |

---

### Criterion 2: Code Functionality

| Level | Score | Descriptor |
|-------|-------|------------|
| **4 — Exemplary** | 4 | Sketch compiles and uploads without errors. Serial Monitor displays labeled, formatted readings from 3+ sensors. All readings include labels AND units. Code includes at least one smart alert (threshold-triggered LED/buzzer) or a third sensor. Code is commented in the student's own words. Variable names are meaningful. |
| **3 — Proficient** | 3 | Sketch compiles and runs correctly. Serial Monitor displays labeled readings with units from both required sensors. No NaN or error messages. Readings update at an appropriate interval (1–2 s). `delay(2000)` or equivalent used when DHT11 is included. Basic comments present. |
| **2 — Developing** | 2 | Sketch runs but has minor issues: readings appear without units OR without labels (not both), OR one sensor occasionally returns error values. Code is partially commented. Required functionality is present but needed 1–2 teacher hints to achieve. |
| **1 — Beginning** | 1 | Sketch compiles but produces incorrect or missing readings for one or both sensors. Significant debugging was needed with teacher help. Output is unlabeled or formatted in a confusing way. No meaningful comments in code. |

---

### Criterion 3: Science Understanding

| Level | Score | Descriptor |
|-------|-------|------------|
| **4 — Exemplary** | 4 | Student can clearly explain the physics behind BOTH sensors used (e.g., "The LDR resistance decreases because more photons free electrons in the semiconductor, which..."). Written reflection connects sensor readings to specific science concepts from Phase 2 (EM spectrum, thermodynamics, sound waves, Ohm's Law). Hypothesis is scientifically reasoned and the conclusion evaluates whether the data supports it. Source of error is specific and scientific. |
| **3 — Proficient** | 3 | Student can explain how at least one sensor works using scientific language (e.g., "The DHT11 measures temperature using a thermistor — a resistor that changes with heat"). Written reflection accurately connects to a Phase 2 science concept. Hypothesis is stated in proper IF/THEN or prediction format. Conclusion references actual data. |
| **2 — Developing** | 2 | Student can describe what each sensor measures but struggles to explain the underlying physics without prompting. Science connection in reflection is vague or partially correct (e.g., "LDR measures light because it's a light sensor"). Hypothesis is present but lacks scientific reasoning. Conclusion does not clearly reference data. |
| **1 — Beginning** | 1 | Student can identify what the sensors measure but cannot explain why or how. Science connection is absent or incorrect. No hypothesis was written, or it is not testable. Reflection is minimal (one or two words per question). |

---

### Criterion 4: Collaboration

| Level | Score | Descriptor |
|-------|-------|------------|
| **4 — Exemplary** | 4 | Both partners actively contributed to all phases: planning, wiring, coding, and reflection. Roles were clearly divided AND both partners can explain every part of the project. Pair helped at least one neighboring pair with debugging. Both partners can demonstrate the working dashboard and answer questions about it independently. |
| **3 — Proficient** | 3 | Both partners contributed meaningfully. Builder and Coder roles were rotated or shared so that both have hands-on experience with wiring AND coding. Pair completed the partner wire-check before powering on. Both partners contributed to the hypothesis and reflection. |
| **2 — Developing** | 2 | One partner dominated most of the work (either wiring or coding or both). The quieter partner can answer some but not all questions about the project. Partner check was done but one partner was primarily passive. |
| **1 — Beginning** | 1 | Work was not shared — one student did nearly all the wiring and/or coding while the other was disengaged. Neither partner can fully explain parts of the project they did not personally build or write. |

---

### Criterion 5: Reflection Quality

| Level | Score | Descriptor |
|-------|-------|------------|
| **4 — Exemplary** | 4 | All 6 reflection items on the worksheet are completed in full sentences with specific, evidence-based responses. Hypothesis evaluation explicitly references multiple data points (e.g., "My Reading #4 showed..."). Source of error is precise and quantitative if possible. "What I would improve" shows creative and technically sound thinking (not just "add more sensors"). Peer feedback was thoughtfully received and commented on. |
| **3 — Proficient** | 3 | All 6 reflection items are completed. Hypothesis is evaluated as supported or not, with at least one specific data reference. Science connection is accurate and specific to the sensor used. Source of error is identified and plausible. "What I would add" is specific and feasible. |
| **2 — Developing** | 2 | Most reflection items are attempted but some are one-word answers or incomplete. Hypothesis is evaluated without referencing specific data. Science connection is present but vague. 1–2 items are missing or left blank. |
| **1 — Beginning** | 1 | Fewer than 3 reflection items are completed. Responses are very brief (single words or "yes/no"). No scientific reasoning is present. Hypothesis evaluation is missing or "yes it worked" without explanation. |

---

### Scoring Summary

| Criterion | Max Points | Student Score | Teacher Comments |
|-----------|-----------|---------------|-----------------|
| 1. Circuit Construction | 4 | | |
| 2. Code Functionality | 4 | | |
| 3. Science Understanding | 4 | | |
| 4. Collaboration | 4 | | |
| 5. Reflection Quality | 4 | | |
| **TOTAL** | **20** | | |

**Grade conversion (teacher discretion):**
- 18–20 = A (Exemplary)
- 14–17 = B (Proficient)
- 10–13 = C (Developing)
- Below 10 = Needs improvement / resubmission recommended

---

## 8. Differentiation

### Support
- Provide a "Sensor Dashboard Starter Code" file with the library include, pin definitions, setup, and comment-guided blank `loop()` with `// FILL IN: read your LDR here` style prompts.
- Give a printed copy of the relevant code sections from Sessions 11, 13, and 17 so students can reference working sensor read code without re-reading full session files.
- Limit supported pairs to 2 sensors maximum; ensure those 2 sensors are the ones they built most successfully in earlier sessions.
- Provide a sentence frame for each reflection question:
  - Hypothesis evaluation: *"My hypothesis was [supported / not supported] because my data showed that _______, which means _______."*
  - Science connection: *"The [sensor] works by _______, which connects to the science idea of _______."*
- Assign a planning partner check: teacher reviews the circuit plan before any wiring begins.

### Extension
- Require 3+ sensors for full marks at the Exemplary level.
- Challenge students to display data as a percentage or a human-readable description (e.g., "Hot," "Warm," "Cool," "Cold") alongside the raw number — requires `if/else` chains.
- Ask students to export their Serial Monitor data to a spreadsheet (copy-paste) and create a multi-line graph — connects to Session 14 data logging.
- Code challenge: replace `delay(2000)` with a `millis()`-based non-blocking timer so the Arduino could theoretically handle other tasks (like responding to a button) while waiting between readings.
- Design challenge: *"If you could add one more sensor that doesn't exist in our kit, what would it be and what science concept would it measure?"* — encourages creative application.

### Visual / Kinesthetic Accommodations
- Provide a large printed "Sensor Connection Summary" showing pin assignments for all 5 sensors on one page — students circle their chosen sensors and annotate with their chosen pin numbers.
- For students who benefit from physical organization: use colored sticky notes on the breadboard to mark sensor regions (yellow zone = LDR, green zone = DHT11, blue zone = HC-SR04).
- Allow a "run-through" of the dashboard: teacher calls out conditions ("cover the light sensor...now release it...now move your hand toward the HC-SR04...") while students watch the Serial Monitor — structured observation reduces open-ended uncertainty.
- If the Serial Monitor output is visually overwhelming, suggest opening the Serial Plotter for a graphical view of all three readings simultaneously (uses the comma-separated format from Challenge 1).
- Provide a word bank for the science connection reflection: *semiconductor, resistance, photon, thermistor, humidity, capacitor, echolocation, longitudinal wave, speed of sound, voltage divider.*
