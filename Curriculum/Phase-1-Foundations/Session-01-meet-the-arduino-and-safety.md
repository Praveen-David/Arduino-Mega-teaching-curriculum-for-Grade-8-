# Week 1, Session 1 — Meet the Arduino & Lab Safety

**Phase:** Phase 1 — Foundations

**Learning Objectives:**
- Identify the key parts of an Arduino Mega 2560 board and describe the function of each section
- Define electricity, electric charge, current, and closed circuit in their own words
- Demonstrate correct lab safety habits, including the Build → Check → Power on sequence
- Navigate the Arduino IDE, open the Blink example sketch, and identify the structure of a sketch

**Science Curriculum Link:** Grade 8 Electricity — What is electric current? Students connect the concept of electrons flowing through a conductor to the idea of a circuit: a complete, closed loop is needed before anything can turn on. This session provides the foundational mental model that every future project will build on.

**Materials Checklist (per pair):**
- 1 × Arduino Mega 2560 board
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard (830 tie-points)
- 1 × Pack of assorted jumper wires
- 1 × Laptop / desktop with Arduino IDE 2.x installed
- 1 × Printed Arduino Mega 2560 pinout diagram (color)
- 1 × Resistor color-code reference card
- 1 × Kit inventory card (for component count check)
- Sticky label to name the kit

---

## 2. Teacher Guide

### 0–5 min — Hook / Warm-Up

**Ask the class:** "Without using your phone, how many things in this room right now are powered by electricity?" Give students 30 seconds to look around and count silently, then take hands.

*Expected answers:* Lights, projector, computers, heating/cooling fans, clock. Most students will count 10–20 items.

**Follow-up question:** "Every single one of those things needs two things — a source of energy AND a complete path for the electricity to travel. What do you think happens if that path is broken?"

*Expected answer:* The device stops working. Use a light switch as the concrete image — when you flip the switch OFF, you are physically breaking the path.

**Transition:** "Today we are going to meet the small computer that will be our tool for the whole semester — the Arduino Mega — and learn the safety rules that keep us, and our equipment, safe."

---

### 5–15 min — Direct Instruction

**Say to students:**

"Before we touch anything, let's understand what electricity actually is — because if you understand the science, the circuits will make sense."

**Concept 1: Electric Charge and Current**

"Everything is made of atoms. Atoms contain electrons, which carry a negative electric charge. In some materials — like copper wire — electrons are not tightly held by their atoms. We call these materials **conductors**. In a conductor, electrons can move from atom to atom. When a huge number of electrons flow in the same direction through a wire, we call that flow **electric current**."

**Analogy (say this):** "Imagine a garden hose full of water. The water is like the electrons. The hose is like the wire. When you turn on the tap, water (electrons) flows through the hose. That flow is current. The pressure from the tap is like **voltage** — it pushes the current along. And anything that slows the flow — a kink in the hose — is like **resistance**. We will explore Ohm's Law in depth in Session 3, but today just hold on to that picture: *voltage pushes, resistance slows, current flows*."

**Concept 2: Closed Circuits**

"For electrons to flow, they need a complete loop — a path from the power source, through whatever device you want to power, and back to the source. That complete loop is a **closed circuit**. If the loop is broken — even by the tiniest gap — current stops and the device turns off."

Draw or project this on the board:

```
  Battery (+) ──── Wire ──── LED ──── Wire ──── Battery (-)
                         ↑
                   (electrons flow through this loop)
```

"Today our power source is not a battery — it is a USB cable connected to a laptop. The Arduino board manages that power and lets us control it with code."

**Concept 3: Tour of the Arduino Mega 2560**

Hold up the board (or use document camera). Point to each section as you name it:

| Area | What it does |
|------|-------------|
| USB port (Type B) | Connects to the computer; provides power and uploads code |
| Microcontroller chip (ATmega2560) | The "brain" — a tiny computer that runs our code |
| Digital pins 0–53 | Can be turned ON (5V) or OFF (0V) by our code |
| Analog input pins A0–A15 | Read variable voltages from sensors (0–5V → 0–1023) |
| Power pins (5V, 3.3V, GND) | Provide power to components we connect |
| Reset button | Restarts the program without unplugging |
| TX/RX LEDs | Blink when data is sent/received via USB |
| Built-in LED (pin 13) | A small LED wired to pin 13 — great for testing |
| PWM pins (~) | Can fake "analog" output using fast ON/OFF pulses |

"The breadboard next to the Arduino is our building surface. It has rows of holes that are connected inside. We will explore it in more detail in Session 2. Today, just look and don't poke!"

---

### 15–35 min — Hands-On Build + Code

**This session has NO circuit build** — the goal is exploration and software setup.

**Part A — Board Scavenger Hunt (15–25 min)**

Distribute the printed Mega pinout diagram and student worksheet. Students work in pairs to find and label 10 items on the actual board using the diagram.

Scavenger hunt items:
1. The USB port (type B — square shape)
2. The ATmega2560 chip (the large black square)
3. Pin 13 digital output (find the tiny built-in LED next to it)
4. Pin GND (find all three GND pins)
5. Pin 5V (the power pin)
6. The analog pins A0 and A15
7. The RESET button
8. A PWM pin (any pin marked with ~)
9. The TX and RX indicator LEDs
10. The 5V power rail on the breadboard (the long red line)

Circulate and check: students point to each item with a pencil, not a wire.

**Part B — Open the Arduino IDE (25–35 min)**

Teacher demonstrates on the projector. Students follow along step-by-step:

1. Open the Arduino IDE 2.x on the computer.
2. Click **File → Examples → 01.Basics → Blink**.
3. A new window opens showing the Blink sketch.
4. Point out the two main sections: `setup()` and `loop()`.
5. **Say:** "Every Arduino sketch has exactly these two sections. `setup()` runs once when the board powers on — like setting up your classroom before students arrive. `loop()` runs over and over forever — like the actual school day repeating."
6. Do NOT upload yet — just read the code together as a class.
7. Go to **Tools → Board** and confirm "Arduino Mega or Mega 2560" is selected.
8. Go to **Tools → Port** and select the correct COM port (the one that appears when the Arduino is plugged in).
9. Connect the USB cable from laptop to Arduino Mega.
10. Click the **Upload** button (right arrow icon).
11. Watch the built-in LED on the board blink! Celebrate this moment.

**Debrief (say to students):** "You just uploaded your first program to a real computer. The code told pin 13 to turn on, wait 1 second, turn off, wait 1 second, repeat forever. You did not build a circuit today — the LED is already wired into the board — but next session, you will build your own."

---

### 35–42 min — Testing & Debugging

**Success looks like:** Built-in LED on the Mega blinks on for 1 second, off for 1 second, repeating.

**Common problems this session:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| "Port" is grayed out or missing | USB cable not connected, or board not powered | Unplug and replug USB; try a different cable or port |
| Upload fails: "Board not found" | Wrong board selected in Tools menu | Go to Tools → Board → Arduino Mega 2560 |
| Upload fails: "avrdude: stk500v2..." | Wrong port selected | Go to Tools → Port and select the only listed port |
| LED does not blink after upload | The upload went to the wrong board | Confirm the correct board and re-upload |
| Arduino IDE won't open | Software not fully installed | Ask teacher; try reinstalling or use Tinkercad online |
| Two ports listed | Multiple USB devices | Unplug everything except the Arduino; retry |

---

### 42–45 min — Reflection / Exit Ticket

**Ask students to answer on a sticky note or the bottom of their worksheet:**

1. "In one sentence: what is an electric current?"
2. "What is the safety rule before making any changes to your wiring?" *(Expected: Power down / unplug first — Build → Check → Power on)*
3. "Name ONE part of the Arduino Mega board and say what it does."

Collect sticky notes as students leave. Quickly sort into three piles: understands, needs support, unclear — to inform Session 2 pacing.

---

## 3. Circuit Diagram

**No circuit is built this session.** The only "circuit" is the Arduino's built-in LED on pin 13, which is already on the board.

```
  ┌─────────────────────────────────────────────────────────┐
  │                   ARDUINO MEGA 2560                      │
  │                                                          │
  │  USB Cable ──→ USB Port ──→ Powers the board             │
  │                                                          │
  │  Pin 13 ──→ [internal 1kΩ resistor] ──→ [LED] ──→ GND  │
  │               (already on the board — nothing to build)  │
  │                                                          │
  │  The Blink code simply tells Pin 13 to turn ON and OFF   │
  └─────────────────────────────────────────────────────────┘
```

**Fritzing-style build description:**
No external wiring this session. The USB-A end connects to the laptop; the USB-B (square) end connects to the Arduino Mega's USB port. This provides 5V power from the computer and creates the data connection for uploading code.

[Google image search: "Arduino Mega 2560 board labeled diagram all pins"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 1 — Meet the Arduino & Blink
 *  SCIENCE EXPLANATION:
 *  This sketch demonstrates a closed circuit. Pin 13 on the
 *  Arduino Mega is connected internally to a small LED and
 *  a resistor already on the board. When we set pin 13 HIGH,
 *  the Arduino provides 5 volts to that pin, which pushes
 *  current through the resistor and the LED, making it light up.
 *  When we set it LOW, voltage drops to 0V — the circuit is
 *  "open" (broken) and no current flows, so the LED turns off.
 *
 *  This is the built-in Blink example from the Arduino IDE.
 *  We are reading it and running it — NOT writing it yet.
 * ============================================================
 *  NOTE FOR SESSION 1:
 *  We are exploring this example sketch only.
 *  Writing original code begins in Session 2.
 * ============================================================
 */

// setup() runs ONCE when the board is powered on or reset
void setup() {

  // Tell the Arduino: "Pin 13 is an OUTPUT — it will send voltage OUT"
  // (The other option is INPUT, which we use with buttons in Session 5)
  pinMode(13, OUTPUT);

}

// loop() runs OVER AND OVER FOREVER after setup() finishes
void loop() {

  digitalWrite(13, HIGH);   // Turn pin 13 ON — sends 5V → LED lights up
  delay(1000);              // Wait 1000 milliseconds (= 1 second)

  digitalWrite(13, LOW);    // Turn pin 13 OFF — sends 0V → LED turns off
  delay(1000);              // Wait 1 second

  // Then loop() starts again from the top — endlessly
}

// ===== CHALLENGE =====
// (For Session 2 and beyond — not today)
// Challenge 1: Can you find where the 1000 is and change the blink speed?
// Challenge 2: What do you think happens if you change the second delay to 200?
// Challenge 3: Can you make the LED blink in a short-short-long pattern (like Morse code)?
```

---

## 5. Student Worksheet

### Session 1 — Meet the Arduino & Lab Safety
**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:** By the end of this session I will be able to:
- Label the key parts of an Arduino Mega 2560
- Explain what an electric current is
- Recite the lab safety rules
- Open and run the Blink sketch in the Arduino IDE

---

#### What I Already Know — Warm-Up Questions

Answer these before the lesson starts. There are no wrong answers!

1. What do you think electricity actually *is*? (What is moving when a device is powered?)

   _____________________________________________________________________________

   _____________________________________________________________________________

2. When you flip a light switch OFF, what do you think physically happens inside the switch?

   _____________________________________________________________________________

   _____________________________________________________________________________

3. Have you ever coded anything before? (Scratch, Minecraft mods, Python, etc.) Describe it.

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Build It — Scavenger Hunt: Find It on the Board!

Using your printed pinout diagram, find each item on the real Arduino Mega and write a checkmark when found. Use a pencil to point — do NOT poke the board with a wire.

| # | Find This | Found? | What do you think it does? |
|---|-----------|--------|---------------------------|
| 1 | USB port (square shape) | ☐ | |
| 2 | ATmega2560 chip (big black square) | ☐ | |
| 3 | Pin 13 label + tiny built-in LED | ☐ | |
| 4 | All three GND pins | ☐ | |
| 5 | The 5V pin | ☐ | |
| 6 | Analog pin A0 | ☐ | |
| 7 | Analog pin A15 | ☐ | |
| 8 | The RESET button | ☐ | |
| 9 | Any pin with a ~ symbol (PWM) | ☐ | |
| 10 | TX and RX indicator LEDs | ☐ | |

---

#### Predict!

Before you upload the Blink code:

1. What do you think will happen when the upload finishes?

   _____________________________________________________________________________

2. Where on the board do you predict something will change?

   _____________________________________________________________________________

3. If you changed `delay(1000)` to `delay(100)`, what do you predict would change?

   _____________________________________________________________________________

---

#### Observe / Data Table

After uploading Blink, observe carefully and fill in the table:

| What I observed | My description |
|-----------------|----------------|
| Which LED blinked? (location on board) | |
| How many times did it blink in 10 seconds? Count! | |
| What did the TX/RX LEDs do during upload? | |
| What changed on screen when upload succeeded? | |

---

#### What Did You Notice?

1. Did the LED blink as you predicted? What surprised you?

   _____________________________________________________________________________

   _____________________________________________________________________________

2. Look at the code. The number `1000` appears in `delay(1000)`. What unit is 1000 in? How do you know?

   _____________________________________________________________________________

3. What do you think would happen if you removed the second `delay(1000)` completely?

   _____________________________________________________________________________

4. The code has `setup()` and `loop()`. In your own words, describe the difference.

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Science Connection — How does this relate to Electricity?

The Blink sketch turns a pin ON and OFF. When the pin is ON, it sends 5 volts to the LED.

1. What must be present for the LED to light up? (Think: what kind of circuit is needed?)

   _____________________________________________________________________________

2. When the code says `digitalWrite(13, LOW)`, voltage drops to 0V. Is the circuit open or closed at that moment? How do you know?

   _____________________________________________________________________________

3. The Arduino gets its power from the USB cable connected to the laptop. Trace the path: where does the energy start, and where does it end up?

   _____________________________________________________________________________

   _____________________________________________________________________________

---

#### Challenge Extension

If you finish early:

1. Find the `delay(1000)` values in the Blink code. Change BOTH to `delay(100)`. What happens? Describe it.

   _____________________________________________________________________________

2. **Morse code challenge:** The letter S in Morse code is three short blinks (· · ·). Can you modify the code to blink S? Write your plan here (you will try it in Session 2).

   _____________________________________________________________________________

   _____________________________________________________________________________

---

## 6. Safety Notes

**Session 1 Hazards — Low risk, but establish habits from day one.**

**Component safety this session:**
- The Arduino board has sharp exposed header pins along its edges. Hold the board by its edges only — do not press down on the pins.
- Do not place the Arduino on a metal surface (desk with metal trim, foil, coins). Use it on the cardboard insert in the kit box.
- USB cables are fine to handle, but inspect the cable ends for fraying or exposed wire before use. Discard damaged cables.
- Do NOT connect anything to the breadboard or the Arduino pins today — observation only.

**Software safety:**
- Do not download any software other than the Arduino IDE from arduino.cc. If a website asks you to install something else, tell the teacher.

**Establishing the safety mantra:**
Say together as a class: **"Build → Check → Power on."**
Explain: "This semester, every time we wire a circuit, we will: (1) Build with the USB UNPLUGGED, (2) Check our wiring with a partner, (3) THEN plug in and power on. We never change wires on a live, powered board."

**If something goes wrong this session:**
| Problem | Action |
|---------|--------|
| Board feels warm after being plugged in for a long time | This is normal — it should be warm, not hot. If hot, unplug and tell teacher. |
| Smoke or burning smell | Unplug USB immediately. Step back. Tell teacher. Do not touch board. |
| USB cable sparks on connection | Use a different cable. Tell teacher. |

---

## 7. Assessment Rubric

### Formative Check — Session 1 (Not Graded)

This session is an introduction. No grade is recorded. Use these three look-fors as you circulate:

| Look-For | What to Observe |
|----------|----------------|
| **Safety habits** | Does the student hold the board by edges? Do they avoid poking pins with wires? Do they know the Build → Check → Power on sequence when asked? |
| **Board knowledge** | Can the student point to at least 5 named parts of the Mega (USB port, digital pins, GND, analog pins, built-in LED)? |
| **IDE navigation** | Can the student open a sketch, select the correct board and port, and upload successfully? Do they understand that `setup()` runs once and `loop()` repeats? |

**Quick stamp / sticker system:** Give a green stamp for all three, yellow for two, red for one or fewer — informs your seating/support plan for Session 2.

---

## 8. Differentiation

### Support
- Provide a printed "anatomy sheet" with the board photographed and labeled, so students can do the scavenger hunt by matching images rather than reading the pinout diagram.
- Pre-select the correct board and port on computers for pairs who may struggle with the software setup — let them focus on the physical board tour.
- Use sentence starters on the worksheet: "An electric current is... because..." / "I predict... because..."
- Pair a student who is anxious about technology with the physical/tactile Builder role first (hold and examine the board) before asking them to touch the keyboard.

### Extension
- Ask early finishers to read the full Blink sketch and annotate every single line with what they think it does, then compare with the teacher's explanation.
- Challenge them to find the `LED_BUILTIN` constant in the IDE reference (Help → Reference) and understand why it is the same as pin 13 on the Mega.
- Have them sketch a diagram of what they think a "closed circuit" looks like, label it with the terms voltage, current, and resistance, and explain it to another pair.
- Tinkercad: load the Blink simulation online and experiment with the delay values before the next session.

### Visual / Kinesthetic Accommodations
- Use a physical analogy for current: stretch a long rope around the classroom in a loop. One student pulls — that's voltage. The rope moving through everyone's hands is current. A student gripping tightly is resistance. Cut the rope = open circuit. The device goes off.
- Large-print version of the pinout card available on request.
- For ELL students: provide a bilingual glossary of terms (electricity, current, voltage, circuit, conductor, resistor).
- Students who need movement: assign the "board runner" role — they carry the pinout poster around the room to help other pairs during the scavenger hunt.
