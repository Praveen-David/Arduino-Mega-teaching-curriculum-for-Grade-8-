# Week 14, Session 28 — Mini-Project: Integrated Control Challenge 🤖 (GRADED)

**Phase:** Phase 3 — Systems & Control (Capstone of Phase 3)

## Learning Objectives
- **Design** and build a complete control system: at least one **input sensor**, one **decision/feedback rule**, and one **output** (LED, buzzer, servo, or LCD).
- Apply **systems thinking**: identify the input → process → output → (feedback) stages of their device.
- Write and debug code that uses `if/else` logic, thresholds, and a library (LCD/servo/sensor).
- Collaborate to plan, build, test, and present a working "smart" device.

## Science Curriculum Link
**Feedback loops & systems thinking (+ review of all sensing).** Students reuse light, temperature, distance, or sound sensing and connect it to an output, closing a control loop. They explain how their system **senses, decides, and acts** — and where feedback occurs — mirroring natural control systems (like body temperature regulation / homeostasis).

## Materials Checklist (per pair — students choose what their design needs)
- Arduino Mega 2560 + USB cable — 1
- Breadboard + jumper wires — 1 set
- **Choose input(s):** LDR + 10 kΩ, DHT11 temperature, HC-SR04 ultrasonic, potentiometer, or pushbutton
- **Choose output(s):** LED + 220 Ω, passive buzzer, SG90 servo, 16×2 I2C LCD
- Resistors as needed (220 Ω, 10 kΩ)
- Computer with Arduino IDE (libraries: `LiquidCrystal_I2C`, `Servo`, `DHT` as needed)

---

## TEACHER GUIDE (45-minute breakdown)

> **Format note:** This is a graded, semi-open design challenge and a "dress rehearsal" for the capstone. Students pick from a short menu of system templates (below) rather than starting from a blank page, so it fits in one 45-min session. Pairs that planned ahead may extend it.

### 0–5 min — Hook / Warm-Up
**Ask:** *"Think of a machine at home that senses something and reacts automatically — a fridge, a toaster, automatic doors, a phone screen that dims. Pick one. What does it SENSE, what DECISION does it make, and what does it DO?"*
Take 3 quick examples. Map each onto the board as **Input → Decide → Output**.

### 5–15 min — Direct Instruction
**Concept: Every control system has the same skeleton.**
```
   INPUT  ──►  PROCESS/DECIDE  ──►  OUTPUT
   (sensor)     (if/else logic)     (act)
        ▲                              │
        └──────── FEEDBACK ◄───────────┘
        (the action changes what the sensor reads next)
```
- **Analogy — a thermostat / your body:** You sense you're cold (input), decide to act (process), put on a jacket / shiver (output); now you're warmer, so the sensor reads differently — that's **feedback**.
- Today's job: build **your own** small system with all three stages.
- **The Menu** (each is a known pattern from Phases 2–3):
  1. **Smart Night Light** — LDR senses dark → LED turns on (+ LCD shows "DAY/NIGHT").
  2. **Mini Thermostat** — DHT11 temp vs setpoint → LED "heater" / servo "vent" + LCD.
  3. **Parking Sensor** — HC-SR04 distance → buzzer beeps faster as it gets closer + LCD distance.
  4. **Auto Gate/Barrier** — distance or button → servo opens/closes + LED status.
- Each pair picks ONE and may add a twist.

### 15–35 min — Hands-On Build + Code
1. **Plan (2 min):** On the worksheet, write your **Input → Decide → Output** for your chosen system and the **threshold** you'll use.
2. **Power down.** Build the circuit for your chosen template (refer to the matching earlier session's diagram).
3. **Partner check**, then **power on**.
4. Adapt the starter code below to your choice (it's structured so you fill in your sensor, threshold, and output).
5. Upload and test.

### 35–42 min — Testing & Debugging
**Success looks like:** the device automatically reacts to a change in its environment (cover the LDR, warm the sensor, move your hand closer) with no human pressing anything — the system decides for itself.

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Output never triggers | Threshold set wrong | Print the sensor value to Serial; pick a threshold between "resting" and "triggered" readings |
| Output always on | Comparison backwards (`>` vs `<`) | Flip the comparison in the `if` |
| LCD blank | Wrong I2C address or SDA/SCL wiring | On Mega use **SDA 20 / SCL 21**; try address 0x27 or 0x3F |
| Servo jitters | Power/ground issue | Common GND; don't force the horn |
| Reacts erratically | Noisy readings | Average a few readings or add a small `delay` |

### 42–45 min — Reflection / Exit Ticket
**Exit ticket:** *"Draw your system as Input → Decide → Output. Circle where feedback happens (or write 'open loop' if there is none). What real-world device is yours most like?"*

---

## CIRCUIT DIAGRAM (depends on chosen system)

Students reuse the diagram from the matching session. Generic template:

```
  INPUT SENSOR ─────► ARDUINO MEGA ─────► OUTPUT DEVICE
  (LDR/DHT11/         (reads pin,         (LED+220Ω / buzzer /
   HC-SR04/pot →       runs if/else)       servo / LCD on SDA20,SCL21)
   analog or
   digital pin)

  Power: red = 5V rail, black = GND rail (all parts share common GND)
```

**Fritzing-style build description (example: Smart Night Light + LCD):**
1. **LDR** + **10 kΩ** form a voltage divider; middle node → **A0** (signal wire); LDR top → **5V** (red); resistor bottom → **GND** (black).
2. **LED long leg** → **220 Ω** → pin **9**; LED short leg → **GND**.
3. **LCD I2C:** VCC→5V, GND→GND, **SDA→pin 20**, **SCL→pin 21**.

[Google image search: **"Arduino Mega LDR LED LCD I2C automatic light control breadboard"**]

---

## ARDUINO CODE (adaptable starter)

```cpp
/*
  ===================================================================
  SESSION 28 — INTEGRATED CONTROL CHALLENGE
  THE SCIENCE — SYSTEMS THINKING:
  Every smart device follows: INPUT (sensor) -> PROCESS (decide with
  if/else) -> OUTPUT (act). When the action changes what the sensor
  reads next, that is FEEDBACK — a closed loop, like a thermostat or
  your body keeping a steady temperature (homeostasis).
  Fill in the 3 marked sections for YOUR chosen system.
  ===================================================================
*/

// ---- 1) CHOOSE YOUR PINS ----
int sensorPin = A0;     // >>> your INPUT sensor pin (A0 for LDR/pot, etc.)
int outputPin = 9;      // >>> your OUTPUT pin (LED/buzzer)

// ---- 2) CHOOSE YOUR THRESHOLD ----
int threshold = 400;    // >>> the value that triggers the action
                        //     (read your sensor first to pick a good number!)

void setup() {
  pinMode(outputPin, OUTPUT);   // The output device
  Serial.begin(9600);           // So we can SEE the sensor values
}

void loop() {
  // ---- INPUT: read the sensor ----
  int reading = analogRead(sensorPin);   // 0..1023
  Serial.print("Sensor reading: ");
  Serial.println(reading);               // Use this to choose your threshold

  // ---- PROCESS + OUTPUT: the decision (feedback loop) ----
  if (reading < threshold) {             // >>> maybe flip < to > for your system
    digitalWrite(outputPin, HIGH);       // ACT: turn the output ON
    // e.g., light comes on when it's dark
  } else {
    digitalWrite(outputPin, LOW);        // ACT: turn the output OFF
  }

  delay(200);   // Small pause so readings are steady
}

/*
  // ===== CHALLENGE =====
  // 1) Add an LCD that shows the live reading AND the current state
  //    ("LIGHT ON" / "LIGHT OFF").  (SDA=20, SCL=21 on the Mega)
  // 2) Replace the LED with a SERVO that opens a "vent/gate" past the
  //    threshold:  #include <Servo.h>;  myServo.write(angle);
  // 3) Use HC-SR04 distance as the input and a buzzer that beeps FASTER
  //    as something gets closer (a true closed-loop parking sensor).
  // 4) Two-condition logic: trigger only if it's dark AND a button is on
  //    (use && for AND, || for OR).
*/
```

---

## STUDENT WORKSHEET

### 🤖 Build a Smart Device — Integrated Control Challenge
**Objectives:** Design a system that senses, decides, and acts — all by itself.

### What I Already Know
1. Name the three stages every control system has.
2. What is a **threshold**, and why do we read the sensor before choosing one?
3. What does "feedback" mean in a control system? Give one real example.

### Plan It First (System Map)
Fill this in **before** building:
- My system is: ☐ Night Light ☐ Thermostat ☐ Parking Sensor ☐ Auto Gate ☐ Other: ______
- **INPUT** (sensor): ____________________
- **DECIDE** (the rule): "IF ____________ THEN ____________ ELSE ____________"
- **OUTPUT** (action): ____________________
- My planned **threshold**: ____________________

### Build It — Step by Step
1. **Power down.** Build your chosen circuit (use the earlier session's diagram).
2. **Partner check**, **power on**, upload the starter code.
3. Open the **Serial Monitor** and read your sensor's resting value and triggered value.
4. Set your **threshold** between them. Re-upload. Test by changing the environment.

### Predict! (before testing)
- What value do you think the sensor will show when "resting"? When "triggered"?
- What will your output do the moment you cross the threshold?

### Observe / Data Table
| Condition | Sensor reading | Output state (on/off / angle / beep rate) |
|-----------|----------------|-------------------------------------------|
| Resting (normal) | | |
| Just at threshold | | |
| Fully triggered | | |

### What Did You Notice?
1. Did your device react **without** anyone pressing a button? How?
2. How did you choose your threshold? What happened if it was too high/low?
3. Where is the **feedback** in your system — or is it "open loop"?
4. What real-world machine is your device most like?

### Science Connection
**How does this relate to feedback loops and systems thinking?** Explain your device using *input, process, output,* and *feedback*. Compare it to how your body keeps a steady temperature.

### Challenge Extension
Add a second input or an LCD status display, OR use AND/OR logic so two conditions must be met. Describe your upgrade and re-draw your system map.

---

## SAFETY NOTES
- **Build → Check → Power on.** Rewire only when unplugged.
- LCD I2C on the Mega uses **SDA = 20, SCL = 21** — don't force it onto A4/A5.
- **Servo:** keep fingers/hair away from the moving horn; don't force it by hand; share a common GND.
- **Buzzer:** keep it brief and at reasonable volume (sound-sensitive classmates).
- Keep the **220 Ω** on LEDs and **10 kΩ** in sensor dividers.
- Heat or burning smell → **unplug immediately**, tell the teacher.

---

## ASSESSMENT RUBRIC (GRADED — Phase 3 Mini-Project)

| Criterion | 1 — Beginning | 2 — Developing | 3 — Proficient | 4 — Exemplary |
|-----------|---------------|----------------|----------------|----------------|
| **Circuit Construction** | Incomplete; major help needed | Builds with hints; a wiring error | Sensor + output correctly wired with right resistors | Neat, robust, multi-part build; correct I2C/servo power |
| **Code Functionality** | Won't compile/upload | Uploads but logic/threshold wrong | Reads sensor, applies threshold, drives output reliably | Adds working extras (LCD/servo/2nd condition); clean code |
| **Science Understanding** | Can't identify the stages | Names some stages | Clearly maps Input→Decide→Output and chooses a sound threshold | Explains feedback/open-loop and links to homeostasis/real systems |
| **Collaboration** | One partner works | Uneven roles | Shared planning, build & code; roles swapped | Excellent teamwork; clear shared design decisions; helps others |
| **Reflection Quality** | Blank/one word | Surface answers | Thoughtful; identifies feedback and a real-world analog | Insightful; evaluates threshold choice and proposes improvements |

**Score:** ___ /20  (≥17 Exemplary · 13–16 Proficient · 9–12 Developing · ≤8 Beginning)

> **Capstone bridge:** Keep this system map and code — it's a great seed for your Phase 4 capstone project!

---

## DIFFERENTIATION
**Support:**
- Assign the **Smart Night Light** template (simplest); provide its circuit pre-wired and code with only the **threshold** to fill in.
- Provide a printed **system-map** graphic organizer to fill in before building.
- Sentence starters: "My sensor measures ___. When it reads ___, my device ___."

**Extension (early finishers):**
- Add an LCD dashboard and a second condition (AND/OR).
- Convert an open loop into a **closed loop** (e.g., servo vent that re-reads temperature) and explain the feedback.
- Help another pair debug — explaining solidifies systems thinking.

**Visual / Kinesthetic:**
- "Human control loop": one student is the sensor, one the "brain" (reads a rule card), one the output — act out the loop, then add feedback.
- Color-code wiring and provide large-print Input→Decide→Output cards to arrange physically.
