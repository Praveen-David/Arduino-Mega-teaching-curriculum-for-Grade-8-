# Week 8, Session 15 — Buzzers & Sound Waves

**Phase:** Phase 2 — Sensors & Analog Signals
**Session Number:** 15 of 36
**Week:** 8

---

## Learning Objectives

By the end of this session, students will be able to:
- Describe sound as a longitudinal pressure wave and explain the relationship between frequency and pitch.
- Use the Arduino `tone()` function to generate specific frequencies from a passive buzzer.
- Calculate the expected pitch from a frequency using the wave equation (v = f × λ).
- Identify how changing the frequency value changes the perceived pitch of a sound.

---

## Science Curriculum Link

**Grade 8 Concept: Sound Waves, Frequency & Pitch**

Sound travels as a **longitudinal wave** — a series of compressions and rarefactions in a medium (usually air). The **frequency** of a sound wave (measured in Hz = cycles per second) determines its **pitch**: higher frequency = higher pitch. The Arduino's `tone()` function drives a passive buzzer at any frequency we specify, making this a direct, physical demonstration of the frequency-pitch relationship. Students connect the equation **v = f × λ** (wave speed = frequency × wavelength) to the sounds they produce and hear.

---

## Materials Checklist (per pair)

- 1 × Arduino Mega 2560
- 1 × USB-A to USB-B cable
- 1 × Solderless breadboard
- 1 × Passive buzzer (NOT active — passive buzzer requires a frequency signal to make sound)
- 1 × 100 Ω resistor (optional but recommended to protect the buzzer and reduce volume)
- Jumper wires: red × 2, black × 2, orange × 1
- Computer with Arduino IDE installed
- (Optional) A ruler or string for a "standing wave" visual demo
- (Optional) Earplugs or foam ear protection for sound-sensitive students

**Important:** Confirm the buzzer is **passive** (also called a "piezo buzzer" or "piezo transducer"). An **active** buzzer makes only one tone and does not respond to `tone()`. Active buzzers often have a sticker on top; passive ones look like a small metal disc.

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

*Before class: upload a short sketch that plays a single tone (440 Hz, A4) on a buzzer and let it play as students walk in.*

Ask: *"What is that sound? Can you describe it? Is it high or low? Now watch what I do..."*

Change the frequency in real time (or show the Serial Monitor with current frequency). Play 200 Hz (low rumble), then 1000 Hz (medium beep), then 4000 Hz (high squeal).

*"That sound comes from a tiny vibrating piece of piezo-electric material inside this buzzer. When I change one number in the code — the frequency — the pitch changes completely. Sound is nothing more than vibration at a specific speed. Today we're going to connect numbers to notes."*

---

### 5–15 min — Direct Instruction

**Sound as a wave:**

Draw on the board:

```
Compression wave in air:

→ direction of wave travel →

[|||][   ][|||][   ][|||][   ][|||]
 ^         ^
 Compression  Rarefaction

One full cycle (from one compression to the next) = 1 wavelength (λ)
```

> "Sound is a longitudinal wave — the air molecules don't travel with the wave, they just push back and forth. The number of compressions that pass your ear every second is the **frequency** (measured in hertz, Hz). Frequency determines pitch: 440 Hz is the musical note A4 — the reference pitch that all orchestras tune to."

**The wave equation:**

Write on the board:
```
v = f × λ
where:
  v = speed of sound in air ≈ 343 m/s (at 20°C)
  f = frequency (Hz)
  λ = wavelength (meters)
  
Rearranged: λ = v / f
```

Example calculation: *"For 440 Hz: λ = 343 / 440 = 0.78 m (about 78 cm). For 4000 Hz: λ = 343 / 4000 = 0.086 m (8.6 cm). Higher frequency = shorter wavelength."*

**The `tone()` function:**

> "The `tone()` function tells the Arduino to output a square wave at a specific frequency on a digital pin. The buzzer converts those electrical pulses into physical vibrations of a piezo disc, which moves air, which creates sound. The three arguments are: `tone(pin, frequency, duration)`."

Write on the board:
```cpp
tone(8, 440, 500);    // Pin 8, 440 Hz, for 500 milliseconds
noTone(8);            // Stop the tone on pin 8
```

> "We can also leave out the duration and call `noTone()` when we want to stop. That gives us more control."

**Frequency ranges:**
```
Human hearing: roughly 20 Hz – 20,000 Hz
Low bass:       20 –  200 Hz
Mid-range:     200 – 2,000 Hz
High treble: 2,000 – 20,000 Hz
Passive buzzer: best range 100 – 8,000 Hz
```

---

### 15–35 min — Hands-On Build + Code

**Remind students:** *"Build → Check → Power on. Unplug USB before wiring."*

1. Unplug the Arduino.
2. Place the **passive buzzer** in the breadboard. Identify the + and – pins (usually marked on the case; + toward the signal pin).
3. Connect **buzzer + (positive)** through a **100 Ω resistor** to **Arduino digital pin 8**.
4. Connect **buzzer – (negative)** to the **GND rail** (black wire).
5. Connect Arduino **GND** to the breadboard GND rail (black).
6. Connect Arduino **5V** to the breadboard power rail (red) — for any future additions.
7. Partner checks wiring. Plug in USB.
8. Enter and upload the code from Section 4.
9. Listen! The buzzer should play a sequence of tones.
10. **Experiment:** Change the frequency values in the code. Can you make it higher? Lower?
11. **Silence test:** Cover the buzzer holes with your finger. Does the sound change? Why?
12. Try generating 20 Hz — can you hear it? (Probably not — below human hearing!)

---

### 35–42 min — Testing & Debugging

**What success looks like:**
- Buzzer produces clearly different pitches at different frequencies.
- Students can identify which frequencies sound low vs high.
- Calculations of wavelength from the wave equation match expectations (lower frequency = longer wavelength).

**Troubleshooting Table:**

| Symptom | Fix |
|---------|-----|
| No sound at all | Check that pin 8 is connected to the buzzer + pin (through resistor). Make sure it's a PASSIVE buzzer, not an active one. |
| Active buzzer makes one tone regardless of frequency | Replace with a passive buzzer. Active buzzers only make one fixed tone. |
| Very quiet sound | Remove the 100 Ω resistor — it's optional and reduces volume. |
| Sound is continuous (doesn't stop) | Make sure `noTone(8)` is called before `delay()` or you're using the duration parameter. |
| Buzzer is very loud | Lower volume is achieved by using a 100–220 Ω resistor inline or by reducing `tone()` duration. |
| `tone()` causes other pins to glitch | On the Arduino Mega, `tone()` uses Timer 2. If you have other timers in use, there may be interference — use a different pin. |

---

### 42–45 min — Reflection / Exit Ticket

**Exit ticket questions:**

1. *"What is the wavelength of a 1000 Hz sound wave in air? (Use v = 343 m/s.) Show your work."*
2. *"Why does increasing the frequency argument in `tone()` make the pitch higher?"*
3. *"What is the difference between an active and a passive buzzer? Which one works with `tone()`?"*

---

## 3. Circuit Diagram

### ASCII Wiring Diagram

```
   Arduino Mega 2560
   ┌──────────────────────────┐
   │                          │
   │  5V ─────────────────────┼──── Red rail (+)  [for future use]
   │  GND ────────────────────┼──── Black rail (–)
   │                          │
   │  Digital Pin 8 ──────────┼──── [100 Ω] ──── Buzzer (+, positive)
   │                          │                  Buzzer (–, negative) ──── GND rail
   └──────────────────────────┘

   Passive Buzzer polarity:
   (+) side: longer lead OR marked "+"  →  toward pin 8 (through 100 Ω)
   (–) side: shorter lead OR no mark   →  toward GND rail
```

### Fritzing-Style Build Description

| Step | From | To | Wire Color | Notes |
|------|------|----|------------|-------|
| 1 | Arduino **5V** | Breadboard **+ rail** | Red | Power (for future use) |
| 2 | Arduino **GND** | Breadboard **– rail** | Black | Ground |
| 3 | Arduino **Pin 8** | 100 Ω resistor **leg 1** (row 5) | Orange | Tone signal |
| 4 | 100 Ω resistor **leg 2** | Buzzer **+ pin** (row 7) | — | In-line current limit |
| 5 | Buzzer **– pin** | Breadboard **– rail** | Black | |

**Component values:** Passive piezo buzzer, 100 Ω resistor (brown-black-brown-gold) — optional but reduces volume.

[Google image search: "Arduino passive buzzer tone function circuit breadboard"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 * SESSION 15: Buzzers & Sound Waves
 * Arduino Mega 2560 — Grade 8 STEM Curriculum
 * ============================================================
 *
 * SCIENCE EXPLANATION:
 * ------------------------------------------------------------------
 * SOUND = a longitudinal pressure wave moving through a medium (air).
 *
 * The passive buzzer contains a PIEZOELECTRIC DISC — a material that
 * physically vibrates when an electric voltage is applied. The faster
 * it vibrates, the higher the frequency → the higher the pitch.
 *
 * The wave equation:   v = f × λ
 *   v = speed of sound ≈ 343 m/s at 20°C
 *   f = frequency (Hz = cycles per second)
 *   λ = wavelength (meters)
 *
 * Examples:
 *   262 Hz (Middle C):  λ = 343/262 = 1.31 m
 *   440 Hz (A4):        λ = 343/440 = 0.78 m
 *   4000 Hz (high):     λ = 343/4000 = 0.086 m
 *
 * tone(pin, frequency, duration) outputs a square wave at the
 * given frequency on the specified digital output pin.
 * noTone(pin) stops the sound.
 * ------------------------------------------------------------------
 *
 * Circuit:  Passive buzzer + 100 Ω on Pin 8
 * ============================================================
 */

// ---- Pin Definitions ----
const int BUZZER_PIN = 8;   // Passive buzzer connected to digital pin 8

// ---- Frequency Reference (musical notes, in Hz) ----
// These are the standard frequencies for musical notes
// (We'll use these in Session 16 for the Button Piano)
const int NOTE_C4  = 262;   // Middle C
const int NOTE_D4  = 294;
const int NOTE_E4  = 330;
const int NOTE_F4  = 349;
const int NOTE_G4  = 392;
const int NOTE_A4  = 440;   // Concert A — standard tuning reference
const int NOTE_B4  = 494;
const int NOTE_C5  = 523;   // High C
const int NOTE_REST = 0;    // Silence

void setup() {
  Serial.begin(9600);
  pinMode(BUZZER_PIN, OUTPUT);   // Set buzzer pin as output

  Serial.println("=== Session 15: Buzzers & Sound Waves ===");
  Serial.println("Listen to the frequencies!");
  Serial.println("------------------------------------------");
}

void loop() {
  // --- Demo 1: Low to High frequency sweep ---
  Serial.println("Demo 1: Sweeping from low to high frequency...");
  for (int freq = 100; freq <= 4000; freq += 100) {
    tone(BUZZER_PIN, freq, 80);   // Play freq Hz for 80 ms
    delay(90);                     // Short pause between tones
    Serial.print("Playing: ");
    Serial.print(freq);
    Serial.println(" Hz");
  }
  noTone(BUZZER_PIN);   // Stop after sweep
  delay(1000);

  // --- Demo 2: Musical scale (C major) ---
  Serial.println("Demo 2: C major scale...");
  int cMajorScale[] = {NOTE_C4, NOTE_D4, NOTE_E4, NOTE_F4,
                       NOTE_G4, NOTE_A4, NOTE_B4, NOTE_C5};
  // >>> TRY CHANGING THIS: Change 300 to 150 for a faster scale
  int noteDuration = 300;   // Each note lasts 300 ms

  for (int i = 0; i < 8; i++) {
    Serial.print("Note: ");
    Serial.print(cMajorScale[i]);
    Serial.println(" Hz");

    tone(BUZZER_PIN, cMajorScale[i], noteDuration);   // Play note
    delay(noteDuration + 50);   // Duration + short gap between notes
  }
  noTone(BUZZER_PIN);
  delay(1000);

  // --- Demo 3: Compare wavelengths ---
  Serial.println("Demo 3: Wavelength comparison...");
  Serial.println("  262 Hz (Middle C) → wavelength = 343/262 = 1.31 m");
  tone(BUZZER_PIN, 262, 1000);
  delay(1500);

  Serial.println("  440 Hz (A4)       → wavelength = 343/440 = 0.78 m");
  tone(BUZZER_PIN, 440, 1000);
  delay(1500);

  Serial.println("  880 Hz (A5)       → wavelength = 343/880 = 0.39 m");
  tone(BUZZER_PIN, 880, 1000);
  delay(1500);

  noTone(BUZZER_PIN);
  Serial.println("Cycle complete. Restarting in 3 seconds...");
  delay(3000);   // Pause before repeating
}

// ===== CHALLENGE =====
// 1. WAVELENGTH CALCULATOR: For each note in the C major scale,
//    calculate and print its wavelength using λ = 343.0 / frequency.
//    Print as: "C4: 262 Hz → wavelength = X.XX m"
//
// 2. MORSE CODE: Write the letter "S" (3 short beeps) and "O" (3 long beeps)
//    as SOS: S = 3× 100ms tone, O = 3× 300ms tone
//    Use tone(BUZZER_PIN, 800, duration) and delay() for each.
//
// 3. ULTRASONIC: The Arduino cannot reliably play above 20,000 Hz,
//    but try playing 18,000 Hz, 19,000 Hz, 20,000 Hz.
//    At what frequency can you no longer hear it? Record your answer.
//    (This demonstrates the upper limit of human hearing!)
//
// 4. SIREN: Write a for loop that sweeps from 500 Hz to 2000 Hz and
//    back to 500 Hz continuously — like a fire truck siren.
//    Hint: use two for loops (one going up, one going down).
```

---

## 5. Student Worksheet

---

### Session 15 Worksheet — Buzzers & Sound Waves

**Name(s):** _________________________________ **Date:** _____________ **Kit #:** _____

**Objectives:**
- Describe sound as a longitudinal wave with frequency and wavelength.
- Generate specific frequencies with `tone()` and relate them to pitch.
- Calculate wavelengths using v = f × λ.

---

#### What I Already Know (Warm-Up)

1. Is sound a transverse wave or a longitudinal wave? What is the difference?

   ___________________________________________________________________________

2. Which wave has a higher frequency: a low rumble (bass drum) or a high squeal (whistle)?

   ___________________________________________________________________________

3. If a sound wave has a frequency of 440 Hz, how many compressions of air pass your ear every second?

   ___________________________________________________________________________

---

#### Build It — Step by Step

Unplug USB before wiring.

1. Place the **passive buzzer** in the breadboard (+ leg in row 5, – leg in row 7).
2. **100 Ω resistor** between **Arduino pin 8** and the buzzer **+ leg**.
3. Buzzer **– leg** → **GND rail** (black wire).
4. Arduino **GND** → breadboard **GND rail** (black).
5. Arduino **5V** → breadboard **+ rail** (red) — for future use.
6. Partner checks. Plug in USB. Upload code. Listen!

---

#### Predict!

Before you upload and listen:

| Frequency (Hz) | Your Prediction: High or Low pitch? | Do you expect to hear it? | What you actually observe |
|----------------|-------------------------------------|--------------------------|--------------------------|
| 50 Hz | | | |
| 440 Hz | | | |
| 2000 Hz | | | |
| 10,000 Hz | | | |
| 20,000 Hz | | | |

---

#### Observe / Data Table

Listen to each frequency and describe what you hear:

| Frequency (Hz) | Description of sound | Relative pitch (1=lowest, 5=highest) | Wavelength (m) |
|----------------|----------------------|------------------------------------|----------------|
| 100 | | | |
| 200 | | | |
| 440 | | | |
| 880 | | | |
| 2000 | | | |
| 4000 | | | |

**Wavelength formula: λ = 343 ÷ frequency (Hz)**

---

#### Wavelength Calculations

Calculate the wavelength for each musical note (show all working):

| Note | Frequency (Hz) | λ = 343 ÷ f = ? (m) |
|------|----------------|----------------------|
| Middle C (C4) | 262 | |
| A4 (concert A) | 440 | |
| A5 (one octave higher) | 880 | |
| High C (C5) | 523 | |

**Pattern check:** When frequency doubles (e.g., A4 → A5), what happens to the wavelength?

___________________________________________________________________________

---

#### What Did You Notice?

1. As frequency increased (in Demo 1), did the pitch go higher or lower? Is this what you predicted?

   ___________________________________________________________________________

2. Compare the 440 Hz (A4) and 880 Hz (A5) tones. How are they musically related? (Hint: 880 = 440 × 2)

   ___________________________________________________________________________

3. At what frequency could you no longer hear the tone clearly? What is the technical term for sounds above human hearing range?

   ___________________________________________________________________________

4. The passive buzzer requires the `tone()` function. An active buzzer does NOT. What is the physical difference between them?

   ___________________________________________________________________________

---

#### Science Connection

Sound travels at different speeds in different materials:
- Air (20°C): 343 m/s
- Water: 1,480 m/s
- Steel: 5,120 m/s

**Question:** A 440 Hz sound wave (A4 note) is played underwater. What is its wavelength in water?

λ = 1480 ÷ 440 = ________ m

How does that compare to the wavelength in air (0.78 m)?

___________________________________________________________________________

**Why does sound travel faster in water than in air?** (Think about how closely packed the molecules are.)

___________________________________________________________________________

---

#### Challenge Extension

Try the **Siren Challenge** from the code:
Write a program that sweeps from 500 Hz to 2,000 Hz and back down continuously, like a siren.

Write your code outline here (pseudocode):
```
for frequency from 500 to 2000, step 50:
    tone( ... )
    delay( ... )

for frequency from 2000 down to 500, step 50:
    tone( ... )
    delay( ... )
```

Actual `delay()` value I used: _______ ms. Effect: ___________________________________________________________________________

---

## 6. Safety Notes

| Hazard | Precaution |
|--------|------------|
| Loud high-frequency tones | Keep tone durations short during testing. High-frequency buzzers can be startling and uncomfortable. Give earplugs to sound-sensitive students before starting. |
| Buzzer polarity | Passive buzzers can tolerate reversed polarity briefly, but it's good practice to wire + to signal and – to GND. |
| Volume level | Adding a 100 Ω resistor reduces volume. If the buzzer is uncomfortably loud, increase to 220 Ω. |

**Build → Check → Power on.**

**If something goes wrong:**
- No sound → check it's a passive buzzer; check pin 8 connection; check GND.
- Continuous unchanging tone → may be an active buzzer instead of passive; replace it.
- Other Arduino functions behave strangely → `tone()` uses Timer 2 on the Mega. If another library also uses Timer 2, there may be a conflict. Use a different pin or library.

---

## 7. Assessment Rubric

### Formative Check (not graded)

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Buzzer produces clearly audible, varied tones at different frequencies | | |
| Student correctly calculates wavelength for at least 2 frequencies | | |
| Student can describe sound as a longitudinal wave and explain frequency-pitch relationship | | |
| Student can distinguish between active and passive buzzers | | |
| Student observes that doubling frequency halves wavelength (octave relationship) | | |

---

## 8. Differentiation

### Support
- Print a frequency-pitch reference table (common frequencies and their musical note names) at each desk.
- Provide the wavelength formula already set up in the data table: "λ = 343 ÷ ___" so students only fill in the frequency value.
- For students who struggle with the wave concept, use a physical analogy: shake a slinky spring slowly (low frequency = long wavelength) then fast (high frequency = short wavelength).
- Use the "sweep" demo at the start — this is the most concrete demonstration. Have students listen before thinking about math.

### Extension
- Challenge students to look up the **equal-tempered scale** and calculate the frequency of every note from C4 to B4 (each note is 2^(1/12) ≈ 1.0595 times the previous one).
- Write code that plays **"Happy Birthday"** or **"Twinkle Twinkle"** as a sequence of notes with correct durations. This previews Session 16.
- Research: *"How does a speaker work? How is it similar to a passive buzzer? How is it different?"*
- Explore **harmonics**: play a tone at 440 Hz, then simultaneously play 880 Hz (its second harmonic). Do they sound pleasant together? Why? (Introduction to music theory.)

### Visual / Kinesthetic Accommodations
- Use a **vibrating tuning fork** or a ruler clamped to the desk (pluck it) to show physical vibration as a physical analogue to the buzzer's piezo disc.
- For students with hearing impairments: hold the buzzer against the desk surface — low frequencies (100–300 Hz) can be felt as vibration through the tabletop.
- Show a slow-motion video of a speaker cone or piezo element vibrating (search online) to connect the abstract wave diagram to a physical vibrating object.
- Color-code the wave diagram: compressions in red, rarefactions in blue, wavelength labeled with a green arrow.
