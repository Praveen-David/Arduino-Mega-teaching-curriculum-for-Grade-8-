# Week 16, Session 32 — Build Day 2: Coding

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Build Day — Software / Code Writing

## Learning Objectives
- **Translate** pseudocode from the planning template into working Arduino C++ code.
- **Upload** and test individual code sections piece-by-piece before combining them.
- **Use** appropriate libraries (DHT.h, Servo.h, LiquidCrystal_I2C.h) correctly in a self-designed sketch.
- **Debug** compile-time errors independently using Serial Monitor output as a diagnostic tool.

**Science Curriculum Link:** Data Measurement & Systems Thinking — Code is the "brain" of a measurement system. Students apply their understanding of conditionals, loops, and sensor reading functions to write code that measures real-world phenomena and responds to them — the computational core of the scientific method.

## Materials Checklist (per team)
- [ ] Arduino Mega 2560 + USB cable
- [ ] Completed circuit from Session 31 (hardware build)
- [ ] Laptop with Arduino IDE 2.x installed
- [ ] Required libraries installed: `DHT sensor library`, `Servo` (built-in), `LiquidCrystal_I2C`
- [ ] Completed Planning Template (pseudocode section — this is the coding guide)
- [ ] Build Log template (Day 2 entry to fill in)
- [ ] `Capstone-G-Sample-Exemplar-Project.md` available for reference (teacher decides whether to share)
- [ ] Notebooks / project binders

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Open the Arduino IDE on the projector. Show a blank `.ino` file.

**Ask:**
> "If pseudocode is the recipe, what is the Arduino code? What needs to happen before you type a single line?"

Expected: review the pseudocode plan, figure out which libraries are needed, know the pin numbers.

**Quick review warm-up (30 seconds each, hands up):**
- "What function reads an analog sensor?" → `analogRead(pin)`
- "What function reads a DHT11 temperature?" → `dht.readTemperature()`
- "How do you move a servo to 90 degrees?" → `myServo.write(90)`
- "What does `Serial.println()` do?" → prints a value to the Serial Monitor

**Bridge (say this):**
> "Today you turn your plan into a real program. Write it in sections, test each section before adding the next. A sketch that does ONE thing perfectly is better than a sketch that tries to do FIVE things and crashes."

---

### 5–15 min — Direct Instruction: Coding Strategy — Incremental Build

**Write on the board:**

```
INCREMENTAL CODING STRATEGY:
1. Write INCLUDES & CONSTANTS (pin defs, thresholds)
2. Write SETUP (Serial, LCD, sensor init)
3. Test upload → confirm Serial Monitor shows startup message
4. Add one SENSOR READ → test → confirm correct values
5. Add CONTROL LOGIC (if/else) → test → confirm correct behaviour
6. Add all remaining sensors / outputs one at a time
7. Add DATA LOGGING (Serial.println with labels)
8. Final integration test
```

**Model the first three steps on the projector** using a generic example:

```cpp
#include <DHT.h>
#define DHT_PIN 4
#define DHT_TYPE DHT11
DHT dht(DHT_PIN, DHT_TYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
  Serial.println("System starting...");
}

void loop() {
  float temp = dht.readTemperature();
  if (isnan(temp)) {
    Serial.println("DHT11 read error!");
  } else {
    Serial.print("Temp: ");
    Serial.print(temp);
    Serial.println(" C");
  }
  delay(2000);
}
```

Upload it, open Serial Monitor, show the temperature reading.

**Key teaching points:**
- `isnan()` — always check for NaN (Not a Number) from DHT11; a wiring error gives garbage data.
- `delay(2000)` — give the DHT11 time between readings; it updates every ~2 seconds.
- Add one thing at a time, test, then add the next.

**Common library errors (write on board — these will come up today):**
| Error | Cause | Fix |
|-------|-------|-----|
| `'DHT' was not declared` | Library not installed | Sketch → Include Library → Manage Libraries → install "DHT sensor library" by Adafruit |
| `'LiquidCrystal_I2C' not found` | Library not installed | Install "LiquidCrystal I2C" by Frank de Brabander |
| `LCD shows garbage/blocks` | Wrong I2C address | Change `0x27` to `0x3F` in constructor |
| `Servo jitters` | Signal on non-PWM pin | Move to a `~` marked pin |

---

### 15–35 min — Hands-On: Write & Test Code

**Teams follow the incremental coding strategy.** Each team's code is unique to their project.

**Teacher role:** Circulate every 3–4 minutes. Ask each team:
- "What are you working on right now?"
- "What does the Serial Monitor show?"
- "What's the next section you're going to add?"

**Milestone checkpoints (write on board — teams aim for as many as possible):**

| Milestone | What it means |
|-----------|---------------|
| ✅ Compiles without errors | Code structure is valid |
| ✅ Serial Monitor shows startup message | Setup() runs correctly |
| ✅ Sensor 1 reads correctly | One sensor value printing |
| ✅ Sensor 2 reads correctly | Second sensor value printing |
| ✅ Output 1 responds to sensor | First control logic working |
| ✅ Output 2 responds to sensor | Second control logic working |
| ✅ LCD displays a reading | Display working |
| ✅ Data logs to Serial with labels | Data collection ready |

Teams who reach all checkpoints should begin the CHALLENGE section in their code.

**Code quality reminders (post on board):**
- Comment every section with what it does
- Use named constants (`#define TEMP_THRESHOLD 28`) instead of magic numbers
- Use `Serial.print()` labels so data is readable: `"Temp: 24.5 C"` not just `24.5`

---

### 35–42 min — Testing & Debugging

By the end of this session teams should have code that compiles and partially works. Full integration (hardware + all code together) is the goal of Session 33.

**Testing steps for Build Day 2:**

1. Upload the sketch — confirm 0 errors in the IDE.
2. Open Serial Monitor at 9600 baud.
3. Confirm sensor values print — are they realistic? (Temperature ~18–28°C in a classroom, LDR value changes when you cover the sensor with your hand).
4. Test one output: does the LED / servo / buzzer respond when you manually trigger the condition (e.g., warm the DHT11 with your hand to push temp above threshold)?

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| "expected ';' before…" error | Missing semicolon | Check the line number in the error; add `;` |
| `was not declared in this scope` | Variable used before being defined | Move variable declaration to the top; check spelling |
| Serial Monitor shows garbage characters | Wrong baud rate | Set Serial Monitor to match `Serial.begin(9600)` |
| DHT11 always prints "read error" | Wiring issue or wrong pin | Recheck DATA wire; confirm `#define DHT_PIN` matches actual wire |
| Servo moves to one extreme and stops | Missing `servo.attach(pin)` in setup | Add `myServo.attach(PIN);` in `setup()` |
| LCD shows blocks/squares | I2C address wrong | Try `0x3F` instead of `0x27` (or vice versa) |
| analogRead always returns 0 | Sensor not connected to the correct analog pin | Check A0–A15 pin assignment |

---

### 42–45 min — Reflection / Exit Ticket

**Students record in project binders:**

1. List the code milestones your team reached today (check the board list):
   `____________________________________________________________`

2. What was the most confusing error you got? How did you fix it?
   `____________________________________________________________`

3. What sections of code are still unfinished for Session 33?
   `____________________________________________________________`

4. On a scale of 1–5, how confident are you in your code right now? (1 = very uncertain, 5 = great)
   Circle: **1 — 2 — 3 — 4 — 5**

---

## 3. Circuit Diagram

The hardware circuit is unchanged from Session 31. Students use their completed Build Day 1 circuit.

```
  BUILD DAY 2: CIRCUIT IS ALREADY BUILT (from Session 31)
  ========================================================
  Your wiring does not change today.

  IMPORTANT: If the circuit needed changes after Session 31's
  debugging, those changes should have been made.
  Confirm the circuit is correct BEFORE writing code —
  bad wiring makes debugging code very confusing.

  REMINDER — Key Connections:
  ┌──────────────────────────────────────────────────────┐
  │ Component          │ Pin          │ Wire colour      │
  ├──────────────────────────────────────────────────────┤
  │ 5V power rail      │ 5V           │ red              │
  │ GND rail           │ GND          │ black            │
  │ DHT11 data (if used)│ pin 4 *      │ yellow/green     │
  │ LDR signal (if used)│ A0 *         │ yellow           │
  │ HC-SR04 TRIG (if)  │ pin 7 *      │ yellow           │
  │ HC-SR04 ECHO (if)  │ pin 8 *      │ green            │
  │ Servo signal (if)  │ pin 9 (~PWM)*│ orange           │
  │ LED anode (if)     │ pin 6 (~PWM)*│ various          │
  │ Buzzer + (if)      │ pin 3 (~PWM)*│ yellow           │
  │ LCD I2C SDA        │ pin 20       │ blue             │
  │ LCD I2C SCL        │ pin 21       │ yellow           │
  └──────────────────────────────────────────────────────┘
  * = these are example pin assignments; use YOUR plan's values

  Build → Check → Power on still applies.
  Power down to adjust wiring; power on only to test code.
```

[Google image search: "Arduino Mega DHT11 servo LCD I2C breadboard circuit"]

---

## 4. Arduino Code

The code below is a **structured skeleton** that teams adapt to their specific project. All common library patterns are shown. Teams replace the placeholder pin numbers and logic with their own project values.

```cpp
// =============================================================
// CAPSTONE PROJECT — BUILD DAY 2: PROJECT CODE SKELETON
// Team: _______________________
// Project: ____________________
// Date: _______________________
//
// SCIENCE: This code is the "brain" of your measuring device.
//   It reads sensors, makes decisions based on thresholds,
//   controls outputs, and logs data for analysis.
//   Each section below matches one line of your pseudocode.
// =============================================================

// ---- LIBRARIES ---- include only what your project uses ----
#include <DHT.h>              // DHT11 temperature/humidity sensor
#include <Servo.h>            // Servo motor control
#include <LiquidCrystal_I2C.h> // LCD 16x2 with I2C backpack

// ---- PIN DEFINITIONS ---- name every pin clearly -----------
#define DHT_PIN       4       // DHT11 data wire → Mega pin 4
#define DHT_TYPE      DHT11   // sensor model
#define LDR_PIN       A0      // LDR voltage divider → Mega A0
#define SERVO_PIN     9       // Servo signal → Mega pin ~9 (PWM)
#define LED_PIN       6       // Status LED → Mega pin ~6 (PWM)
#define BUZZER_PIN    3       // Passive buzzer → Mega pin ~3 (PWM)
#define TRIG_PIN      7       // HC-SR04 trigger → Mega pin 7
#define ECHO_PIN      8       // HC-SR04 echo → Mega pin 8

// ---- THRESHOLDS ---- change these for your experiment ------
#define TEMP_HIGH     28      // °C — above this = "hot" condition
#define LIGHT_LOW     300     // 0–1023 — below this = "dark"
#define DISTANCE_NEAR 20      // cm — below this = object detected

// >>> TRY CHANGING THIS: adjust thresholds to test your hypothesis

// ---- LIBRARY OBJECTS ----------------------------------------
DHT dht(DHT_PIN, DHT_TYPE);               // DHT11 sensor object
Servo myServo;                             // Servo motor object
LiquidCrystal_I2C lcd(0x27, 16, 2);      // LCD at I2C address 0x27
// If LCD shows blank/blocks, change 0x27 to 0x3F above

// ---- GLOBAL VARIABLES ---------------------------------------
float temperature = 0.0;      // current temperature reading (°C)
float humidity    = 0.0;      // current humidity reading (%)
int   lightLevel  = 0;        // LDR reading (0–1023)
long  duration    = 0;        // HC-SR04 pulse duration (microseconds)
int   distanceCm  = 0;        // calculated distance (cm)
int   loopCounter = 0;        // counts loop iterations for timing

// =============================================================
void setup() {
  // Start Serial communication for data logging
  Serial.begin(9600);
  Serial.println("=== Capstone Project Starting ===");
  Serial.println("Project: [YOUR PROJECT NAME HERE]");
  Serial.println("Science Question: [YOUR QUESTION HERE]");
  Serial.println("----------------------------------");
  Serial.println("Time(s), Temp(C), Humidity(%), Light(0-1023), Dist(cm)");

  // Initialise the DHT11 sensor
  dht.begin();

  // Attach the servo to its pin and set start position
  myServo.attach(SERVO_PIN);
  myServo.write(0);             // servo starts at 0 degrees

  // Initialise the LCD
  lcd.init();
  lcd.backlight();              // turn on LCD backlight
  lcd.setCursor(0, 0);
  lcd.print("Project Ready!");  // startup message on LCD

  // Set output pin modes
  pinMode(LED_PIN,    OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(TRIG_PIN,   OUTPUT);
  pinMode(ECHO_PIN,   INPUT);

  delay(2000);                  // pause so LCD message is readable
  lcd.clear();
}

// =============================================================
void loop() {

  // ---- READ DHT11 SENSOR ----------------------------------
  temperature = dht.readTemperature();   // read °C
  humidity    = dht.readHumidity();      // read %

  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("ERROR: DHT11 read failed. Check wiring.");
    return;                      // skip the rest of this loop if sensor fails
  }

  // ---- READ LDR (LIGHT SENSOR) ----------------------------
  lightLevel = analogRead(LDR_PIN);     // 0 (dark) to 1023 (bright)

  // ---- READ HC-SR04 (DISTANCE) ----------------------------
  // Send a 10-microsecond trigger pulse
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // Measure the echo pulse duration
  duration   = pulseIn(ECHO_PIN, HIGH);
  distanceCm = duration * 0.034 / 2;   // convert to cm

  // ---- CONTROL LOGIC (if/else based on readings) ----------
  // >>> TRY CHANGING THIS: modify the conditions for your experiment

  if (temperature > TEMP_HIGH) {
    // Hot condition: open the "vent" (servo) and turn on LED
    myServo.write(90);                   // rotate servo to 90 degrees
    digitalWrite(LED_PIN, HIGH);         // LED on
    tone(BUZZER_PIN, 1000, 200);         // short beep at 1000 Hz
  } else {
    // Normal condition: close vent, LED off
    myServo.write(0);                    // servo back to 0 degrees
    digitalWrite(LED_PIN, LOW);          // LED off
  }

  if (lightLevel < LIGHT_LOW) {
    // Dark condition — add your logic here
    // e.g., turn on a grow light LED:
    // analogWrite(GROWLIGHT_PIN, 200);
  }

  // ---- UPDATE LCD DISPLAY ---------------------------------
  lcd.setCursor(0, 0);                    // top row
  lcd.print("T:");
  lcd.print(temperature, 1);             // 1 decimal place
  lcd.print("C H:");
  lcd.print(humidity, 0);                // 0 decimal places
  lcd.print("%");

  lcd.setCursor(0, 1);                    // bottom row
  lcd.print("Lux:");
  lcd.print(lightLevel);
  lcd.print(" D:");
  lcd.print(distanceCm);
  lcd.print("cm ");                       // trailing space clears old chars

  // ---- SERIAL DATA LOG ------------------------------------
  // Format: time, temp, humidity, light, distance (CSV-style)
  Serial.print(loopCounter * 2);         // approximate seconds
  Serial.print(", ");
  Serial.print(temperature);
  Serial.print(", ");
  Serial.print(humidity);
  Serial.print(", ");
  Serial.print(lightLevel);
  Serial.print(", ");
  Serial.println(distanceCm);

  loopCounter++;                         // increment time counter

  delay(2000);   // wait 2 seconds between readings
                 // (DHT11 needs at least 2 seconds between reads)
}

// =============================================================
// ===== CHALLENGE =====
//
// 1. Add a running AVERAGE of the last 5 temperature readings
//    instead of showing a single reading. Hint: use an array.
//
// 2. Add a BUTTON that, when pressed, resets the loop counter
//    (and therefore the time axis in your data log).
//
// 3. Write a function called checkAlarm() that encapsulates
//    all the if/else control logic — call it from loop().
//    This makes the code cleaner and easier to understand.
//
// 4. Add a second threshold (e.g., TEMP_VERY_HIGH = 35) and
//    create a 3-level response: normal / warm / hot.
// =============================================================
```

---

## 5. Student Worksheet

---
### Worksheet — Session 32: Build Day 2 — Coding

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### What I Already Know — Warm-Up Questions

1. What is the difference between `Serial.print()` and `Serial.println()`?
   `____________________________________________________________`

2. Write the Arduino syntax to read analog pin A0 and store the value in a variable called `sensorValue`:
   `____________________________________________________________`

3. What does `#define TEMP_THRESHOLD 28` do? Why is it better than writing `28` directly in the code?
   `____________________________________________________________`

---

#### Build It — Coding Milestones Checklist

Work through these milestones in order. Check off each one when you have tested and confirmed it works:

- [ ] 1. All `#include` library statements are at the top
- [ ] 2. All pin numbers defined as `#define` constants
- [ ] 3. All threshold values defined as `#define` constants
- [ ] 4. `setup()` initialises Serial, LCD, sensors, outputs
- [ ] 5. Sketch compiles without errors (0 errors in IDE)
- [ ] 6. Serial Monitor shows startup message
- [ ] 7. Sensor 1 reads correct values (test by changing conditions — e.g., cover LDR with hand)
- [ ] 8. Sensor 2 reads correct values
- [ ] 9. Control logic responds correctly (output changes when condition is met)
- [ ] 10. LCD shows current readings
- [ ] 11. Serial data log prints labelled readings (CSV format)
- [ ] 12. Code is fully commented (every section has a comment)

---

#### Predict!

1. Before you upload your code, what do you predict the DHT11 will read for temperature? _______ °C
   (After upload — actual reading: _______ °C. Were you close?)

2. When you cover the LDR with your hand, do you predict the analog reading will go UP or DOWN?
   Prediction: _________ Actual result: _________
   Why? `____________________________________________________________`

3. What do you think the HARDEST part of today's coding will be?
   `____________________________________________________________`

---

#### Observe / Data Table — Serial Monitor Test Readings

Record 5 readings from your Serial Monitor during testing:

| Reading # | Time (s) | Temp (°C) | Humidity (%) | Light (0–1023) | Distance (cm) |
|-----------|----------|-----------|--------------|----------------|---------------|
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

**Do these readings seem realistic? Explain any unexpected values:**
`____________________________________________________________`

---

#### What Did You Notice?

1. What was the first error you encountered? How did you fix it?
   `____________________________________________________________`

2. Did your control logic work as expected the first time? If not, what did you change?
   `____________________________________________________________`

3. What is the most interesting reading you got from a sensor today?
   `____________________________________________________________`

4. How does your code right now compare to your pseudocode plan?
   `____________________________________________________________`

---

#### Build Log Entry — Day 2

**Goal for today:** Write code and test sensor readings

**Milestones reached:** (list from the checklist above)
`____________________________________________________________`

**What worked well in the code:**
`____________________________________________________________`

**What errors did we encounter?**
`____________________________________________________________`

**How we fixed them:**
`____________________________________________________________`

**What still needs to be done (for Session 33):**
`____________________________________________________________`

---

#### Science Connection

> "Your Serial Monitor is doing exactly what a data logger does in real scientific equipment — recording measurements with timestamps so they can be analysed later. Weather stations, medical monitors, and space probes all use the same principle: measure → record → analyse. Today you built a data logger."

What pattern (if any) did you notice in your 5 test readings?
`____________________________________________________________`

---

#### Challenge Extension
*(For teams who reach all milestones early)*

Add a **running average** to your sensor reading. Instead of showing a single reading, maintain an array of the last 5 readings and display the average. This reduces the effect of noise (random errors) in the data — just like scientists run multiple trials.

```
// HINT: 
float readings[5] = {0, 0, 0, 0, 0};
int index = 0;

// In loop():
readings[index] = dht.readTemperature();
index = (index + 1) % 5;     // wrap around 0-4

float sum = 0;
for (int i = 0; i < 5; i++) sum += readings[i];
float average = sum / 5.0;
```

---

## 6. Safety Notes

- **Power down to recode.** If you need to change wiring to fix a hardware issue discovered during coding, unplug USB first.
- **Do not leave USB plugged in while a student steps away.** Unattended powered boards can overheat if code accidentally drives outputs continuously.
- **Buzzer volume:** If using `tone()`, keep frequencies in the 1000–4000 Hz range and durations short. Continuous tone at high frequency is disruptive and can be distressing for sound-sensitive students.
- **Servo caution:** Do not write `myServo.write(180)` and then `myServo.write(0)` in rapid succession (no delay) — this can draw excessive current. Always include a `delay(500)` or more between large servo movements.
- **DHT11 heat warning:** If DHT11 is misread constantly and the chip gets warm, this is a wiring issue — unplug and recheck.

| Symptom | Action |
|---------|--------|
| IDE shows red "Serial port not found" | USB not connected or wrong port — check Tools → Port |
| Upload fails with "avrdude: stk500v2" error | Board not in bootloader — press RESET on Mega, try again |
| Buzzer makes continuous noise without expected code | `tone()` called without `noTone()` — add `noTone(BUZZER_PIN)` after the tone block |
| Servo moves to extreme position and vibrates | `servo.write()` receiving a value outside 0–180 — clamp with `constrain()` |

---

## 7. Assessment Rubric

**Formative Check (contributes to Build Log and Code Quality grades):**

| Look-For | Observed? | Notes |
|----------|-----------|-------|
| Code compiles without errors | | |
| At least one sensor reads correctly and prints to Serial Monitor | | |
| Control logic (if/else) is present and tested | | |
| Code has meaningful comments throughout | | |
| Named constants (`#define`) used for pins and thresholds | | |
| Build Log Day 2 entry is specific and detailed | | |

**Grading note:** Code Quality is one of the five criteria in `Capstone-F-Final-Presentation-Rubric.md`. Key indicators: correct syntax, meaningful comments, named constants, logical structure, and evidence of testing/debugging (documented in Build Log).

---

## 8. Differentiation

### Support
- Provide the **skeleton code** (the file above) pre-loaded in the Arduino IDE with `// FILL IN HERE` comments marking exactly what each team needs to change — they adapt it, rather than writing from scratch.
- Create a **"library cheat sheet"** printed card showing the most common function calls for DHT.h, Servo.h, and LiquidCrystal_I2C.h.
- Allow teams to use **Tinkercad Circuits** (in-browser simulator) for initial code testing if hardware issues are blocking progress — they can debug the logic without needing the physical circuit.
- Break the task into only 2 milestones for struggling teams: "Get one sensor reading to print" and "Make one output respond to that reading" — celebrate those two wins before adding more.

### Extension
- Challenge the team to **implement functions** — move each major task (read sensors, check thresholds, update display, log data) into its own named function and call them from `loop()`. This teaches modular programming.
- Ask the team to research and implement **I2C scanning** to confirm the LCD's address programmatically, rather than guessing 0x27 vs 0x3F.
- Have the team modify their data log format to be **true CSV** (comma-separated values, with a header row) so it can be directly copied into a spreadsheet for graphing.

### Visual / Kinesthetic Accommodations
- Post the **incremental coding strategy** list (numbered 1–8) on the board or provide it as a printed card — this gives students a physical checklist to tick off as they go.
- For students who struggle with typing, allow voice-dictation tools for comment text (the most important parts to have correct are the actual code lines).
- Use **colour syntax highlighting** in the Arduino IDE — point out that keywords appear in orange, strings in red, comments in grey; this helps students visually parse code structure.
- Allow students to annotate a **printed copy of the skeleton code** with highlighters before typing — yellow = "this is mine," green = "I understand this," pink = "I need to change this."
