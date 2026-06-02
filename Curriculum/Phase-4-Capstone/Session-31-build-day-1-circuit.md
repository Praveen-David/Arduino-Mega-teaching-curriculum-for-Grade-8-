# Week 16, Session 31 — Build Day 1: Circuit Construction

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Build Day — Hardware / Circuit Construction

## Learning Objectives
- **Construct** the hardware portion of their capstone project following their circuit plan from Session 30.
- **Apply** the Build → Check → Power on safety mantra to a multi-component circuit of their own design.
- **Complete** a Build Log entry documenting progress, problems encountered, and solutions found.
- **Verify** that each component is correctly wired before powering on the Arduino for the first time.

**Science Curriculum Link:** Electrical Circuits & Systems — Students apply their understanding of circuit topology, polarity, voltage dividers, and signal paths (from Phases 1–3) to a self-designed multi-sensor circuit, reinforcing that engineering requires translating a plan into a physical system.

## Materials Checklist (per team)
- [ ] Arduino Mega 2560
- [ ] USB-A to USB-B cable
- [ ] Full-size breadboard (830 tie-points)
- [ ] Jumper wires (M-M and M-F assorted)
- [ ] All sensors / modules from the team's approved plan (e.g., DHT11, LDR, HC-SR04, potentiometer)
- [ ] All output components from the plan (e.g., LED(s) + 220 Ω resistors, servo SG90, passive buzzer, LCD I2C)
- [ ] Resistors: 220 Ω (LED current limiting), 10 kΩ (pull-down / voltage dividers)
- [ ] Component inventory card
- [ ] Completed Planning Template from Session 30 (the circuit sketch is the build guide)
- [ ] Printed copy of `Capstone-D-Build-Log-Template.md` (1 per student)
- [ ] Pencil for build log
- [ ] Multimeter (shared — for voltage checks if needed)

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Hold up a completed circuit (from Phase 3, or a pre-built teacher demo of a simple multi-sensor board).

**Ask:**
> "What is the FIRST thing you do before you add a single wire? What is the LAST thing you do before you power on?"

Expected answers: Power down first / check polarity / partner check before powering on.

**Reinforce the mantra (say it together):** "Build → Check → Power on."

**Bridge (say this):**
> "Today the training wheels come off. Your planning template is your guide. Build exactly what you planned. If the plan has a mistake, that's OK — find it now, fix it, and note it in your build log. That IS the science."

---

### 5–15 min — Direct Instruction: Pre-Build Review

**Give teams 5 minutes to review their planning templates and answer these pre-build questions (write on board):**
1. Which component goes on the breadboard first? (Power rails, then Arduino connections, then sensors)
2. Which components are polarized? (Mark them on your plan — LEDs, electrolytic caps, DHT11)
3. Which pins need to be PWM (~) pins? (Servo, LED dimming, buzzer tone)
4. Which components share the I2C bus? (LCD → SDA pin 20, SCL pin 21)
5. Where will the 5V and GND rails connect on the breadboard?

**Model the build sequence (say this and demonstrate on the teacher's board):**
> "Step 1: Connect power rails — red wire from Mega 5V to breadboard positive rail, black wire from Mega GND to breadboard negative rail. Step 2: Place each module. Step 3: Add signal wires one at a time. Step 4: Do the partner check. Step 5: Power on."

Address any questions teams have about their specific circuit plans. If any team's plan has a critical error (identified in your Session 30 review), pull them aside now for a 2-minute redirect.

---

### 15–35 min — Hands-On Build: Circuit Construction

**Teams follow their planning template circuit sketch. Numbered process for students:**

1. **Clear the breadboard** — start fresh, no leftover components from Phase 3.
2. **Power rails first** — red wire: Mega 5V → breadboard positive (+) rail. Black wire: Mega GND → breadboard negative (−) rail.
3. **Place the Arduino** at the edge of the workspace — do not insert it into the breadboard (Mega is too large).
4. **Place modules on the breadboard** one at a time following the plan.
5. **Wire each module** — VCC to red rail, GND to black rail, signal wire to the designated Mega pin.
6. **Check polarity** on each polarized component before moving to the next.
7. **LCD I2C last** (if used) — connect SDA to pin 20, SCL to pin 21, VCC to 5V, GND to GND.
8. **Servo** (if used) — brown wire to GND, red wire to 5V, orange signal wire to a PWM (~) pin.
9. **Double-check** every connection against the planning template pin table.
10. **Partner check** — builder reads connections aloud; coder traces each wire on the board.
11. **Only after partner check: plug in USB** and watch for any burning smell, smoke, or hot components.

**Teacher circulates constantly** during this phase. Prioritise teams using:
- Sensors they have less experience with
- More complex circuit plans (3+ components)
- Students who struggled during Phase 3

**Common issues to watch for:**
- Power and GND rails not connected to the Mega
- DHT11 or sensors wired with VCC/GND/Signal in wrong order (always check the module's own labels)
- LCD I2C address mismatch (most modules use 0x27 — will be fixed in Session 32)
- Servo signal wire on a non-PWM pin

---

### 35–42 min — Testing & Debugging

At this stage (end of Build Day 1), the code is not yet written — students are testing the **hardware only**.

**Hardware-only checks (no code needed):**

| Test | How to Check | Success Indicator |
|------|-------------|-------------------|
| Power rails live | Multimeter: probe + rail and − rail | Should read ~5V DC |
| No short circuit | After plugging in USB: board LED on, no heat, no smell | Green power LED on Mega stays on |
| LCD backlight (if fitted) | Plug in USB with I2C connected (no code needed for backlight) | LCD backlight glows |
| Servo (if fitted) | With USB power only, servo may twitch briefly on power-up — this is normal | No burning smell |
| LED orientation | Multimeter in diode-check mode, touch anode/cathode | Beep = correct polarity |

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Mega power LED off after plugging in | Short circuit somewhere | Unplug immediately; remove all wires from 5V rail one at a time until short is isolated |
| Component is warm/hot immediately | Wrong polarity or missing resistor | Unplug; reverse the component; add the missing resistor |
| LCD backlight not on | VCC/GND swapped or I2C wired to wrong pins | Unplug; check SDA=20, SCL=21; check VCC=5V rail |
| Servo buzzing continuously without code | Signal pin is floating (picking up noise) | Add a 10 kΩ pull-down resistor on the signal pin, or leave signal disconnected until code session |
| DHT11 getting hot | VCC and GND swapped | Unplug immediately; reverse the connection |

**What success looks like at end of Build Day 1:**
- All components physically placed and wired on the breadboard.
- No short circuits, no hot components.
- Board powers on with green LED lit.
- Build Log entry for Day 1 is filled in.

---

### 42–45 min — Reflection / Exit Ticket

**Students fill in Build Log Entry #1 (`Capstone-D-Build-Log-Template.md`) — collect at the end or photograph:**

1. What did you complete today?
2. What worked as expected?
3. What didn't go as planned? How did you fix it?
4. What is your plan for Session 32 (Build Day 2)?

**Quick verbal exit — teacher asks the class:**
> "One word: how does your circuit feel right now — confident, uncertain, or something else?"
Take 5 quick answers and note teams that say "uncertain" or similar — prioritise them at the start of Session 32.

---

## 3. Circuit Diagram

Each team's circuit is unique to their project. The diagram below shows the general structure of a multi-sensor capstone circuit and applies to most projects.

**NOTE: Wire colours vary by project — the conventions ALWAYS hold:**
- **Red wire = 5V / power**
- **Black wire = GND**
- **Other colours = signal wires (one colour per signal line where possible)**

```
  GENERAL CAPSTONE CIRCUIT STRUCTURE (Arduino Mega 2560)
  =========================================================

                    [ ARDUINO MEGA 2560 ]
                   ┌─────────────────────┐
        5V ────────┤ 5V              GND ├──────── GND
                   │                     │
  SENSOR 1         │  A0 (analog in)     │
  (e.g. LDR)  ─────┤                     │
                   │  A1 (analog in)     │
  SENSOR 2    ─────┤                     │
  (e.g. pot)       │  2  (digital in)    │
                   │                     │
  SENSOR 3    ─────┤  (see plan)         │
  (e.g. DHT11)     │                     │
                   │  ~9 (PWM out)       ├──── Servo signal (orange)
                   │                     │
                   │  ~6 (PWM out)       ├──── LED anode → 220 Ω → GND
                   │                     │
                   │  ~3 (PWM out)       ├──── Buzzer +
                   │                     │
                   │  20 SDA             ├──── LCD I2C SDA (blue)
                   │  21 SCL             ├──── LCD I2C SCL (yellow)
                   └─────────────────────┘

  BREADBOARD POWER RAILS:
  (+) rail ─── red wire ──────────────────── 5V pin on Mega
  (−) rail ─── black wire ─────────────────── GND pin on Mega

  ALL modules: VCC → (+) red rail, GND → (−) black rail

  HC-SR04 (if used):
    VCC → (+) rail (red)
    GND → (−) rail (black)
    TRIG → any digital pin (e.g., pin 7) — signal wire (yellow)
    ECHO → any digital pin (e.g., pin 8) — signal wire (green)

  SERVO SG90 (if used):
    Red wire  → (+) 5V rail
    Brown wire → (−) GND rail
    Orange signal → PWM (~) pin (e.g., pin 9)

  LCD I2C (if used):
    VCC → (+) 5V rail (red)
    GND → (−) GND rail (black)
    SDA → Mega pin 20 (blue wire)
    SCL → Mega pin 21 (yellow wire)

  LDR VOLTAGE DIVIDER (if used):
    5V → LDR → junction point → A0
    junction point → 10 kΩ resistor → GND

  DHT11 MODULE (3-pin module):
    VCC → (+) rail (red)
    GND → (−) rail (black)
    DATA → digital pin (e.g., pin 4) (yellow or green wire)
```

**Team-specific notes:**
- Each team replaces the generic "SENSOR 1/2/3" with their actual sensors and pin assignments from their planning template.
- Circuits that differ from this general structure should be checked by the teacher before powering on.

[Google image search: "Arduino Mega 2560 multiple sensors breadboard circuit"]

---

## 4. Arduino Code

No complete code is uploaded on Build Day 1. The code below is a **skeleton / stub** that teams can have open in the Arduino IDE to confirm the board connects and uploads correctly after the hardware is checked.

```cpp
// =============================================================
// CAPSTONE PROJECT — BUILD DAY 1: HARDWARE CHECK SKETCH
// Team: _______________________  Date: _______________
//
// PURPOSE: This minimal sketch confirms the Arduino Mega
//          uploads correctly and the basic hardware is alive.
//          Real project code will be written in Session 32.
//
// SCIENCE: Every sensor needs a working circuit BEFORE code
//          can make sense of the data. Hardware first!
// =============================================================

// --- STEP 1: Note your pin assignments here as comments ---
// SENSOR 1 (name: _______): pin ___
// SENSOR 2 (name: _______): pin ___
// OUTPUT 1 (name: _______): pin ___
// OUTPUT 2 (name: _______): pin ___
// LCD I2C: SDA = 20, SCL = 21

// --- STEP 2: A hardware-check blink on the built-in LED ---
// The built-in LED is on pin 13 of the Mega.
// If this blinks, your board is alive and uploading correctly.

void setup() {
  Serial.begin(9600);           // Open the Serial Monitor at 9600 baud
  pinMode(13, OUTPUT);          // Built-in LED as output
  Serial.println("=== Build Day 1 Hardware Check ===");
  Serial.println("If you see this, the board is alive!");
}

void loop() {
  digitalWrite(13, HIGH);       // Built-in LED on
  delay(500);                   // Wait half a second
  digitalWrite(13, LOW);        // Built-in LED off
  delay(500);                   // Wait half a second
  Serial.println("Heartbeat — hardware check running.");
}

// =============================================================
// >>> NEXT SESSION (Build Day 2):
//     Replace this skeleton with your real project code.
//     Use your pseudocode plan as your guide.
// =============================================================
```

**Instructions for teams (optional, if time allows):**
1. Open the Arduino IDE.
2. Set Board = Arduino Mega 2560, Port = the correct COM/serial port.
3. Upload this sketch.
4. Open Serial Monitor (9600 baud).
5. Confirm you see "Heartbeat" printing and the built-in LED blinking.
6. This confirms the board is working — even before the project code is written.

---

## 5. Student Worksheet

---
### Worksheet — Session 31: Build Day 1 — Circuit Construction

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### What I Already Know — Warm-Up Questions

1. What are the three steps of the **Build → Check → Power on** mantra and why does the order matter?
   `____________________________________________________________`
   `____________________________________________________________`

2. On the Arduino Mega, which pins are the I2C data and clock pins?
   SDA = pin _______ SCL = pin _______

3. In a voltage divider for an LDR, what happens to the voltage at the junction as the light gets brighter?
   `____________________________________________________________`

---

#### Build It — Step by Step

Use your Planning Template circuit sketch as your guide. Check off each step as you complete it:

- [ ] 1. Cleared the breadboard — starting fresh
- [ ] 2. Connected 5V rail: red wire from Mega 5V to breadboard (+) rail
- [ ] 3. Connected GND rail: black wire from Mega GND to breadboard (−) rail
- [ ] 4. Placed Sensor 1 (______________________) on breadboard; wired VCC, GND, signal
- [ ] 5. Placed Sensor 2 (______________________) on breadboard; wired VCC, GND, signal
- [ ] 6. Wired Output 1 (______________________) with correct resistor/signal
- [ ] 7. Wired Output 2 (______________________) with correct signal
- [ ] 8. If LCD: SDA → pin 20, SCL → pin 21, VCC, GND connected
- [ ] 9. All polarized components oriented correctly (LEDs, DHT11, etc.)
- [ ] 10. Partner check completed — builder read connections aloud, coder traced each wire
- [ ] 11. Teacher check (or pair from another team checked) ☐ Teacher initials: _______
- [ ] 12. Plugged in USB — green power LED on Mega lit, no smell, no heat

---

#### Predict!

1. Before you power on, do you think the hardware will work first try? Why or why not?
   `____________________________________________________________`

2. Which part of the circuit are you MOST uncertain about? Why?
   `____________________________________________________________`

3. If a component gets warm right after powering on, what should you do first?
   `____________________________________________________________`

---

#### Observe / Data Table — Hardware Check Log

| Test Performed | Result (pass/fail/note) | Action Taken if Failed |
|----------------|------------------------|------------------------|
| Power rails connected (5V to +, GND to −) | | |
| Mega green LED lights on USB connect | | |
| No hot/warm components after 30 sec | | |
| LCD backlight visible (if applicable) | | |
| Sensor 1 physically placed correctly | | |
| Sensor 2 physically placed correctly | | |
| Hardware-check sketch uploaded & blinks | | |
| Serial Monitor shows "Heartbeat" message | | |

---

#### What Did You Notice?

1. Did the circuit power on as expected? If not, what was the problem and how did you fix it?
   `____________________________________________________________`

2. Did you need to make any changes to your circuit plan during the build? Describe them:
   `____________________________________________________________`

3. Which step took the most time? Why?
   `____________________________________________________________`

4. What is ONE thing you would do differently if you were starting the build over?
   `____________________________________________________________`

---

#### Build Log Entry — Day 1

*(Complete this in your `Capstone-D-Build-Log-Template.md` as well — keep both copies)*

**Goal for today:** Build the hardware circuit

**What we completed:**
`____________________________________________________________`

**What worked:**
`____________________________________________________________`

**What did NOT work / problems we hit:**
`____________________________________________________________`

**How we solved the problem(s):**
`____________________________________________________________`

**Plan for Session 32 (Build Day 2 — Coding):**
`____________________________________________________________`

---

#### Science Connection

> "A circuit on a breadboard is a physical model of an electrical system. Scientists and engineers build physical models to test ideas before committing to a final product. The errors you find on a breadboard today would be expensive and dangerous if found in a real-world system (like a hospital monitor or a satellite)."

What "model" are you building and what real-world system does it relate to?
`____________________________________________________________`
`____________________________________________________________`

---

#### Challenge Extension
*(For teams who finish the hardware build early)*

- Extend the hardware-check sketch: add one `analogRead()` on your sensor's pin and print the raw value to the Serial Monitor. Is the reading within the expected range for that sensor?
- Draw a neat, labelled final circuit diagram (in pencil with coloured pencils for wire colours) to replace your rough plan sketch. This becomes the official "schematic" in your final presentation poster.

---

## 6. Safety Notes

**This is the first live-build session. Reinforce these critical rules:**

- **Build → Check → Power on.** Students MUST complete the partner check before plugging in USB. Teacher should spot-check at least half the teams before they power on.
- **Polarity is critical today.** DHT11, servo, LEDs — all polarized. Reversed polarity can damage components or create heat. If a component gets warm/hot within 5 seconds of power-on: unplug immediately.
- **No bare wire ends touching.** Trim any excessively long jumper wire ends. A floating bare wire can cause intermittent shorts.
- **Servo handling:** Keep fingers clear of the servo horn during initial power-on. When the signal pin is unconnected, servos sometimes twitch unpredictably.
- **LCD I2C:** If the LCD gets very hot, check the I2C address and wiring — incorrect VCC (e.g., accidentally connecting 3.3V or VIN instead of 5V) can cause issues.

| Symptom | Action |
|---------|--------|
| Burning smell / visible smoke | Unplug USB immediately. Do not touch components. Notify teacher. Ventilate area. |
| Component hot to the touch | Unplug. Do not touch. Let cool 30 seconds. Check polarity before re-wiring. |
| Mega power LED does not come on | Possible short. Unplug. Remove 5V connections one at a time until LED comes back on — that last wire was the short. |
| Arduino repeatedly resetting (LED flashes erratically) | Brown-out from too much current draw — check if servo or multiple modules are all pulling from the 5V pin at once (use external power for servo if needed). |

---

## 7. Assessment Rubric

**Formative Check (contributes to Build Log grade) — Teacher Look-Fors During Session 31:**

| Look-For | Observed? | Notes |
|----------|-----------|-------|
| Team follows Build → Check → Power on sequence correctly | | |
| Partner check performed before USB plugged in | | |
| Components wired according to the planning template | | |
| Polarized components oriented correctly | | |
| Team troubleshoots methodically (unplug, identify, fix, recheck) | | |
| Build Log Day 1 entry is filled in with specific details | | |

**Grading note:** The **Build Log** (`Capstone-D`) is a graded deliverable (collected at the end of Phase 4). Each day's entry contributes. Build Day 1 entry should document: what was built, what worked, what didn't, how problems were solved, and the plan for next time.

---

## 8. Differentiation

### Support
- Provide a **pre-drawn circuit diagram** based on the team's approved plan (teacher draws a clean version of their sketch) — students wire from this rather than from their rough sketch.
- Allow struggling teams to build a **reduced-scope circuit first** (just the power rails + one sensor) and power-test before adding more components.
- Pair teams with a strong Phase 3 builder as a peer mentor for the first 10 minutes of the build.
- Provide the hardware-check sketch pre-typed and open in the Arduino IDE so teams can upload it immediately without typing.

### Extension
- Challenge the team to add **decoupling capacitors** (0.1 µF ceramic) across the VCC/GND pins of each module — explain why these reduce noise.
- Have the team write the complete formal circuit diagram with proper schematic symbols (not just a breadboard diagram) for inclusion in their final presentation.
- Ask the team to calculate the **total current draw** of all their components and confirm the Arduino Mega's 5V pin can supply it (the Mega's onboard regulator can supply up to ~800 mA from USB).

### Visual / Kinesthetic Accommodations
- Provide a **physical colour-coding guide**: a printed card that shows red = 5V, black = GND with photos of a correctly-coloured breadboard for reference.
- Allow students to mark their breadboard rows with small pieces of coloured tape to visually identify power rails before wiring.
- For students who find the physical dexterity of breadboarding difficult, assign them as the "Quality Controller" who reads the plan aloud and traces wires while their partner places them.
- Students who have visual processing difficulties can use larger jumper wires with distinct colours rather than all-same-colour packs.
