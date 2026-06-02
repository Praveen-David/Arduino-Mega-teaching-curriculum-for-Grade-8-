# Week 3, Session 5 — Digital Input: Pushbuttons

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- Wire a pushbutton circuit with a 10kΩ pull-down resistor and explain its purpose
- Use `digitalRead()` to detect button state in code and respond with an LED output
- Explain the difference between INPUT and OUTPUT pin modes
- Define the terms pull-up resistor, pull-down resistor, floating pin, and switch debounce

**Science Curriculum Link:** Grade 8 Electricity — Switches, Conductors, and Electrical Logic. A pushbutton is a mechanical switch: it physically connects two conductors when pressed, completing the circuit. The concept of "high" and "low" logic states (1 and 0) is the foundation of digital electronics — every computer, phone, and microcontroller communicates through these two states. Students make the direct link between a physical action (pressing a button) and a logical state that code can read.

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 + USB cable
- 1 × Solderless breadboard
- 1 × Red LED (5mm)
- 1 × 220 Ω resistor (for LED — red-red-brown)
- 1 × 10 kΩ resistor (for pull-down — brown-black-orange)
- 1 × Tactile pushbutton (4-pin, 6mm)
- 5 × Jumper wires (red, black, yellow — M-M)
- 1 × Laptop with Arduino IDE 2.x
- 1 × Printed Mega pinout diagram

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Physical demo:** Hold up a pushbutton (4-pin tactile switch). Pass it around or place one at each workstation.

"Press it. What do you feel?" *(A click, a slight resistance)*

"What is happening inside the button when you press it? Think about what we know about closed circuits."

*(Expected: two metal contacts come together, completing the circuit)*

"Now here is the interesting question: how does the Arduino KNOW if the button is being pressed? It is not alive. It cannot feel. It can only measure one thing: voltage. So somehow, a button press has to become a voltage that the Arduino can read. How would you do that?"

Take 2–3 responses. Let curiosity build — reveal the answer during direct instruction.

---

### 5–15 min — Direct Instruction

**Concept 1: Digital input and the two states**

"So far, we have only used Arduino pins as OUTPUTS — sending voltage out to control LEDs. But pins can also be set as INPUTS — measuring voltage coming IN from a component. That is what we do with buttons."

"A digital input pin reads either HIGH (approximately 5V) or LOW (approximately 0V). Your code calls `digitalRead(pin)` and gets back one of two values: `HIGH` (also written as `1`) or `LOW` (also written as `0`). That is it — just two states."

**Concept 2: The floating pin problem**

"Here is the tricky part. When a button is NOT pressed, what voltage does the input pin see?"

Draw on board:

```
  Button NOT pressed:  Pin 7 ─────────── [nothing] ──── ???
  
  The pin is connected to... nothing. It just floats.
  A "floating" pin picks up electrical noise from the air,
  the desk, nearby devices. It randomly reads HIGH or LOW.
  The LED would flicker randomly! That is useless.
```

"We need a way to give the pin a definite LOW signal when the button is not pressed, and a definite HIGH signal when it IS pressed."

**Concept 3: The pull-down resistor**

"A pull-down resistor solves the floating pin problem. We connect the pin to GND through a large resistor (10kΩ). When the button is not pressed, the pin sees 0V — a definite LOW. When the button IS pressed, it connects the pin to 5V, which overwhelms the 10kΩ to GND and the pin reads HIGH."

Draw on board:

```
  5V ──────────────── [Button] ──── Pin 7
                                      │
                                   [10kΩ]    ← pull-down resistor
                                      │
                                     GND

  Button NOT pressed:  Pin 7 sees 0V (pulled to GND through 10kΩ)  → reads LOW
  Button PRESSED:      Pin 7 sees 5V (directly from 5V)            → reads HIGH
```

**Concept 4: INPUT_PULLUP — the built-in option**

"The Arduino Mega also has built-in pull-UP resistors (about 20kΩ) inside the chip that we can enable with `INPUT_PULLUP` mode. With this mode, logic is REVERSED: button NOT pressed = HIGH, button PRESSED = LOW. We will try both today."

**Concept 5: The 4-pin button layout**

"The tactile buttons have 4 legs arranged in a rectangle. Opposite legs are connected in pairs. The button bridges the two pairs when pressed. Always bridge the CENTER GAP of the breadboard — the button straddles columns E and F so each side is electrically separate before pressing."

---

### 15–35 min — Hands-On Build + Code

**SAFETY: USB unplugged. Build → Check → Power on.**

**Build Steps:**

1. **USB UNPLUGGED.**
2. **LED circuit:** Place red LED in rows 20–21 (anode row 20/E, cathode row 20/F). Insert 220Ω resistor from row 18/A to row 20/A. Connect red wire from pin 8 → row 18/A. Connect black wire from GND → row 20/J.
3. **Button:** Straddle the button across the breadboard center gap, rows 7–9 (legs in rows 7 and 9, columns E and F). Each side is isolated until pressed.
4. **5V to button:** Connect red wire from Arduino 5V pin → row 7/A (left side of button = "input" side).
5. **Signal wire to pin 7:** Connect yellow wire from row 9/A (right side of button = "output" side) → Arduino digital pin 7.
6. **Pull-down resistor:** Connect 10kΩ resistor from row 9/B to GND row (or use a second black wire to GND). One leg at row 9/B, other leg connects to GND rail or directly to a GND jumper wire on another row.
7. **Partner check:** Verify 5V → button side 1, button side 2 → pin 7 AND → 10kΩ → GND.
8. Upload code. Test button: LED should turn ON when button is pressed, OFF when released.

**After basic build works (25–35 min):**

9. Modify code to use `INPUT_PULLUP` instead (see Challenge section). Observe inverted behavior (LED on when NOT pressed, off when pressed).
10. Try pressing the button rapidly — observe any flickering (debounce preview).

---

### 35–42 min — Testing & Debugging

**Success looks like:** LED is OFF when button is not pressed, turns ON as soon as button is pressed, turns OFF immediately when released.

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| LED is always ON regardless of button | Button wired backwards, or pin 7 always reading HIGH | Check pull-down resistor connection; confirm 10kΩ is between pin 7 and GND |
| LED is always OFF regardless of button | 5V not reaching button, or LED circuit has an error | Check 5V wire to button and LED polarity/resistor |
| LED flickers randomly without pressing button | No pull-down resistor / pin is floating | Confirm 10kΩ pull-down is connected between signal side and GND |
| LED stays ON after releasing button | Code uses `while` instead of `if` — reads button once only | Check code logic in loop() |
| Button requires very hard press to work | Button oriented wrong (connected across long axis instead of short axis of gap) | Rotate button 90° — the gap-bridging sides should face left-right |
| Code compiles but LED doesn't respond | Wrong pin number in code (pin 7 vs whatever you wired to) | Confirm `const int BUTTON_PIN = 7` matches actual wiring |

---

### 42–45 min — Reflection / Exit Ticket

**Students answer:**

1. "What is a 'floating pin'? Why is it a problem for digital inputs?"
   *(Expected: A pin not connected to a definite voltage level. It reads random HIGH/LOW due to electrical noise.)*

2. "What does `digitalRead()` return? What are the two possible values?"
   *(Expected: HIGH or LOW / 1 or 0)*

3. "If you use `INPUT_PULLUP` mode, is the button logic the same or inverted compared to external pull-down? Explain."
   *(Expected: Inverted — button NOT pressed = HIGH, button PRESSED = LOW)*

---

## 3. Circuit Diagram

```
  Arduino Mega 2560                 Breadboard
  ┌───────────────┐         ┌──────────────────────────────────────┐
  │               │         │                                      │
  │   5V      ●───┼──red───→│ Row 7/A  ──[Button left legs]──      │
  │               │         │                [BTN]                  │
  │               │         │          ──[Button right legs]──      │
  │   Pin 7   ●───┼──yel───→│ Row 9/A                              │
  │               │         │  Row 9/B ──[10kΩ]──→ (to GND rail) │
  │               │         │                                      │
  │               │         │  Row 18/A ←red wire← Pin 8          │
  │   Pin 8   ●───┼──red───→│  Row 18/A──[220Ω]──Row 20/A         │
  │               │         │  Row 20/E  [LED anode +]            │
  │               │         │  Row 20/F  [LED cathode -]          │
  │               │         │  Row 20/J ─────────────────→ GND   │
  │   GND     ●───┼──blk───→│  (power rail or direct to GND)      │
  └───────────────┘         └──────────────────────────────────────┘

  When button is PRESSED:
    5V → button → Row 9/A (Pin 7) → reads HIGH → LED turns ON

  When button is NOT pressed:
    Row 9/A (Pin 7) → 10kΩ → GND → reads LOW → LED stays OFF
```

**Fritzing-style Build Description:**

1. **Arduino Mega, 5V pin** → red jumper wire → breadboard **row 7, column A** (left side of button)
2. **Tactile pushbutton**: straddles center gap, legs at rows 7 and 9, columns E and F
3. **Arduino Mega, digital pin 7** → yellow jumper wire → breadboard **row 9, column A** (right side of button)
4. **10 kΩ resistor** (pull-down): one leg in **row 9, column B**; other leg in GND rail or **GND row**
5. **Arduino Mega, digital pin 8** → red jumper wire → breadboard **row 18, column A**
6. **220 Ω resistor**: one leg in **row 18, column A**; other leg in **row 20, column A**
7. **Red LED**: long leg (anode +) in **row 20, column E**; short leg (cathode –) in **row 20, column F**
8. **Arduino Mega, GND** → black jumper wire → breadboard **row 20, column J**

Wire colors: Red = 5V & pin 8, Yellow = pin 7 signal, Black = GND.

[Google image search: "Arduino pushbutton pull-down resistor circuit digitalRead LED control breadboard"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 5 — Digital Input: Pushbuttons
 *
 *  SCIENCE EXPLANATION:
 *  A pushbutton is a simple switch — a mechanical device that
 *  physically connects two conductors when pressed, allowing
 *  current to flow through a previously open circuit.
 *
 *  The Arduino reads the button state using digitalRead():
 *    - If pin 7 is at ~5V (button pressed) → HIGH (value = 1)
 *    - If pin 7 is at ~0V (button not pressed) → LOW (value = 0)
 *
 *  The 10kΩ pull-down resistor ensures pin 7 reads a definite
 *  LOW (0V) when the button is NOT pressed. Without it, the pin
 *  would "float" and pick up random electrical noise, causing
 *  unpredictable behavior.
 *
 *  This is the foundation of ALL digital input:
 *  every button, sensor, and signal is ultimately just a
 *  HIGH or LOW voltage that code can read and react to.
 * ============================================================
 */

// ---- PIN DEFINITIONS ----
const int BUTTON_PIN = 7;   // Pushbutton connected to digital pin 7
const int LED_PIN    = 8;   // Red LED connected to digital pin 8

// ---- VARIABLE ----
// A variable to store the button state (HIGH or LOW)
// 'int' because digitalRead() returns an integer (0 or 1)
int buttonState = LOW;   // Start assuming button is not pressed

// ============================================================
// setup() — Runs once on power-on or reset
// ============================================================
void setup() {
  // Pin 8 is an OUTPUT — it will send voltage to the LED
  pinMode(LED_PIN, OUTPUT);

  // Pin 7 is an INPUT — it will receive voltage from the button
  // We use INPUT (not INPUT_PULLUP) because we have an external pull-down resistor
  pinMode(BUTTON_PIN, INPUT);

  // Start the Serial Monitor — useful for seeing the button state
  // (This is preview for Session 9; for now it just lets you debug)
  Serial.begin(9600);
}

// ============================================================
// loop() — Runs forever
// ============================================================
void loop() {

  // READ the button — store whether it is HIGH (pressed) or LOW (not pressed)
  buttonState = digitalRead(BUTTON_PIN);

  // >>> TRY CHANGING THIS: use Serial.println to see the value
  Serial.println(buttonState);  // Print 1 (HIGH) or 0 (LOW) to Serial Monitor

  // CHECK the button state with an if/else
  if (buttonState == HIGH) {
    // Button IS pressed → turn the LED ON
    digitalWrite(LED_PIN, HIGH);

  } else {
    // Button is NOT pressed → turn the LED OFF
    digitalWrite(LED_PIN, LOW);
  }

  // A tiny delay reduces noise (basic debounce)
  // >>> TRY CHANGING THIS: remove this delay and see if the LED flickers
  delay(10);   // 10 milliseconds — small enough to feel instant to humans
}

// ===== CHALLENGE =====
// Challenge 1: INPUT_PULLUP — change pinMode(BUTTON_PIN, INPUT) to
//              pinMode(BUTTON_PIN, INPUT_PULLUP)
//              and remove the 10kΩ external resistor (no longer needed).
//              Note: with INPUT_PULLUP, logic is REVERSED:
//              button NOT pressed = HIGH, button PRESSED = LOW.
//              Update your if/else to match the inverted logic.
//
// Challenge 2: TOGGLE — instead of holding the button to keep the LED on,
//              make ONE press turn the LED ON and the NEXT press turn it OFF.
//              Hint: you need a new boolean variable, e.g. bool ledOn = false;
//              and you need to detect when the button CHANGES from LOW to HIGH
//              (called "edge detection" — look for buttonState != lastButtonState).
//
// Challenge 3: THREE LEDs — add two more LEDs on pins 9 and 10.
//              First press: LED on pin 8 lights.
//              Second press: LED on pin 9 lights (pin 8 off).
//              Third press: LED on pin 10 lights (pin 9 off).
//              Fourth press: all off. Repeat.
//              Hint: use a counter variable and a switch/case or if/else chain.
```

---

## 5. Student Worksheet

### Session 5 — Digital Input: Pushbuttons
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Wire a pushbutton with a pull-down resistor and connect it to an Arduino input pin
- Use `digitalRead()` to read button state and control an LED
- Explain what a floating pin is and why a pull-down resistor fixes it

---

#### What I Already Know — Warm-Up Questions

1. So far we have only used Arduino pins as OUTPUTS. What do you think an INPUT pin would do differently?

   _____________________________________________________________________________

2. When you press a button, what physically happens inside the button? (Think: open/closed circuit)

   _____________________________________________________________________________

3. If a pin is not connected to anything, what voltage do you think it would read? Why might this be a problem?

   _____________________________________________________________________________

---

#### Build It — Step by Step

Use the numbered steps in Section 2 of the Teacher Guide. Check off each step as you complete it.

- [ ] Step 1: USB unplugged — confirmed
- [ ] Step 2: LED circuit (pin 8, 220Ω, red LED)
- [ ] Step 3: Button placed across center gap at rows 7–9
- [ ] Step 4: 5V wire to row 7/A (left side of button)
- [ ] Step 5: Yellow signal wire from row 9/A to pin 7
- [ ] Step 6: 10kΩ pull-down resistor from row 9/B to GND
- [ ] Step 7: Partner check completed
- [ ] Step 8: Code uploaded and tested

**Quick partner check question:** Your partner asks: "What happens at pin 7 when the button is not pressed?" You answer: ____________________________________________

---

#### Predict!

Answer BEFORE powering on:

1. When the button is NOT pressed, what voltage does pin 7 read? What state does the LED have?

   _____________________________________________________________________________

2. When the button IS pressed, what voltage does pin 7 read? What state does the LED have?

   _____________________________________________________________________________

3. What do you predict will happen if you remove the 10kΩ resistor from the circuit? (The button is still wired in.)

   _____________________________________________________________________________

---

#### Observe / Data Table

| Test | Button state | Pin 7 voltage reading | LED state | Serial Monitor shows |
|------|--------------|-----------------------|-----------|---------------------|
| Button NOT pressed | Released | | | |
| Button PRESSED and held | Held down | | | |
| Button released quickly | Released | | | |
| Remove 10kΩ resistor, button not pressed | Released | | | *(if tested)* |

---

#### What Did You Notice?

1. How quickly does the LED respond to the button press? Does it feel instant? What does this tell you about how fast the `loop()` function runs?

   _____________________________________________________________________________

2. When you removed the 10kΩ resistor (if tested), what happened? Does this explain the term "floating pin"?

   _____________________________________________________________________________

3. The Serial Monitor shows numbers. When the button is pressed, it shows ____. When not pressed, it shows ____. What do these numbers represent?

   _____________________________________________________________________________

4. How is `digitalRead()` different from `digitalWrite()`? Use the words input and output in your answer.

   _____________________________________________________________________________

---

#### Science Connection

1. A pushbutton is a type of switch. What physical change inside the button causes the Arduino to read HIGH? Describe in terms of conductors and circuits.

   _____________________________________________________________________________

2. Computers process billions of on/off (HIGH/LOW) signals every second. Every key on a keyboard, every tap on a touchscreen, every sensor reading — all start as a HIGH or LOW. Why is "just two states" so powerful and reliable?

   _____________________________________________________________________________

3. The pull-down resistor (10kΩ) allows a tiny bit of current to flow from pin 7 to GND even when the button is not pressed. This current is so small that `I = 5V / 10000Ω = 0.5mA` — barely anything. Why do we use such a LARGE resistor here (compared to the 220Ω for the LED)?

   _____________________________________________________________________________

---

#### Challenge Extension

1. **Toggle:** Modify the code so one press turns the LED ON and the next press turns it OFF (like a real light switch). Describe the logic you need:

   _____________________________________________________________________________
   _____________________________________________________________________________

2. **INPUT_PULLUP:** Change the code to use `INPUT_PULLUP` mode and remove the 10kΩ resistor from the breadboard. What happens? Write the change you made to the `if` condition and explain why:

   Old condition: `if (buttonState == HIGH)`
   New condition: `if (buttonState == ___________)`

   Why: ________________________________________________________________________

---

## 6. Safety Notes

**Session 5 Hazards — Buttons and input wiring.**

**General reminders:**
- USB UNPLUGGED during all wiring changes. Never swap the button or move the signal wire while powered.
- With the 5V wire added to the breadboard, there is now a 5V source on the board at all times when USB is plugged in. Do not allow the 5V wire to touch any signal pin directly — this will send 5V into the pin, which may damage it.

**Button handling:**
- The 4-pin tactile button leads are fine metal pins — do not push them into the breadboard at an angle or they will bend. Press straight down with even pressure.
- Do not press the button mechanically with a metal tool (pen cap, screwdriver) — use a finger only.
- If a button lead breaks off: do not use, return to spare parts bin.

**Wiring caution:**
- The 5V (power) and GND wires are close together on the Arduino's power pins. Double-check before plugging in: ensure the red wire goes to 5V and the black wire goes to GND, NOT the other way around. Reversed power wires can damage the Mega.
- The 10kΩ resistor must connect the signal side of the button to GND — NOT to 5V. If the pull-down is accidentally connected to 5V, it provides no pull-down effect and the pin still floats.

**If something goes wrong:**
| Symptom | Action |
|---------|--------|
| Board gets warm immediately after plugging in | Possible 5V-to-GND short or 5V directly into a digital pin — unplug USB, check all wiring |
| Button feels "crunchy" or doesn't click | Button may be damaged or inserted at wrong angle — power down, remove and reinsert straight |
| LED flickers with no button press | Pin is floating (no pull-down) or pull-down not properly grounded — check resistor connections |

---

## 7. Assessment Rubric

### Formative Check — Session 5 (Not Graded)

| Look-For | What to Observe |
|----------|----------------|
| **Circuit construction** | Is the pull-down resistor correctly connecting the signal side of the button to GND (not 5V)? Is the 5V wire on the input side of the button? Does the circuit work — LED responds to button presses? |
| **Code understanding** | Can the student explain what `digitalRead()` returns (HIGH or LOW)? Can they explain what the `if/else` is checking? Do they understand why `INPUT` mode is used for the button pin? |
| **Conceptual understanding of floating pins** | When asked "what happens without the 10kΩ resistor?", does the student predict or describe floating behavior? Do they understand that the resistor gives the pin a default voltage level? |

---

## 8. Differentiation

### Support
- Pre-label the button's two sides with sticky notes: "5V side" and "Pin 7 side" — removes the need to understand the button's internal pin connections.
- Provide a printed circuit photo taken from directly above, with each wire color labeled and the button placement marked.
- Give a partially filled code template with `buttonState = digitalRead(BUTTON_PIN);` already written and a hint for the `if` condition.
- Sentence starters: "A pull-down resistor is needed because... / When the button is pressed, the pin reads... because..."

### Extension
- Toggle button challenge: introduces the concept of "state" in programming — the LED needs to remember whether it was last on or off. This requires a boolean variable and edge detection (comparing current state to previous state).
- Three-button sequencer: three separate buttons, each controlling a different LED — introduces the idea of multiple independent inputs.
- Debounce: for students who notice LED flickering on fast presses, explain the concept of contact bounce (the button contacts briefly make-break-make when first pressed) and have them implement a `delay(50)` debounce or research the `Bounce2` library.

### Visual / Kinesthetic Accommodations
- Physical demo: hold two wires — one from the 5V source, one from the "input" side. Your hands are the button contacts. Touch them together (press) = circuit closed = HIGH. Separate them = open = LOW. The pull-down resistor is a classmate holding the input wire to GND loosely with one hand — when you touch it with 5V, 5V "wins" because it is a direct connection. That is the pull-down in action.
- Large-print wiring diagram available.
- For students with fine motor challenges: use M-F jumper wires for the button (insert the female end directly over the button lead), reducing the need for careful breadboard row placement.
- Serial Monitor: encourage use of Serial.println to make the digital read visible as printed numbers — this makes the abstract concept concrete.
