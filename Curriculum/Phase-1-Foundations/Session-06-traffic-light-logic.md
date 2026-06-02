# Week 3, Session 6 — Variables, Loops & Logic: Traffic Light

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- Declare and use integer variables to store pin numbers and timing values
- Use `for` loops to repeat a block of code a set number of times
- Write `if` / `else if` / `else` conditional statements that respond to a button press
- Build a functional three-color traffic light with a pedestrian crossing button

**Science Curriculum Link:** Grade 8 Electricity — Sequencing and Electrical Control. Real-world control systems (traffic lights, elevator doors, automatic doors) use programmed sequences — a set of predetermined states that execute in a defined order with timing constraints. Students connect the abstraction of code to a physical system they interact with daily, reinforcing that the Arduino is a programmable controller, not just a light-flasher.

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 + USB cable
- 1 × Solderless breadboard
- 1 × Red LED (5mm)
- 1 × Yellow LED (5mm)
- 1 × Green LED (5mm)
- 3 × 220 Ω resistors (red-red-brown)
- 1 × 10 kΩ resistor (brown-black-orange) — for button pull-down
- 1 × Tactile pushbutton (4-pin)
- 8 × Jumper wires (red, yellow, green, black + white for button signal)
- 1 × Laptop with Arduino IDE 2.x

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Ask the class:** "Describe exactly what a traffic light does — step by step, with timing. How long is each light on? What is the sequence? Does anything change the sequence?"

Write student answers on the board. A correct sequence:
1. Red ON for ~30 seconds
2. Red OFF → Green ON for ~25 seconds
3. Green OFF → Yellow ON for ~5 seconds
4. Yellow OFF → Red ON → repeat
5. *If pedestrian button pressed: shorten the green phase and go to the Walk signal sooner*

"You just described a program. You described variables (the time values), a loop (it repeats), and a conditional (if someone presses the button, change the behavior). Today you are going to write exactly this program — and wire a real traffic light to go with it."

---

### 5–15 min — Direct Instruction

**Concept 1: Variables**

"A variable is a named box in the computer's memory that holds a value. Instead of writing the number `8` everywhere we mean 'the red LED pin,' we create a variable called `redPin` and store 8 in it. If we later decide to use a different pin, we only change it in one place."

Write on board:
```cpp
int redPin = 8;     // 'int' = integer (whole number). Name = redPin. Value = 8.
int redTime = 3000; // The red light on-time in milliseconds
```

"We can also CHANGE variable values while the program is running — unlike constants (which are fixed). That is useful for the pedestrian button: when pressed, we can change `greenTime` to a shorter value."

**Concept 2: For loops**

"A `for` loop repeats a block of code a specific number of times. The structure is:"

```cpp
for (int i = 0; i < 3; i++) {
  // This runs 3 times (when i = 0, 1, 2)
}
```

"In traffic light terms: `for` loops are useful for blinking the yellow light 3 times quickly before it turns solid."

**Concept 3: if / else if / else**

"A conditional statement asks a question and runs different code depending on the answer. We used `if / else` in Session 5. Today we add `else if` for a three-way choice:"

```cpp
if (condition1) {
  // runs if condition1 is true
} else if (condition2) {
  // runs only if condition1 was false AND condition2 is true
} else {
  // runs if ALL conditions above were false
}
```

"In our traffic light: `if (pedestrianRequest == true)` → shorten the green phase. Otherwise, run the normal full green phase."

**Concept 4: System design thinking**

"Before writing code for any system, experienced programmers write out the LOGIC in plain English first — called pseudocode. Let us plan the traffic light together on the board before any typing."

Walk through the pseudocode as a class:
```
SETUP: set pin modes, set all lights to OFF

LOOP:
  Turn RED on
  Wait 3 seconds
  Check: was pedestrian button pressed? (set a flag = true)
  Turn RED off, turn GREEN on
  IF pedestrian button was pressed:
    Wait 1 second (short green for pedestrians)
    Reset the flag to false
  ELSE:
    Wait 3 seconds (full green)
  Turn GREEN off
  Blink YELLOW 3 times (each blink: 300ms on, 300ms off)
  Repeat forever
```

---

### 15–35 min — Hands-On Build + Code

**SAFETY: USB unplugged. Build → Check → Power on.**

**Build Steps:**

This builds on Session 5's button circuit and Session 4's multi-LED circuit.

1. **USB UNPLUGGED.**
2. **Red LED:** rows 3–5. Resistor row 3/A to 5/A. LED anode at 5/E, cathode at 5/F. Red jumper pin 8 → row 3/A. Black jumper row 5/J → GND.
3. **Yellow LED:** rows 10–12. Resistor row 10/A to 12/A. LED anode at 12/E, cathode at 12/F. Yellow jumper pin 9 → row 10/A. Black jumper row 12/J → GND.
4. **Green LED:** rows 17–19. Resistor row 17/A to 19/A. LED anode at 19/E, cathode at 19/F. Green jumper pin 10 → row 17/A. Black jumper row 19/J → GND.
5. **Pedestrian button:** place button straddling center gap, rows 25–27. Red wire from 5V → row 25/A. White wire from row 27/A → digital pin 7. 10kΩ pull-down: row 27/B → GND.
6. **Partner check:** All 5 LED color wires correct? All 4 GND wires connected? Button wired with 5V on input side, pin 7 on output side, 10kΩ to GND?
7. Upload code. Observe normal traffic light sequence.
8. Press button during green phase — confirm green shortens and goes to pedestrian mode.

---

### 35–42 min — Testing & Debugging

**Success looks like:** Red lights for 3s, green lights for 3s (or 1s if button pressed), yellow blinks 3× then red again. Pressing button during any phase shortens the next green phase.

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| One LED never turns on | That LED wired backwards or wrong pin | Power down, check polarity and which pin the wire goes to |
| Yellow light does not blink (just stays on or off) | for-loop not working, or wrong pin for yellow | Check `yellowPin` is defined correctly; check loop syntax |
| Button press does not change timing | `pedestrianRequest` not being read during correct phase | Check that `digitalRead` is inside the red-light phase |
| All three LEDs turn on at once | Code sets multiple pins HIGH without turning others OFF | Check the order of HIGH/LOW calls; ensure previous light is turned off before next is turned on |
| Traffic light runs too fast to see | Delay values are too small | Check that `redTime`, `greenTime`, `yellowTime` values are in milliseconds (3000 = 3 sec) |

---

### 42–45 min — Reflection / Exit Ticket

**Students answer:**

1. "What is a variable? Give one example of how you used a variable in the traffic light code."
   *(Expected: a named storage location; e.g., `redTime = 3000` stores the red light duration)*

2. "What does a `for` loop do? In the traffic light, what did we use it for?"
   *(Expected: repeats code a fixed number of times; used to blink yellow LED 3 times)*

3. "Describe ONE real-world system (not a traffic light) where you could use an `if/else if/else` conditional. What would the condition be checking?"

---

## 3. Circuit Diagram

```
  Arduino Mega 2560               Breadboard
  ┌───────────────┐     ┌──────────────────────────────────────────┐
  │               │     │                                          │
  │  Pin 8    ●───┼─red─→ Row 3/A──[220Ω]──Row 5/A                │
  │               │     │           Row 5/E [RED LED +]            │
  │               │     │           Row 5/F [RED LED -]──→ Row 5/J │
  │               │     │                                          │
  │  Pin 9    ●───┼─yel─→ Row 10/A─[220Ω]──Row 12/A               │
  │               │     │           Row 12/E [YEL LED +]           │
  │               │     │           Row 12/F [YEL LED -]─→Row 12/J │
  │               │     │                                          │
  │  Pin 10   ●───┼─grn─→ Row 17/A─[220Ω]──Row 19/A               │
  │               │     │           Row 19/E [GRN LED +]           │
  │               │     │           Row 19/F [GRN LED -]─→Row 19/J │
  │               │     │                                          │
  │  5V       ●───┼─red─→ Row 25/A──[BTN left]──[BTN right]        │
  │  Pin 7    ●───┼─wht─→ Row 27/A──[10kΩ]──────GND rail          │
  │               │     │                                          │
  │  GND      ●───┼─blk─→ GND rail (connects all GND rows)         │
  └───────────────┘     └──────────────────────────────────────────┘
```

**Fritzing-style Build Description:**

1. **Arduino Mega, Pin 8** → red wire → **row 3, col A** → 220Ω → **row 5, col A** → red LED (anode **row 5/E**, cathode **row 5/F**) → **row 5/J** → GND
2. **Arduino Mega, Pin 9** → yellow wire → **row 10, col A** → 220Ω → **row 12, col A** → yellow LED (anode **row 12/E**, cathode **row 12/F**) → **row 12/J** → GND
3. **Arduino Mega, Pin 10** → green wire → **row 17, col A** → 220Ω → **row 19, col A** → green LED (anode **row 19/E**, cathode **row 19/F**) → **row 19/J** → GND
4. **Arduino Mega, 5V** → red wire → **row 25, col A** (button input side)
5. **Tactile button**: legs in rows 25 and 27, straddling center gap
6. **Arduino Mega, digital pin 7** → white wire → **row 27, col A** (button output side)
7. **10 kΩ resistor** (pull-down): **row 27, col B** → GND rail
8. All GND wires share the breadboard GND power rail (black wire from Arduino GND to rail)

Wire colors: Red = pin 8 + 5V, Yellow = pin 9, Green = pin 10, White = pin 7 (button signal), Black = GND.

[Google image search: "Arduino traffic light circuit red yellow green LED breadboard pushbutton tutorial"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 6 — Variables, Loops & Logic: Traffic Light
 *
 *  SCIENCE EXPLANATION:
 *  A traffic light is an electrical control system:
 *  it cycles through defined states (RED → GREEN → YELLOW)
 *  in a fixed sequence with measured timing.
 *
 *  The pedestrian button is an INPUT that changes the system's
 *  behavior — this is called a "feedback" or "control input."
 *  Real traffic systems use sensors (pressure plates, cameras,
 *  infrared detectors) to adjust timing dynamically.
 *
 *  KEY PROGRAMMING CONCEPTS this session:
 *  - Variables: store and change values during runtime
 *  - for loops: repeat an action a specific number of times
 *  - if/else if/else: branch code based on a condition
 *  - boolean: a variable that is either true or false
 * ============================================================
 */

// ---- PIN DEFINITIONS (using const int for fixed values) ----
const int RED_PIN    = 8;    // Red traffic light LED
const int YELLOW_PIN = 9;    // Yellow traffic light LED
const int GREEN_PIN  = 10;   // Green traffic light LED
const int BUTTON_PIN = 7;    // Pedestrian pushbutton

// ---- TIMING VARIABLES (using int — these could be changed at runtime) ----
// >>> TRY CHANGING THIS: adjust timing to match your local traffic light
int redTime    = 3000;   // Red phase duration in milliseconds
int greenTime  = 3000;   // Normal green phase duration in milliseconds
int shortGreen = 1000;   // Short green when pedestrian has requested crossing
int yellowBlinks = 3;    // How many times yellow blinks before going back to red

// ---- STATE VARIABLE ----
// A boolean is a variable that can ONLY be true or false
bool pedestrianRequest = false;   // Has the button been pressed? Starts as false.

// ============================================================
// setup() — Runs once on power-on or reset
// ============================================================
void setup() {
  // Set all LED pins as outputs
  pinMode(RED_PIN,    OUTPUT);
  pinMode(YELLOW_PIN, OUTPUT);
  pinMode(GREEN_PIN,  OUTPUT);

  // Set the button pin as an input with external pull-down resistor
  pinMode(BUTTON_PIN, INPUT);

  // Start Serial Monitor for debugging
  Serial.begin(9600);

  // Make sure all LEDs start OFF
  digitalWrite(RED_PIN,    LOW);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN,  LOW);
}

// ============================================================
// A helper function — turns all lights OFF
// (Keeps the loop() code clean and readable)
// ============================================================
void allOff() {
  digitalWrite(RED_PIN,    LOW);
  digitalWrite(YELLOW_PIN, LOW);
  digitalWrite(GREEN_PIN,  LOW);
}

// ============================================================
// loop() — The traffic light sequence, runs forever
// ============================================================
void loop() {

  // ─── PHASE 1: RED ────────────────────────────────────────
  allOff();
  digitalWrite(RED_PIN, HIGH);    // Turn RED on
  Serial.println("RED");          // Print state for debugging

  // Check for pedestrian button press DURING the red phase
  // We check multiple times during the red phase using a for loop
  // (This is a simple way to "poll" for button presses during a delay)
  for (int i = 0; i < 30; i++) {       // Loop 30 times × 100ms = 3 seconds total
    if (digitalRead(BUTTON_PIN) == HIGH) {
      pedestrianRequest = true;         // Record that button was pressed
      Serial.println("Pedestrian request received!");
    }
    delay(100);                         // Wait 100ms per loop iteration
  }

  // ─── PHASE 2: GREEN ──────────────────────────────────────
  allOff();
  digitalWrite(GREEN_PIN, HIGH);  // Turn GREEN on
  Serial.println("GREEN");

  // Check if pedestrian requested short green
  if (pedestrianRequest == true) {
    Serial.println("Short green for pedestrians");
    delay(shortGreen);            // Short wait for pedestrians to cross
    pedestrianRequest = false;    // Reset the flag — request has been served

  } else {
    // No pedestrian request — normal green duration
    delay(greenTime);
  }

  // ─── PHASE 3: YELLOW — blink before returning to red ─────
  allOff();
  Serial.println("YELLOW");

  // Use a for loop to blink the yellow LED a specific number of times
  // >>> TRY CHANGING THIS: change yellowBlinks to 5 for more blinks
  for (int j = 0; j < yellowBlinks; j++) {
    digitalWrite(YELLOW_PIN, HIGH);  // Yellow ON
    delay(300);
    digitalWrite(YELLOW_PIN, LOW);   // Yellow OFF
    delay(300);
  }

  // Back to top of loop() — RED phase starts again

}

// ===== CHALLENGE =====
// Challenge 1: Add a WALK LED (4th LED on pin 11).
//              When pedestrian button is pressed:
//              → After green goes short, turn WALK LED on for 2 seconds
//              → Blink it 5 times as a warning, then turn off before yellow phase.
//
// Challenge 2: Add a SECOND button on pin 6 (from the other direction).
//              The light should stay green for the direction that has more requests.
//              Hint: count requests from each button during the red phase.
//
// Challenge 3: VARIABLE TIMING — read the pedestrian button during BOTH the
//              red AND green phases. If pressed during green, start the yellow
//              phase early (end green early). This is how many real lights work.
//
// Challenge 4: FUNCTIONS — move each light phase (red, green, yellow) into
//              its own function, e.g. void redPhase() { ... }
//              Then call them from loop(). Does the code look cleaner?
```

---

## 5. Student Worksheet

### Session 6 — Variables, Loops & Logic: Traffic Light
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Use variables to store timing values in my code
- Write a `for` loop to repeat an action a set number of times
- Use `if/else if/else` to make code respond to a button press
- Build and program a working 3-color traffic light with a pedestrian button

---

#### What I Already Know — Warm-Up Questions

1. Describe the exact sequence of a traffic light you see every day. How long is each light on?

   _____________________________________________________________________________

2. From Session 5: how does the Arduino know if a button is pressed? What function is used?

   _____________________________________________________________________________

3. What is the difference between a constant (`const int`) and a variable (`int`) in code?

   _____________________________________________________________________________

---

#### Pseudocode — Plan Before You Code

Complete this pseudocode BEFORE looking at the real code:

```
SETUP:
  Set pins 8, 9, 10 as ___________
  Set pin 7 as ___________

LOOP:
  Turn __________ LED on
  Wait __________ seconds
  Check if button was pressed → set pedestrianRequest = ___________
  Turn RED off, turn __________ on
  IF pedestrianRequest is true:
    Wait only __________ second(s)  ← short green
    Reset pedestrianRequest to ___________
  ELSE:
    Wait __________ second(s)  ← normal green
  Turn green off
  Blink __________ LED __________ times
  Repeat
```

---

#### Build It — Checklist

Check off each component as you install it:

**Traffic lights:**
- [ ] Red LED + 220Ω resistor on pin 8
- [ ] Yellow LED + 220Ω resistor on pin 9
- [ ] Green LED + 220Ω resistor on pin 10
- [ ] All three GND wires connected

**Pedestrian button:**
- [ ] Button placed across center gap (rows 25–27)
- [ ] 5V wire to button input side (row 25/A)
- [ ] White wire from button output side to pin 7 (row 27/A)
- [ ] 10kΩ pull-down resistor (row 27/B → GND)

**Partner check:** ☐ All connections verified by partner.

---

#### Predict!

1. In the code, `redTime = 3000`. What unit is 3000 in? How long is the red light on?

   _____________________________________________________________________________

2. The `for (int j = 0; j < yellowBlinks; j++)` loop blinks yellow. If `yellowBlinks = 3`, how many times does yellow turn on and off?

   _____________________________________________________________________________

3. What do you predict will happen if you press the button DURING the yellow phase?
   *(Hint: look at when in the code we check `digitalRead(BUTTON_PIN)`)*

   _____________________________________________________________________________

---

#### Observe / Data Table

| Traffic light phase | Duration (count seconds) | What LED(s) are on? | Notes |
|--------------------|--------------------------|---------------------|-------|
| Red (no button press) | | | |
| Green (no button press) | | | |
| Yellow blink | | | How many blinks? |
| Red → Green (button pressed during red) | | | Is green shorter? |

---

#### What Did You Notice?

1. When the button is pressed during the red phase, what changes in the green phase? Does it work as expected?

   _____________________________________________________________________________

2. The `for` loop counts from `j = 0` to `j < yellowBlinks`. Why does it start at 0 and not 1? How many times does it run if `yellowBlinks = 3`?

   _____________________________________________________________________________

3. What happens if you press the button MULTIPLE times during one red phase? Does the request still get served? Why or why not?

   _____________________________________________________________________________

4. In the `allOff()` function, all three LEDs are turned off. Why is it important to turn the previous light OFF before turning the next one ON?

   _____________________________________________________________________________

---

#### Science Connection

1. Identify one place in the real world where an electronic control system changes its behavior based on an INPUT (like the pedestrian button changes our traffic light timing).

   _____________________________________________________________________________

2. The traffic light runs in a fixed SEQUENCE. What would happen to traffic safety if the sequence were random? What does this tell you about the importance of sequencing in control systems?

   _____________________________________________________________________________

3. Variables in code are like measurements in a science experiment — you can change them to test different outcomes. If you changed `redTime` from 3000 to 500, what would be the scientific question you are testing?

   _____________________________________________________________________________

---

#### Challenge Extension

1. Add a WALK light (4th LED on pin 11): when the button is pressed, turn on a "walk" LED for the duration of the short green phase. Sketch the wiring you would add:

   _(space for sketch)_

2. **Real-world connection:** Research one real technology that uses a similar sequence-with-input control system (elevator, washing machine, dishwasher, automated door). Write 2–3 sentences explaining how the input changes the sequence.

   _____________________________________________________________________________
   _____________________________________________________________________________

---

## 6. Safety Notes

**Session 6 Hazards — More components, more connections.**

**Multiple LEDs:**
- With three LED circuits, there are now 6 GND connections needed. Before powering on, count all GND wires and confirm each traces back to a GND pin. A single unconnected GND wire means one LED will not work, but this is not a safety hazard — just a debugging issue.
- Never share one 220Ω resistor between two LEDs. Each LED must have its own resistor.

**Wiring organization:**
- With 8+ wires on the board, wiring can get messy. Encourage students to route wires along the edges of the breadboard so they can trace each connection clearly. A mess of wires is a troubleshooting nightmare.
- Color-matching wires to LED colors (red wire for red LED, yellow for yellow, green for green) is not required by the circuit but makes it much easier to debug.

**Button + LEDs on same board:**
- The button uses 5V (from the 5V power pin) as its HIGH signal. This is DIFFERENT from how we normally power LEDs (which use a digital pin). Remind students: the 5V wire goes ONLY to the button input side — not to any LED resistor row.

**If something goes wrong:**
| Symptom | Action |
|---------|--------|
| Wrong LED lights in sequence (red lights when green should) | Wire is on wrong pin — power down, trace each wire to its correct pin |
| Short circuit: board suddenly warm or resets | Likely a GND wire on a 5V pin or 5V on a signal pin — unplug USB immediately, inspect all power connections |
| Button seems to trigger the wrong phase | Check timing of the button poll in the code (it should be during the red phase) |

---

## 7. Assessment Rubric

### Formative Check — Session 6 (Not Graded)

| Look-For | What to Observe |
|----------|----------------|
| **Circuit construction** | Are all three LED branches independently wired with correct resistors and correct pins? Does the button have a pull-down and is it wired to 5V input → pin 7 output? Does the complete system work — all three lights sequence correctly and button changes green timing? |
| **Code structure** | Does the student understand what a variable is and can they change a timing variable and predict the result? Can they explain what the `for` loop is counting and when it stops? Do they understand the `if/else` decision structure? |
| **System thinking** | Can the student describe the traffic light as a "sequence with inputs" — i.e., articulate that the code defines a series of states and that an input can modify one of those states? This previews the "systems thinking" theme of Phase 3. |

---

## 8. Differentiation

### Support
- Provide the pseudocode table pre-filled (from the worksheet) so students can match it directly to the code during the typing step.
- Provide a partially completed code template: all variable definitions done, `setup()` done, comments in `loop()` indicating where each section goes — students fill in the `digitalWrite()` and `delay()` calls.
- Reduce complexity: omit the pedestrian button initially and get the basic red→green→yellow cycle working first before adding the button logic.
- Sentence starters: "A variable is useful because... / The for loop runs ___ times because... / When the button is pressed, the code..."

### Extension
- Two-direction traffic light: add a second set of red/green/yellow LEDs on pins 11/12/13. Program them to be opposite each other (when one direction is green, the other is red) with a brief all-red "clearance" phase between transitions.
- Pedestrian countdown: use `Serial.println()` to print a countdown (3... 2... 1...) during the short green phase.
- Functions challenge: have students refactor the code by moving each phase into its own named function (`redPhase()`, `greenPhase()`, `yellowPhase()`). Discuss: how does this make the code more readable and reusable?
- Research: look up how an actual traffic light controller works (fixed timing vs. adaptive timing with vehicle detection). Write a brief comparison.

### Visual / Kinesthetic Accommodations
- Act it out: assign three students as "red," "yellow," and "green" light operators. A fourth student is the "button." Students sit or stand as their light is on. The teacher runs through the sequence calling commands. When the button student waves, the green phase shortens. This makes the logic physical before coding.
- Large-print timing table: provided on request.
- For students who find typing long code challenging: provide the complete code as a printout to copy, so they focus on understanding rather than typing speed.
- Post the pseudocode on the board throughout the coding portion so students can always match their code to the logic.
