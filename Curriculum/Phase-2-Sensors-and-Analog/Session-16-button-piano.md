# Week 8, Session 16 — Make Music: A Button Piano

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 16 of 36
**Week:** 8

---

## Learning Objectives

By the end of this session, students will be able to:
- Declare and use an array in Arduino C++ to store a list of related values.
- Wire multiple pushbuttons with pull-down resistors to separate digital input pins.
- Map each button to a specific musical note frequency and play it using `tone()`.
- Connect the physics of frequency and pitch to the musical concept of notes and scales.

---

## Science Curriculum Link

**Grade 8 Concept: Frequency, Pitch & Musical Notes as Frequencies**

From Session 15, students know that sound frequency determines pitch. Musical notes are nothing more than standardized frequencies: Middle C (C4) = 262 Hz, A4 = 440 Hz, etc. The **equal-tempered scale** divides each octave into 12 equal frequency ratios (each note is 2^(1/12) ≈ 1.0595× the previous). This session applies the physics of sound waves directly to music — students discover that musical harmony and melody are, at their root, mathematical frequency relationships.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × Passive buzzer
- 1 × 100 Ω resistor (buzzer current limit)
- 4 × Pushbuttons (tactile, 4-pin)
- 4 × 10 kΩ resistors (brown-black-orange-gold — pull-down resistors)
- Jumper wires: red × 2, black × 5, white/gray × 4 (button signals), orange × 1 (buzzer)
- Computer with Arduino IDE installed

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

Play a recognizable tune on the buzzer from Session 15 code (or upload a "Happy Birthday" or "Twinkle Twinkle" sketch if prepared).

*"That tune was made entirely from numbers. Each number is a frequency in hertz — a specific vibration speed. Middle C is 262 vibrations per second. One octave up is 524 vibrations per second — exactly double."*

*"Today you're going to build a piano from scratch. Four buttons, four notes. You'll be able to play a simple melody by pressing buttons. Let's think about this: what information does your code need for each button press? It needs to know: (1) IS the button pressed? And if so, (2) WHICH note do I play?"*

Quick review: *"Who remembers how we read a button in Phase 1? What function do we use, and what value does it return when pressed?"* (Answer: `digitalRead()`, returns HIGH when pressed.)

---

### 5–15 min — Direct Instruction

**Arrays:**

> "In Phase 1 we stored one value in a variable. But now we need to store 4 frequencies — one for each button. We could write 4 variables (`freq1`, `freq2`, `freq3`, `freq4`), but there's a better way: an **array**. An array is a named list of values, all the same type, indexed by position (starting at 0)."

Write on the board:
```cpp
int notes[] = {262, 294, 330, 349};  // C4, D4, E4, F4
//               [0]  [1]  [2]  [3]  ← index numbers
notes[0]  // → 262
notes[2]  // → 330
```

> "Similarly, we store the button pin numbers in an array: `int buttonPins[] = {3, 4, 5, 6};`. Then we can use a `for` loop to check ALL buttons at once — instead of writing 4 separate `if` statements."

**Pull-down resistors (review from Phase 1):**

> "Each button needs a 10 kΩ pull-down resistor connected between the button's output pin and GND. Without it, the pin floats and gives random readings. The pull-down holds the pin firmly at 0V (LOW) when the button is not pressed. When the button IS pressed, 5V overpowers the resistor and the pin reads HIGH."

Draw on the board:
```
5V ──── [Button] ──── Digital Pin  ──── [10 kΩ pull-down] ──── GND
```

**Musical notes:**

Write the notes and frequencies on the board:
```
C4 = 262 Hz     D4 = 294 Hz     E4 = 330 Hz     F4 = 349 Hz
G4 = 392 Hz     A4 = 440 Hz     B4 = 494 Hz     C5 = 523 Hz
```

> "A4 (440 Hz) is the international tuning standard — every piano, violin, and digital instrument agrees that A4 is 440 Hz. Each octave up doubles the frequency."

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB before wiring."*

1. Unplug the Arduino.
2. Keep the passive buzzer + 100 Ω resistor wired to **pin 8** from Session 15 (or rewire it now).
3. Place **4 pushbuttons** in the breadboard, spanning the center gap — one button per pair of rows, spaced apart for easy pressing.
4. For **each button** (repeat steps 5–7 four times):
5. Connect one leg of the button (on the left side of the gap) to the **5V rail** (red wire).
6. Connect the opposite leg (right side of gap) to an Arduino digital pin: button 1 → pin 3, button 2 → pin 4, button 3 → pin 5, button 4 → pin 6.
7. Connect a **10 kΩ resistor** from the same pin-side leg of that button to the **GND rail**.
8. Double-check all 4 buttons have 5V on one leg, signal + pull-down to its Arduino pin, and the resistor to GND.
9. Partner verification check — this is a more complex circuit. Use the checklist on the worksheet.
10. Plug in USB. Enter and upload the code from Section 4.
11. Open Serial Monitor (9600 baud). Press each button — the Serial Monitor should show which note is playing.
12. Try pressing two buttons simultaneously — what happens?

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Each of the 4 buttons plays a distinctly different pitch.
- Holding a button sustains the note. Releasing silences it.
- Serial Monitor shows the note name and frequency when a button is pressed.
- No note plays when no button is pressed.

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| Button always HIGH (constant tone) | Pull-down resistor missing or not connected to GND. Check 10 kΩ to GND. |
| Button press has no effect | Check button orientation — 4-pin buttons have two sides of the center gap. Confirm 5V goes to one side, signal+pull-down to the other. |
| All buttons play the same note | Check that each button wire goes to its correct pin (3, 4, 5, 6), not all to the same pin. |
| Buzzer makes no sound | Check it's a passive buzzer; check pin 8 to buzzer through 100 Ω, buzzer – to GND. |
| Bouncing — one press plays note multiple times | This is button debounce. It's normal without debounce code. The simple version in our code still sounds like a single held note. |
| Two buttons pressed = silence | This is correct behavior in the simple code (last button wins). Use the advanced polyphony note in the Challenge. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"What is an array? Write an array that stores the numbers 10, 20, 30. What is the value of `myArray[1]`?"*
2. *"Why do we put a 10 kΩ resistor between the button's output and GND? What goes wrong without it?"*
3. *"A4 = 440 Hz. What frequency is A5 (one octave higher)? What about A3 (one octave lower)?"*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌──────────────────────────────────┐
   │                                  │
   │  5V ─────────────────────────────┼──── Red rail (+)
   │  GND ────────────────────────────┼──── Black rail (–)
   │                                  │
   │  Pin 3 ──────────────────────────┼──── Button 1 right leg ──── [10 kΩ] ──── GND
   │  Pin 4 ──────────────────────────┼──── Button 2 right leg ──── [10 kΩ] ──── GND
   │  Pin 5 ──────────────────────────┼──── Button 3 right leg ──── [10 kΩ] ──── GND
   │  Pin 6 ──────────────────────────┼──── Button 4 right leg ──── [10 kΩ] ──── GND
   │                                  │
   │  Pin 8 ──────────────────────────┼──── [100 Ω] ──── Buzzer (+) ──── Buzzer (–) ──── GND
   └──────────────────────────────────┘
   
   Each button (repeat x4):
   
   5V rail ──── Button left leg ──+── Button right leg ──── Pin 3/4/5/6
                                  |
                                 (button open: Pin reads LOW via 10 kΩ to GND)
                                 (button closed: Pin reads HIGH — 5V overpowers pull-down)
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | Breadboard **+ rail** | Button 1 **left leg** (row 5) | Red | 5V to button |
| 4 | Button 1 **right leg** (row 7) | Arduino **Pin 3** | White | Signal |
| 5 | Button 1 **right leg** (row 7) | 10 kΩ **leg 1** | — | Pull-down R |
| 6 | 10 kΩ **leg 2** | Breadboard **– rail** | Black | To GND |
| 7–10 | Repeat steps 3–6 for Button 2 (Pin 4, rows 10–12) | | | |
| 11–14 | Repeat steps 3–6 for Button 3 (Pin 5, rows 15–17) | | | |
| 15–18 | Repeat steps 3–6 for Button 4 (Pin 6, rows 20–22) | | | |
| 19 | Arduino **Pin 8** | 100 Ω resistor (row 28) | Orange | Buzzer signal |
| 20 | 100 Ω resistor leg 2 | Buzzer **+ pin** (row 30) | — | |
| 21 | Buzzer **– pin** | Breadboard **– rail** | Black | |

**Component values:** 4 × 10 kΩ pull-down resistors (brown-black-orange-gold), 100 Ω buzzer resistor (brown-black-brown-gold), passive piezo buzzer, 4 × tactile pushbuttons.

[Google image search: "Arduino 4 button piano buzzer circuit breadboard tone"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 16: Make Music — A Button Piano
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * Musical notes are FREQUENCIES. The equal-tempered chromatic scale
 * divides each octave into 12 equal frequency steps. Each step
 * multiplies frequency by the 12th root of 2 (≈ 1.0595).
 *
 * Example: A4 = 440 Hz
 *          A5 = 440 × 2   = 880 Hz  (one octave up = double frequency)
 *          A3 = 440 / 2   = 220 Hz  (one octave down = half frequency)
 *          B4 = 440 × 1.0595 ≈ 466 Hz (one semitone up)
 *
 * This means: frequency IS pitch. When you press a piano key, you
 * are selecting a specific frequency. Middle C is 262 Hz — the
 * piano string (or our buzzer) vibrates 262 times per second.
 *
 * ARRAYS allow us to store multiple related values and access them
 * by index number, making our code clean and scalable.
 * ------------------------------------------------------------------
 *
 * Circuit:  4 buttons on pins 3–6 with 10 kΩ pull-downs
 *           Passive buzzer + 100 Ω on pin 8
 * ============================================================
 */

// ---- Pin Definitions ----
const int BUZZER_PIN = 8;                        // Passive buzzer
const int NUM_BUTTONS = 4;                       // Number of piano keys

// ---- Button Pins (stored in an array) ----
// Array index: [0]=Button1, [1]=Button2, [2]=Button3, [3]=Button4
int buttonPins[] = {3, 4, 5, 6};    // Pins connected to each button

// ---- Note Frequencies (stored in a parallel array) ----
// Each index matches the corresponding buttonPins index above
// >>> TRY CHANGING THIS: Change any frequency to try different notes
int noteFreqs[] = {
  262,   // Button 1 = C4 (Middle C)
  294,   // Button 2 = D4
  330,   // Button 3 = E4
  349    // Button 4 = F4
};

// ---- Note Names (for Serial Monitor display) ----
// These match the same indices
String noteNames[] = {"C4 (262 Hz)", "D4 (294 Hz)", "E4 (330 Hz)", "F4 (349 Hz)"};

void setup() {
  Serial.begin(9600);
  pinMode(BUZZER_PIN, OUTPUT);

  // --- Set all button pins as INPUT (with external pull-down resistors) ---
  for (int i = 0; i < NUM_BUTTONS; i++) {
    pinMode(buttonPins[i], INPUT);   // External 10 kΩ pull-down holds pin LOW
  }

  Serial.println("=== Session 16: Button Piano ===");
  Serial.println("Press buttons 1–4 to play notes!");
  Serial.println("Button 1=C4, 2=D4, 3=E4, 4=F4");
  Serial.println("-------------------------------");
}

void loop() {
  bool anyButtonPressed = false;   // Track if ANY button is pressed

  // --- Check all buttons using a for loop ---
  for (int i = 0; i < NUM_BUTTONS; i++) {
    if (digitalRead(buttonPins[i]) == HIGH) {   // Button i is pressed!
      tone(BUZZER_PIN, noteFreqs[i]);            // Play the note for button i
      anyButtonPressed = true;

      Serial.print("Playing: ");
      Serial.println(noteNames[i]);   // Print note name to Serial Monitor

      break;   // Play only the first pressed button (lowest index wins)
               // Remove "break" to allow the last pressed button to win
    }
  }

  // --- If no button is pressed, silence the buzzer ---
  if (!anyButtonPressed) {
    noTone(BUZZER_PIN);   // Turn off the buzzer
  }

  delay(10);   // Small delay to debounce and reduce serial spam
}

// ===== CHALLENGE =====
// 1. EXPAND TO 8 NOTES (full octave):
//    Add 4 more buttons on pins 7, 10, 11, 12.
//    Set NUM_BUTTONS = 8.
//    Add G4(392), A4(440), B4(494), C5(523) to the arrays.
//
// 2. MELODY PLAYER: When a special 5th button is pressed, play
//    "Twinkle Twinkle Little Star" automatically:
//    int melody[] = {C4,C4,G4,G4,A4,A4,G4, F4,F4,E4,E4,D4,D4,C4};
//    int durations[] = {400,400,400,400,400,400,800, 400,400,400,400,400,400,800};
//    Use a for loop to play each note.
//
// 3. SHARP KEYS: Add a 5th button that raises the current note by
//    one semitone (multiply frequency by 1.0595).
//    This simulates a "sharp" key on a piano!
//
// 4. VOLUME CONTROL: Add the potentiometer from Session 10 on A0.
//    Map its reading to a note duration (50–1000 ms) so the pot
//    controls how long each note rings after you release the button.
//    (Hint: use the pot value to set the tone() duration parameter.)
```

---

## 5. Student Worksheet

---

### Session 16 Worksheet — Make Music: A Button Piano

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Build a 4-button piano that plays different notes.
- Use arrays to organize frequencies and pin numbers.
- Connect frequency values to musical pitch and the octave relationship.

---

#### What I Already Know (Warm-Up)

1. From Session 15: What function plays a specific frequency on the buzzer?

   ___________________________________________________________________________

2. If Middle C is 262 Hz, what frequency is Middle C one octave higher (C5)?

   ___________________________________________________________________________

3. What is an array? How is it different from a regular variable?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Use this checklist to verify your wiring before powering on:

**Buzzer:**
- [ ] Pin 8 → 100 Ω resistor → buzzer **+**
- [ ] Buzzer **–** → GND rail

**For each button (repeat × 4):**

| Button | Pin | 5V side (left leg) | Signal + pull-down side (right leg) | 10 kΩ to GND |
|--------|-----|--------------------|--------------------------------------|--------------|
| 1 | Pin 3 | [ ] 5V connected | [ ] Wire to Pin 3 | [ ] 10 kΩ to GND |
| 2 | Pin 4 | [ ] 5V connected | [ ] Wire to Pin 4 | [ ] 10 kΩ to GND |
| 3 | Pin 5 | [ ] 5V connected | [ ] Wire to Pin 5 | [ ] 10 kΩ to GND |
| 4 | Pin 6 | [ ] 5V connected | [ ] Wire to Pin 6 | [ ] 10 kΩ to GND |

After all checkboxes are ticked: plug in USB, upload code, open Serial Monitor (9600 baud).

---

#### Predict!

Before testing:

| Button | Note | Frequency (Hz) | Is it high or low pitch relative to the others? |
|--------|------|----------------|------------------------------------------------|
| 1 | C4 | 262 | |
| 2 | D4 | 294 | |
| 3 | E4 | 330 | |
| 4 | F4 | 349 | |

What do you predict will happen when you press buttons 1 and 4 at the same time?

___________________________________________________________________________

---

#### Observe / Data Table

Press each button and fill in the table:

| Button | Note playing | Frequency shown on Serial Monitor | Sound quality (nice? harsh? low? high?) |
|--------|-------------|----------------------------------|-----------------------------------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 1 + 2 together | | | |
| 1 + 4 together | | | |

---

#### What Did You Notice?

1. Could you tell the four notes apart by ear? Describe how each sounded compared to the previous one.

   ___________________________________________________________________________

2. What happened when you pressed two buttons at the same time? How does the code decide which note to play? (Look at the `break` statement in the for loop.)

   ___________________________________________________________________________

3. The code uses an array `noteFreqs[]` and `buttonPins[]`. What is the advantage of using arrays instead of writing 4 separate `if` statements?

   ___________________________________________________________________________

4. Try pressing button 4 (349 Hz) and then button 1 (262 Hz) in a rhythm. Can you recognize any melody? What interval do you hear?

   ___________________________________________________________________________

---

#### Science Connection

**The octave and frequency:** When frequency doubles, we say the note is one **octave** higher. This ratio (2:1) is the same in all musical cultures worldwide — it sounds naturally harmonious because the sound waves align every other cycle.

**Complete the table:**

| Note | Frequency (Hz) | One octave higher (×2) | One octave lower (÷2) |
|------|----------------|------------------------|----------------------|
| A3 | 220 | | |
| A4 | 440 | | |
| C4 | 262 | | |
| C5 | 523 | | (should be ≈ C4) |

**Question:** If you change button 2's frequency from 294 Hz to 588 Hz, how does it sound? Is it still D4?

___________________________________________________________________________

---

#### Challenge Extension

**Extend to 8 notes** (full C major scale) using the Challenge section of the code:
- Add 4 more buttons on pins 7, 10, 11, 12.
- Extend the arrays: add G4 (392), A4 (440), B4 (494), C5 (523).
- Change `NUM_BUTTONS = 8`.
- Test by playing "Twinkle Twinkle Little Star" (C-C-G-G-A-A-G).

Notes used: ___________________________________________________________________________

Did it sound like the real song? ___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| High-frequency tones | Same as Session 15. Avoid sustained high-frequency tones (above 3000 Hz) at close range. Offer ear protection to sensitive students. |
| Button wiring | With 4 buttons, the breadboard is more crowded. Double-check no wires are accidentally bridging adjacent rows. |
| Buzzer with no resistor | The 100 Ω resistor limits current to the buzzer and reduces volume. Do not remove it unless volume is too low. |

**Build → Check → Power on.** This is the most complex circuit so far — a thorough partner check before powering on is especially important.

**If something goes wrong:**
- Multiple notes play at once → check that only one button at a time sends HIGH to its pin.
- No notes → check the buzzer circuit first (upload Session 15 code temporarily to confirm buzzer works).
- Wrong notes → check that `buttonPins[]` matches the physical wiring exactly.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| All 4 buttons produce distinct, audibly different pitches | | |
| Student can identify which element in the `notes[]` array controls which pitch | | |
| Student can explain why we use arrays instead of 4 separate variables | | |
| Student can calculate the octave above/below a given frequency | | |
| Student attempts the 8-note extension or melody challenge | | |

---

## 8. Differentiation

### Support
- Provide a pre-wired "starter circuit" for the buzzer section; students only add buttons one at a time and test after each one.
- Give a step-by-step button checklist (included in the worksheet) — this helps students track which of the 12 connections they've made.
- Provide the code with the array values pre-filled and clearly labeled; students only need to understand the concept, not derive the frequencies.
- Allow students to start with 2 buttons instead of 4, then add more if time allows.

### Extension
- Challenge: encode a full melody as two arrays (frequencies + durations) and play it automatically — introduction to music programming.
- Investigate the **chromatic scale**: calculate all 12 note frequencies from C4 to B4 using the formula `f = 261.63 × 2^(n/12)` where n = 0..11. Enter all 12 into the array.
- Build a **drum machine**: assign 4 buttons to 4 different frequencies (low bass, snare, hi-hat, cymbal) and press them rhythmically to create a beat.
- Research: *"How does a MIDI keyboard work? How does it send note and frequency information to a computer?"*

### Visual / Kinesthetic Accommodations
- Label each button with a sticky note (C4, D4, E4, F4) so students can visually identify keys.
- Print a simple piano keyboard diagram showing which keys correspond to which frequencies — students draw their Arduino's 4 keys on the diagram.
- For students who are musically engaged: once the piano works, let them experiment freely for 5 minutes making up short melodies.
- For hearing-impaired students: hold the buzzer against the desk surface and feel different frequencies as different vibration intensities — lower frequencies produce stronger felt vibrations.
