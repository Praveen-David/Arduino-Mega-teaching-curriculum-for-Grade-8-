# Week 17, Session 33 — Build Day 3: Integration & Testing

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Build Day — Integration, System Testing, Debugging

## Learning Objectives
- **Integrate** the completed hardware circuit and project code into a single functioning system.
- **Test** the system as a whole, identifying and resolving integration-level bugs (problems that only appear when all parts run together).
- **Apply** systematic debugging strategies to isolate which component or code section is causing a problem.
- **Document** integration test results in the Build Log with specific observations.

**Science Curriculum Link:** Systems Thinking — A system is more than the sum of its parts; interactions between components create emergent behaviours. Students discover that integration testing (checking how parts work together) is a distinct step from unit testing (checking parts separately) — a key concept in both engineering and biology (e.g., how individual organs form a body system).

## Materials Checklist (per team)
- [ ] Arduino Mega 2560 + USB cable
- [ ] Completed circuit from Sessions 31–32 (hardware + wiring)
- [ ] Laptop with Arduino IDE and project code from Session 32
- [ ] Build Log template (Day 3 entry)
- [ ] Planning Template (circuit sketch and pseudocode for reference)
- [ ] Multimeter (shared — for circuit diagnostics)
- [ ] Spare components (resistors, jumper wires — from classroom spares bin)
- [ ] Notebooks / project binders

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

**Show a split image (on projector or describe verbally):**
Left side: a car engine in perfect condition. Right side: a car that won't start.

**Ask:**
> "An engineer says: 'Every single part of this car passed its individual test. The engine works. The battery is fine. The ignition is fine. But the car won't start.' What is the most likely kind of problem?"

Expected answers: a connection between parts, a wire to the wrong place, a timing issue.

**Bridge (say this):**
> "That is an integration problem. Your sensor reads perfectly. Your code compiles perfectly. Your circuit looks fine. But when you run them together — something unexpected happens. That's the most common kind of bug in real engineering, and today we hunt it down systematically."

---

### 5–15 min — Direct Instruction: Integration Testing Strategy

**Write the four-step integration test process on the board:**

**Step 1 — Confirm the baseline**
Upload the working Session 32 code. Open Serial Monitor. Confirm all sensors are printing values. If not, fix that first before testing outputs.

**Step 2 — Test one output at a time**
Comment out ALL control logic. Force each output manually:
```cpp
// Test the servo by itself:
myServo.write(90);   // does it move?
delay(1000);
myServo.write(0);    // does it return?
```
Confirm each output works before reconnecting it to the control logic.

**Step 3 — Reconnect control logic one section at a time**
Uncomment one `if/else` block. Test. Then uncomment the next. This isolates which block causes problems.

**Step 4 — Full system run**
All sections active. Run for at least 3 minutes. Watch for:
- Sensor values drifting unexpectedly
- Outputs responding at wrong times
- LCD freezing or showing wrong values
- Arduino resetting by itself (power issue)

**Common integration problems (model on the board):**
| Problem | Root Cause | Fix |
|---------|-----------|-----|
| Servo moves when it shouldn't | Threshold too sensitive | Adjust `#define TEMP_HIGH` value |
| LCD freezes after a few seconds | I2C bus conflict or too many library calls | Add `lcd.clear()` less often; reduce frequency |
| DHT11 returns NaN every few loops | Delay too short between reads | Ensure `delay(2000)` — DHT11 needs 2+ seconds |
| Buzzer sounds constantly | `tone()` called every loop iteration without `noTone()` | Add `noTone()` after tone; use a flag variable |
| Arduino resets randomly | Too much current draw | Power servo separately; add delay between outputs |

**Say this to the class:**
> "If something strange happens and you don't understand why — the first step is always: add a `Serial.println()` message before and after every section. Let the code tell you where it's getting stuck."

---

### 15–35 min — Hands-On: Integrate, Test, Debug

**Teams follow the four-step integration test process.**

**Teacher circulates with this checklist (check each team at least twice):**
- Are both sensors reading correct values?
- Do outputs respond to the right conditions?
- Is the LCD displaying the correct data?
- Is Serial Monitor logging clean, readable data?
- Are there any unexpected behaviours? What are they?

**Encourage teams who finish quickly to:**
- Run the system for 5 full minutes and record the Serial Monitor output in their data table
- Check whether sensor values are stable (consistent) or noisy (jumping around)
- Try deliberately triggering a threshold (e.g., hold a warm hand near DHT11 to cross the temperature threshold)

**Teams who are stuck:** Use the "Serial print everywhere" strategy. Guide them to add diagnostic print statements between every section to find exactly where the program is behaving unexpectedly.

**Key integration fixes to know:**

**Fix: Buzzer sounds every loop**
```cpp
// Instead of:
tone(BUZZER_PIN, 1000, 200);    // called every 2 seconds — annoying!

// Use a flag so it only sounds ONCE when condition is first triggered:
bool alarmActive = false;

if (temperature > TEMP_HIGH && !alarmActive) {
  tone(BUZZER_PIN, 1000, 500);
  alarmActive = true;
} else if (temperature <= TEMP_HIGH) {
  noTone(BUZZER_PIN);
  alarmActive = false;
}
```

**Fix: LCD flickering**
```cpp
// Instead of calling lcd.clear() every loop, only update changed values:
// Use lcd.setCursor(col, row) and overwrite the specific positions
// Pad numbers with spaces to clear old characters:
lcd.setCursor(3, 0);
lcd.print(temperature, 1);
lcd.print("  ");    // two trailing spaces clear any leftover digits
```

---

### 35–42 min — Testing & Debugging (Formal Integration Test)

**Each team runs a 3-minute formal test run:**

1. Upload the most recent code.
2. Start a fresh Serial Monitor session.
3. Let the system run for 3 minutes without touching it.
4. Then: trigger each threshold manually (warm the sensor, cover the LDR, hold hand near HC-SR04) and observe the response.
5. Record results in the worksheet data table.

**Scoring the integration test (teams self-assess):**

| Feature | Working? | Notes |
|---------|---------|-------|
| All sensors reading | ✓ / ✗ | |
| At least one output responding correctly | ✓ / ✗ | |
| LCD displaying data | ✓ / ✗ | |
| Serial Monitor logging | ✓ / ✗ | |
| System runs 3 min without crashing | ✓ / ✗ | |

**What success looks like:**
- All 5 items above check ✓.
- Teams who have all 5 checked are ready for Build Day 4 (data collection).
- Teams with any ✗ items have a clear fix list for the start of Session 34.

---

### 42–45 min — Reflection / Exit Ticket

**In project binders:**

1. List any integration bugs you found today and how you fixed them (or plan to fix them):
   `____________________________________________________________`

2. Did the system behave differently when all parts ran together vs. tested separately? Describe:
   `____________________________________________________________`

3. Is your project ready for data collection (Session 34)? If not, what specifically needs fixing?
   `____________________________________________________________`

4. What is the most impressive thing your project does right now?
   `____________________________________________________________`

---

## 3. Circuit Diagram

The circuit is unchanged from Build Day 1 (Session 31), except for any fixes made based on Session 32 testing.

```
  BUILD DAY 3: SAME CIRCUIT AS SESSIONS 31–32
  ==============================================
  No new wiring is expected today.

  If integration testing reveals a hardware problem,
  POWER DOWN before making any changes.

  COMMON INTEGRATION HARDWARE FIXES:
  ┌───────────────────────────────────────────────────────┐
  │ Problem Found          │ Hardware Fix                 │
  ├───────────────────────────────────────────────────────┤
  │ Buzzer sounds all time │ Add 100Ω resistor in series  │
  │ LDR value not changing │ Check 10kΩ to GND is present │
  │ Servo jittering        │ Add 10µF cap across servo power│
  │ DHT11 NaN             │ Check DATA wire is on correct pin│
  │ LCD shows stray chars  │ Slower update, add spaces    │
  └───────────────────────────────────────────────────────┘

  SYSTEM BLOCK DIAGRAM (general — fill in your project's specifics):

  [DHT11]──────────────────────┐
                               │  reads
  [LDR]───────────────────────►│
                               │       ┌───────────►[SERVO]
  [HC-SR04]───────────────────►│ MEGA  │
                               │       ├───────────►[LED]
  (other sensors)─────────────►│       │
                               │       ├───────────►[BUZZER]
                               └───────┤
                                       ├───────────►[LCD]
                                       └───────────►[SERIAL LOG]
```

[Google image search: "Arduino Mega multi-sensor system integration testing breadboard"]

---

## 4. Arduino Code

The full project code from Session 32 continues here. The most important addition for Session 33 is **integration debugging tools**: diagnostic print statements and flag variables to prevent unwanted repeated triggers.

```cpp
// =============================================================
// CAPSTONE PROJECT — BUILD DAY 3: INTEGRATION CODE ADDITIONS
// Add these patterns to your Session 32 code as needed.
// =============================================================

// ---- ADDITION 1: FLAG VARIABLE to prevent buzzer looping ---
// Add this to your global variables section:
bool alarmTriggered = false;    // tracks whether alarm has sounded

// In your loop() control logic, replace simple tone() call with:
if (temperature > TEMP_HIGH) {
  myServo.write(90);
  digitalWrite(LED_PIN, HIGH);

  if (!alarmTriggered) {        // only beep ONCE when threshold crossed
    tone(BUZZER_PIN, 1000, 300);
    alarmTriggered = true;
  }
} else {
  myServo.write(0);
  digitalWrite(LED_PIN, LOW);
  noTone(BUZZER_PIN);
  alarmTriggered = false;       // reset flag when condition clears
}

// ---- ADDITION 2: LCD STABLE UPDATE (no flickering) ----------
// Replace lcd.clear() + full rewrite with targeted updates:
void updateDisplay(float temp, float hum, int light, int dist) {
  // Row 0: temperature and humidity
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(temp, 1);
  lcd.print("C ");             // extra space clears previous characters
  lcd.print("H:");
  lcd.print(hum, 0);
  lcd.print("%  ");

  // Row 1: light level and distance
  lcd.setCursor(0, 1);
  lcd.print("L:");
  lcd.print(light);
  lcd.print("  D:");
  lcd.print(dist);
  lcd.print("cm  ");
}
// Call this from loop() instead of writing to LCD directly.

// ---- ADDITION 3: SERIAL DIAGNOSTIC MODE --------------------
// Add a debug flag at the top of your sketch:
#define DEBUG true     // set to false to turn off extra debug prints

// Use it like this:
if (DEBUG) {
  Serial.print("DEBUG - loopCount: ");
  Serial.println(loopCounter);
}

// ---- ADDITION 4: CONSTRAIN SERVO VALUES --------------------
// Prevent servo from going outside safe range:
int servoAngle = map(temperature, 15, 40, 0, 180); // map temp to servo angle
servoAngle = constrain(servoAngle, 0, 180);         // limit to safe range
myServo.write(servoAngle);

// >>> TRY CHANGING THIS: map() lets you proportionally control
// the servo based on the sensor value — not just on/off!

// =============================================================
// ===== CHALLENGE =====
//
// 1. Make the LED brightness proportional to temperature
//    using analogWrite() instead of just on/off:
//    int brightness = map(temperature, 20, 40, 0, 255);
//    analogWrite(LED_PIN, brightness);
//
// 2. Display different messages on the LCD depending on the
//    current state ("NORMAL", "WARNING", "ALERT").
//
// 3. Add a "data mode / control mode" toggle using a button:
//    press once to show data on LCD, press again to show system
//    status.
// =============================================================
```

---

## 5. Student Worksheet

---
### Worksheet — Session 33: Build Day 3 — Integration & Testing

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### What I Already Know — Warm-Up Questions

1. What is the difference between **unit testing** (testing one part) and **integration testing** (testing the full system)?
   `____________________________________________________________`

2. If your Serial Monitor suddenly shows nothing (blank screen), what are 3 possible causes?
   1. `____________________________________________________________`
   2. `____________________________________________________________`
   3. `____________________________________________________________`

3. What does `constrain(value, min, max)` do in Arduino code? Why is it useful for servos?
   `____________________________________________________________`

---

#### Build It — Integration Test Steps

Follow these steps in order. Check each off when complete:

- [ ] 1. Upload Session 32 code — confirm it compiles and uploads without errors
- [ ] 2. Open Serial Monitor — confirm sensor values are printing
- [ ] 3. Commented out control logic — tested each output independently
- [ ] 4. Servo test: moves to 90° and back to 0° on command ✓/✗
- [ ] 5. LED test: turns on and off on command ✓/✗
- [ ] 6. Buzzer test: sounds once and stops on command ✓/✗
- [ ] 7. LCD test: shows correct values on both rows ✓/✗
- [ ] 8. Reconnected control logic one section at a time — tested each
- [ ] 9. Full system ran for 3 minutes without crashing ✓/✗
- [ ] 10. Manually triggered threshold conditions — observed correct responses ✓/✗

---

#### Predict!

1. Before running the full integration test, predict: what will be the FIRST problem you find?
   `____________________________________________________________`

2. When you hold a warm hand near the DHT11 to push it above the threshold, what do you predict will happen?
   `____________________________________________________________`

3. Do you think your system will run for 3 minutes without crashing? Explain:
   `____________________________________________________________`

---

#### Observe / Data Table — 3-Minute Integration Test

Record the state of the system at 30-second intervals during the formal test run:

| Time (s) | Temp (°C) | Light (0–1023) | Dist (cm) | Servo pos. (°) | LED (on/off) | Any issues? |
|----------|-----------|----------------|-----------|----------------|--------------|-------------|
| 0 | | | | | | |
| 30 | | | | | | |
| 60 | | | | | | |
| 90 | | | | | | |
| 120 | | | | | | |
| 150 | | | | | | |
| 180 | | | | | | |

**Manually triggered tests:**

| Test Action | Expected Response | Actual Response | Pass? |
|-------------|-------------------|-----------------|-------|
| Warm DHT11 above threshold | Servo opens, LED on, beep | | |
| Cover LDR with hand | (your expected result) | | |
| Hold hand near HC-SR04 | (your expected result) | | |

---

#### What Did You Notice?

1. What integration bugs did you find? How did you fix them?
   `____________________________________________________________`

2. Was there any behaviour that surprised you? What do you think caused it?
   `____________________________________________________________`

3. Did the system behave consistently (same result every time the condition was met)?
   `____________________________________________________________`

4. What is the MOST IMPORTANT thing you need to fix or improve before data collection?
   `____________________________________________________________`

---

#### Build Log Entry — Day 3

**Goal for today:** Integrate hardware + code and test as a complete system

**Integration test result** (circle): PASS / MOSTLY WORKING / SIGNIFICANT ISSUES

**What worked:**
`____________________________________________________________`

**Bugs found and how fixed:**
`____________________________________________________________`

**What is still unresolved:**
`____________________________________________________________`

**Plan for Session 34 (Data Collection):**
`____________________________________________________________`

---

#### Science Connection

> "Integration testing is how NASA checks the International Space Station before astronauts board it — every system works independently, but they must also work together flawlessly. In biology, a 'systems' view of the body recognizes that the heart works with the lungs and blood to form the circulatory+respiratory system — no one part is enough alone."

How does your project demonstrate the idea that "the whole is more than the sum of its parts"?
`____________________________________________________________`
`____________________________________________________________`

---

#### Challenge Extension

Add a **"system status" function** that runs every 10 loops (20 seconds) and prints a summary:
```
=== SYSTEM STATUS ===
Sensor 1: OK / ERROR
Sensor 2: OK / ERROR
Outputs: All responding / [list issues]
Loop count: [number]
Uptime: [seconds]
====================
```
This mirrors the kind of health-check logs used in real embedded systems.

---

## 6. Safety Notes

- **Powered board + wiring changes = danger.** Reinforce: any wiring changes today must follow Build → Check → Power on. Even tiny adjustments (moving one wire) require unplugging first.
- **Servo continuous running:** A servo jammed against a mechanical stop will draw excessive current and overheat. If your project has a physical vent/door the servo controls, make sure the servo can complete its full intended travel without obstruction.
- **Multiple components simultaneously:** With all components active at once, total current draw increases. If the Mega resets randomly, this is a sign of brownout (too much current). Solution: use a 9V battery barrel-jack to power the Mega (not just USB), or power the servo from a separate supply.
- **Prolonged runs:** During the 3-minute test, someone must watch the board at all times. Do not leave a powered board unattended.

| Symptom | Action |
|---------|--------|
| Board resets during full test | Check current draw; power from 9V barrel jack; consider removing one output temporarily |
| LCD shows random characters during full run | I2C timing issue — increase delay between LCD updates; call `lcd.clear()` less often |
| Servo gets hot | Servo is stalling — check mechanical travel; reduce movement frequency |
| Unexpected sensor spikes | Check for loose wires — a partially-disconnected jumper can cause false readings |

---

## 7. Assessment Rubric

**Formative Check (contributes to Build Log and Engineering Design grades):**

| Look-For | Observed? | Notes |
|----------|-----------|-------|
| Team tests each output independently before full integration | | |
| Integration test 3-minute run performed and recorded | | |
| Bugs identified and at least one fixed or fix plan documented | | |
| Serial Monitor shows clean, labelled, consistent data | | |
| Build Log Day 3 entry is detailed and specific | | |
| Team has a clear plan for what needs to be done before data collection | | |

---

## 8. Differentiation

### Support
- Provide a **"Debug Decision Tree"** printed card:
  ```
  System not working?
  └─ Does it upload? No → check port, cable, board selection
  └─ Does Serial Monitor show sensor values? No → fix sensor wiring
  └─ Does output respond? No → test output independently first
  └─ Still not working? → Add Serial.println() before and after every block
  ```
- Reduce scope: allow struggling teams to demonstrate **partial integration** (2 of 3 sensors working, 1 of 2 outputs working) and document the reason the full system isn't complete — partial credit exists.
- Sit with the team for 5 minutes and walk through their code line-by-line out loud — often hearing it spoken aloud reveals the bug immediately.

### Extension
- Challenge the team to implement a **state machine** — the system has named states (e.g., "NORMAL," "WARNING," "ALERT") and transitions between them based on sensor readings.
- Ask the team to calculate how much **data** their Serial log produces per hour (bytes per reading × readings per second × 3600) — how long until the Serial Monitor buffer fills? What would a real data logger do?
- Have the team create a **"user manual"** for their device — a one-page document explaining how to operate it, what the outputs mean, and what to do if something goes wrong.

### Visual / Kinesthetic Accommodations
- Write the **four-step integration test process** in large text on the board and leave it visible throughout the session.
- Allow students to **colour-code their code** with highlighters on a printed copy: yellow = sensors, green = control logic, blue = outputs, pink = data logging. Then they can see the structure visually.
- For students who struggle to stay focused during debugging (which can feel frustrating and repetitive): give them a specific role — "your job is to watch the Serial Monitor and call out when a value changes unexpectedly" — this keeps them engaged with a concrete task.
