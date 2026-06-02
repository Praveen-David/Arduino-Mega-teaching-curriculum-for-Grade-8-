# Week 4, Session 7 — PWM & Fading LEDs (Analog Output)

**Phase:** Phase 1 — Foundations

## Learning Objectives
- Explain the difference between a digital ON/OFF signal and an analog-like signal made with **PWM** (Pulse Width Modulation).
- Use `analogWrite()` on a `~`PWM pin to control LED **brightness** (not just on/off).
- Build a smooth "breathing" LED fade using a `for` loop.
- Connect the idea of **duty cycle** to **energy delivered** over time.

## Science Curriculum Link
**Energy & Electricity (duty cycle, average power).** A PWM signal switches the LED on and off hundreds of times per second. The *fraction* of time it is ON (the duty cycle) sets how much **energy** the LED receives on average, which our eyes read as brightness. This previews **analog signals** (Phase 2) and reinforces that energy can be delivered in controlled amounts.

## Materials Checklist (per pair)
- Arduino Mega 2560 + USB cable — 1
- Breadboard — 1
- LED (any color) — 1 (challenge: add 2 more)
- 220 Ω resistor — 1
- Jumper wires (red, black, 1 signal color) — 3
- Computer with Arduino IDE — 1

---

## TEACHER GUIDE (45-minute breakdown)

### 0–5 min — Hook / Warm-Up
**Demo:** Plug in a desk lamp on a dimmer switch (or a phone screen brightness slider). Slowly dim it.
**Ask:** *"This light isn't just ON or OFF — it has in-between levels. But a computer pin can only be HIGH (5V) or LOW (0V). How could we make a single digital pin produce a 'half-brightness' light?"*
Expected answers: "blink it really fast," "turn it on only part of the time." Celebrate that intuition — that's exactly PWM.

### 5–15 min — Direct Instruction
**Concept: Pulse Width Modulation (PWM).**
- The pin still only does HIGH or LOW — but it flips between them **very fast** (~490 times/sec on the Mega).
- **Duty cycle** = the percent of each cycle that is HIGH.
  - 100% HIGH = full brightness. 50% = half. 0% = off.
- **Analogy — a flickering ceiling fan / strobe:** If you flick a light switch on/off faster than your eye can follow, your brain blends it into a steady, dimmer glow. More "on time" = brighter.
- **Analogy — watering a plant:** A trickle (small duty cycle) vs a full stream (large duty cycle). Same tap, different *average* flow.
- In code: `analogWrite(pin, value)` where **value is 0–255** (0 = off, 255 = full). 128 ≈ half brightness.
- **Key rule:** Only pins marked with a **`~`** can do `analogWrite`. On the Mega these include 2–13 and 44–46.

> Write on the board: **digitalWrite = ON or OFF (2 choices). analogWrite = 0…255 (256 choices).**

### 15–35 min — Hands-On Build + Code
1. **Power down** (USB unplugged). Mantra: *Build → Check → Power on.*
2. Place the LED on the breadboard. Remember: **long leg = +** (anode), short leg = – (cathode).
3. Connect the **long leg** through the **220 Ω resistor** to Arduino pin **~9** (a PWM pin).
4. Connect the **short leg** to the **GND** rail, and run a black wire from GND rail to an Arduino **GND** pin.
5. Partner **check** the wiring. Then **power on** (plug in USB).
6. Open the Arduino IDE → type in the sketch below → Upload.

### 35–42 min — Testing & Debugging
**Success looks like:** the LED smoothly fades up (gets brighter), then fades down, over and over — "breathing."

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| LED is just ON, no fading | Wired to a non-PWM pin | Move signal wire to a `~` pin (e.g., ~9) |
| LED never lights | Backwards LED or missing resistor/GND | Power down, flip LED, check 220 Ω & black GND wire |
| Fades but "jumps" / flickers harshly | `delay` too long in loop | Reduce delay to ~10 ms |
| Code won't upload | Wrong board/port | Tools → Board: Mega 2560; select Port |

### 42–45 min — Reflection / Exit Ticket
**Exit ticket (on a sticky note):** *"At analogWrite value 64, is the LED on more or less than half the time? Will it look brighter or dimmer than value 200?"* (Answer: 64 = on ~25% of the time → dimmer than 200.)

---

## CIRCUIT DIAGRAM

```
   ARDUINO MEGA                         BREADBOARD
  ┌───────────┐
  │       ~9  ├──────[ 220Ω ]──────►|───┐   (LED: ►| = long leg/anode on left)
  │           │     (yellow wire)    LED  │
  │       GND ├───────────────────────────┘
  └───────────┘     (black wire to LED short leg → GND rail)

  Signal path:  Pin ~9  →  220Ω resistor  →  LED(+long)  →  LED(–short)  →  GND
```

**Fritzing-style build description:**
1. **Pin ~9** (PWM) → **yellow** jumper → one end of a **220 Ω** resistor (red-red-brown bands).
2. Other end of resistor → **long leg (+)** of the LED.
3. **Short leg (–)** of LED → breadboard ground rail (blue/–).
4. Breadboard **GND rail** → **black** jumper → any Arduino **GND** pin.

[Google image search: **"Arduino LED 220 ohm resistor breadboard fade PWM pin 9 wiring"**]

---

## ARDUINO CODE

```cpp
/*
  ===================================================================
  SESSION 7 — PWM & FADING LEDs
  THE SCIENCE:
  A digital pin can only be HIGH (5V) or LOW (0V). To make "in-between"
  brightness, the Arduino flips the pin on/off very fast (~490 times a
  second). The percent of time it stays ON is the DUTY CYCLE.
  More on-time = more ENERGY delivered = brighter light to our eyes.
  This is called PWM (Pulse Width Modulation). analogWrite() sets the
  duty cycle using a number from 0 (always off) to 255 (always on).
  ===================================================================
*/

int ledPin = 9;   // The LED is on pin ~9 — it MUST be a PWM (~) pin

void setup() {
  pinMode(ledPin, OUTPUT);   // Tell the Arduino this pin sends power out
}

void loop() {
  // FADE UP: brightness climbs from 0 (off) to 255 (full)
  for (int brightness = 0; brightness <= 255; brightness++) {
    analogWrite(ledPin, brightness);  // Set the duty cycle (0-255)
    delay(8);                         // >>> TRY CHANGING THIS: bigger = slower fade
  }

  // FADE DOWN: brightness drops from 255 back to 0
  for (int brightness = 255; brightness >= 0; brightness--) {
    analogWrite(ledPin, brightness);  // Lower number = dimmer
    delay(8);                         // >>> TRY CHANGING THIS too
  }
}

/*
  // ===== CHALLENGE =====
  // 1) Add a SECOND LED on pin ~10. Make it fade DOWN while the first
  //    fades UP (opposite brightness). Hint: analogWrite(led2, 255 - brightness);
  //
  // 2) "Heartbeat": instead of a smooth fade, make the LED do a quick
  //    double-pulse like a heartbeat, then pause. Use analogWrite + delay.
  //
  // 3) Connect a potentiometer later (Phase 2) so YOU control the
  //    brightness by hand. For now, predict: which analogWrite value
  //    is exactly half brightness? (Answer: about 128.)
*/
```

---

## STUDENT WORKSHEET

### 🌟 Fading LEDs — Making Light "Breathe"
**Objectives:** Control LED brightness with PWM and `analogWrite()`; connect duty cycle to energy.

### What I Already Know
1. What is the only difference between `digitalWrite(pin, HIGH)` and `digitalWrite(pin, LOW)`?
2. Why does an LED always need a resistor?
3. Name one thing in your home that can be **dim** as well as bright.

### Build It — Step by Step
1. **Power down** the Arduino (unplug USB).
2. Put the LED in the breadboard. Long leg is **+**.
3. Connect pin **~9** through a **220 Ω** resistor to the LED's long leg (yellow wire).
4. Connect the LED's short leg to GND (black wire).
5. **Partner check**, then **power on** and upload the code.

### Predict! (before uploading)
- What do you think will happen if `analogWrite` value is **0**? What about **255**?
- If we change `delay(8)` to `delay(2)`, will the breathing get faster or slower?

### Observe / Data Table
Change the `delay` and the brightness values, then record what you see.

| analogWrite value | How bright? (off / dim / medium / full) | Your guess at duty cycle % |
|-------------------|------------------------------------------|----------------------------|
| 0 | | |
| 64 | | |
| 128 | | |
| 200 | | |
| 255 | | |

### What Did You Notice?
1. Did doubling the value from 64 → 128 look "twice as bright"? Why might it not?
2. What happened to the *speed* of the breathing when you changed `delay`?
3. Which pins could you use for fading, and which couldn't? How did you know?
4. The pin is only ever HIGH or LOW — so how does "half brightness" happen?

### Science Connection
**How does this relate to energy?** Explain why a smaller duty cycle delivers **less energy** to the LED over each second, using the words *on-time* and *average*.

### Challenge Extension
Add a second LED on `~10` that fades **opposite** to the first (one bright while the other is dim). Sketch your wiring and write the one line of code that makes it opposite.

---

## SAFETY NOTES
- **Build → Check → Power on.** Never rewire while USB is plugged in.
- Always keep the **220 Ω resistor** in series with the LED — `analogWrite` still sends real current.
- LEDs have **polarity** (long leg +). Backwards = it just won't light (no damage at this current, but check it).
- If the LED or any part feels warm or you smell anything, **unplug immediately** and tell the teacher.
- Mind the trimmed/sharp resistor and LED legs.

---

## ASSESSMENT RUBRIC
*Formative check (not graded this session) — teacher look-fors:*
- [ ] Pair used a **`~`PWM pin** (understands not all pins fade).
- [ ] LED fades smoothly (correct `analogWrite` in a loop).
- [ ] Pair can explain "duty cycle = on-time = brightness/energy" in their own words.

*(Full graded rubric appears next session, the Phase 1 mini-project.)*

---

## DIFFERENTIATION
**Support:**
- Provide the circuit pre-wired or a labeled wiring photo. Give the code with only the two `delay(8)` values as `// FILL IN` blanks.
- Use a "dimmer slider" analogy and let them physically slide a phone brightness bar first.

**Extension (early finishers):**
- Complete all 3 CHALLENGE tasks; add a 3rd LED for a "traffic-wave" fade.
- Use `map()` (preview) to fade between two brightness limits, e.g., 50–200.

**Visual / Kinesthetic:**
- "Be the PWM pin": students flick a paper sign HIGH/LOW faster and slower while the class judges the "average brightness."
- Color-code wires (yellow = signal, black = GND) and provide a large-print 0–255 brightness strip.
