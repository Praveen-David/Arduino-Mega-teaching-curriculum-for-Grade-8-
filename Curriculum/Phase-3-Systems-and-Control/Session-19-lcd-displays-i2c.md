# Week 10, Session 19 — LCD Displays (16×2 I2C)

**Phase:** Phase 3 — Systems & Control
**Week:** 10 | **Session:** 19 of 36

---

## Learning Objectives

By the end of this session, students will be able to:
- Wire a 16×2 LCD display with an I2C backpack to the Arduino Mega (SDA = pin 20, SCL = pin 21).
- Install and use the `LiquidCrystal_I2C` library to initialize and write text to the LCD.
- Explain why humans need **output systems** to communicate information from machines.
- Adjust the I2C backpack contrast potentiometer to optimize display readability.

---

**Science Curriculum Link:** Output systems & communication of information (Grade 8 Systems & Technology).
An LCD is an **output transducer** — it converts electrical signals (the Arduino's digital data) into human-readable light patterns. Just as a speaker converts electrical signals to sound waves, the LCD converts data into visible characters, closing the information loop between machine and human.

---

## Materials Checklist (per pair)

- [ ] 1 × Arduino Mega 2560 + USB cable
- [ ] 1 × Solderless breadboard (full size)
- [ ] 1 × 16×2 LCD with I2C backpack (PCF8574 chip; I2C address usually `0x27` or `0x3F`)
- [ ] 4 × Male-to-female (M-F) jumper wires (red, black, and 2 signal colors)
- [ ] 1 × Small flat-head screwdriver (for contrast potentiometer on the backpack)
- [ ] Computer with Arduino IDE and `LiquidCrystal_I2C` library installed

---

## 2. Teacher Guide (45-minute breakdown)

### 0–5 min — Hook / Warm-Up

**Ask the class:** "Think of three devices around you that show information on a small screen. How does the device 'talk' to you? What would be different if it just beeped instead?"

*Expected answers:* microwave, calculator, phone, clock, fitness tracker. Students should notice that visual text communicates more precise information than a beep.

**Follow-up:** "Today we're giving our Arduino a 'voice' — a screen it can use to tell us exactly what it's thinking."

---

### 5–15 min — Direct Instruction

**Concept: Output transducers and information communication**

*Words to say:*
"So far our Arduino has communicated with us through LEDs (on/off) and the Serial Monitor on the computer. But what if we want to read data without a laptop plugged in? We need a **display** — an output transducer that converts digital data into visible text.

Our LCD has 16 columns and 2 rows — enough for two short sentences. The backpack on the back uses a protocol called **I2C** (eye-squared-see). I2C is like a telephone conversation between chips: the Arduino sends a message on just two wires — SDA (data) and SCL (clock) — and the backpack listens and drives all 16 pins of the LCD for us. One big advantage: instead of using 6+ wires to drive the LCD, I2C uses **only 2 signal wires**. That's the power of communication protocols.

**Mega I2C pin note — very important:** On the Arduino Uno I2C lives on pins A4/A5. On our Mega it is on **dedicated pins 20 (SDA) and 21 (SCL)**. These are the only pins that work. Using A4/A5 will NOT work on the Mega."

Draw on the board:

```
Arduino Mega                I2C Backpack (PCF8574)        LCD Panel
  pin 20 (SDA) ──────────►  SDA  ──────────────────────►  [D4-D7, RS, EN, R/W]
  pin 21 (SCL) ──────────►  SCL                            16 columns × 2 rows
  5V           ──────────►  VCC
  GND          ──────────►  GND
```

**Contrast:** "There's a tiny blue potentiometer on the backpack. If the screen is blank or shows all black squares, that pot needs to be turned — this is just like adjusting the brightness knob on an old TV."

**Library:** Show the `LiquidCrystal_I2C` library in the Library Manager (Sketch → Include Library → Manage Libraries → search "LiquidCrystal I2C" by Frank de Brabander or similar).

---

### 15–35 min — Hands-On Build + Code

**Remind students:** Build → Check → Power on. Wire while USB is unplugged.

**Building the circuit (numbered steps):**

1. Place the LCD display flat on the desk beside the breadboard (it does not insert into the breadboard — it uses M-F jumper wires).
2. Locate the 4-pin header on the I2C backpack: **GND · VCC · SDA · SCL** (left to right when pins face you).
3. Connect a **black** M-F wire from backpack **GND** → Arduino **GND**.
4. Connect a **red** M-F wire from backpack **VCC** → Arduino **5V**.
5. Connect a **blue** M-F wire from backpack **SDA** → Arduino **pin 20**.
6. Connect a **yellow** M-F wire from backpack **SCL** → Arduino **pin 21**.
7. Do a partner check: red to 5V, black to GND, SDA to 20, SCL to 21. ✓
8. Plug in USB. If the backlight turns on but shows black boxes, adjust the contrast pot gently with a screwdriver until characters appear (you may need to upload code first for characters to show).

**Entering the code:**
9. Open Arduino IDE. Create a new sketch.
10. Copy/type the code from Section 4 below.
11. Confirm the library is installed (Sketch → Include Library → check for `LiquidCrystal_I2C`).
12. Upload. The LCD should display "Hello, World!" on row 1 and "Arduino Mega!" on row 2.
13. If the display is blank: adjust the contrast pot (small screwdriver on the blue pot on the backpack).
14. If still blank: open the I2C Scanner sketch (search online or see troubleshooting below) to find your LCD's I2C address.

---

### 35–42 min — Testing & Debugging

**What success looks like:** Row 1 shows "Hello, World!" and Row 2 shows "Arduino Mega!" with clear, readable characters. The backlight is on.

**Troubleshooting table:**

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Screen fully dark, no backlight | Power wiring wrong | Check red → 5V, black → GND |
| Backlight on, all black squares | Contrast too high | Turn contrast pot counter-clockwise slowly |
| Backlight on, no characters | Wrong I2C address in code | Run I2C scanner; change `0x27` to `0x3F` (or vice versa) |
| Backlight on, garbled characters | SDA/SCL swapped | Swap pins 20 and 21 wires |
| "No such file" error in IDE | Library not installed | Sketch → Include Library → Manage Libraries → install LiquidCrystal_I2C |
| Characters only on row 1 | `lcd.setCursor(0,1)` missing | Check code — see Section 4 |

---

### 42–45 min — Reflection / Exit Ticket

Ask students to write answers on a sticky note or in their worksheet:

1. "What two Arduino Mega pins does I2C use, and what are their names?"
2. "Name one advantage of I2C over wiring the LCD directly (hint: think about wire count)."
3. "In your own words, what is an output transducer?"

---

## 3. Circuit Diagram

```
  ARDUINO MEGA 2560
  ┌─────────────────────────────────────┐
  │                                     │
  │  pin 20 (SDA) ──────────────────────┼──► [blue wire]  ──► SDA  ─┐
  │  pin 21 (SCL) ──────────────────────┼──► [yellow wire]──► SCL  ─┤
  │  5V           ──────────────────────┼──► [red wire]   ──► VCC  ─┤  I2C BACKPACK
  │  GND          ──────────────────────┼──► [black wire] ──► GND  ─┘  (PCF8574)
  │                                     │                       │
  └─────────────────────────────────────┘                       │
                                                     ┌──────────▼──────────┐
                                                     │  16×2 LCD DISPLAY   │
                                                     │  Row 1: _ _ _ _ _   │
                                                     │  Row 2: _ _ _ _ _   │
                                                     │  [◄ contrast pot ►] │
                                                     └─────────────────────┘
```

**Fritzing-style build description (numbered connections):**

| # | From | To | Wire Color | Notes |
|---|------|----|-----------|-------|
| 1 | Arduino **GND** | Backpack **GND** (pin 1) | Black | Ground reference |
| 2 | Arduino **5V** | Backpack **VCC** (pin 2) | Red | Powers LCD + backlight |
| 3 | Arduino **pin 20** | Backpack **SDA** (pin 3) | Blue | I2C Data line |
| 4 | Arduino **pin 21** | Backpack **SCL** (pin 4) | Yellow | I2C Clock line |

> No breadboard is needed — the 4 wires connect directly from the Mega to the backpack's 4-pin header using M-F jumper wires.

> **Contrast pot:** Located on the back of the I2C backpack. Turn slowly with a small flat-head screwdriver until text appears clearly. If characters are invisible but the backlight is on, this is always the first thing to adjust.

[Google image search: "Arduino Mega LCD I2C wiring diagram 16x2 SDA SCL pin 20 21"]

---

## 4. Arduino Code

```cpp
/*
 * ============================================================
 *  Session 19 — LCD Display with I2C (16x2)
 *  Arduino Mega Teaching Curriculum — Phase 3
 * ============================================================
 *
 *  SCIENCE EXPLANATION:
 *  ─────────────────────────────────────────────────────────
 *  An LCD (Liquid Crystal Display) works by using electric
 *  fields to rotate tiny liquid crystal molecules, which
 *  either block or allow a backlight to shine through. Each
 *  character position on the screen is a 5×8 grid of tiny
 *  "pixels" (dots). By switching the right ones on/off, the
 *  LCD draws letters and numbers.
 *
 *  I2C (Inter-Integrated Circuit) is a two-wire communication
 *  protocol invented by Philips. The clock wire (SCL) beats
 *  like a metronome; the data wire (SDA) carries bits in time
 *  with each beat. On the Arduino Mega, I2C lives on:
 *    SDA = pin 20
 *    SCL = pin 21
 *  (Different from the Uno's A4/A5 — important!)
 *
 *  The PCF8574 chip on the I2C backpack has a unique address
 *  (like a house number on the I2C "street") — usually 0x27
 *  or 0x3F. If your display doesn't work, try the other one.
 * ============================================================
 *
 *  HARDWARE:
 *    LCD VCC  → 5V
 *    LCD GND  → GND
 *    LCD SDA  → Mega pin 20
 *    LCD SCL  → Mega pin 21
 * ============================================================
 */

// Include the I2C library (built into Arduino IDE)
#include <Wire.h>

// Include the LCD library — install via Library Manager if needed
// Search: "LiquidCrystal I2C" by Frank de Brabander
#include <LiquidCrystal_I2C.h>

// Create an lcd object.
// Arguments: I2C address, number of columns, number of rows
// Try 0x27 first; if blank try 0x3F
// >>> TRY CHANGING THIS: change 0x27 to 0x3F if your screen stays blank
LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  // Initialize the LCD hardware
  lcd.init();

  // Turn on the backlight (the screen backlight is off by default)
  lcd.backlight();

  // Move the cursor to column 0, row 0 (top-left corner)
  // Columns count 0–15 (left to right)
  // Rows count 0–1 (row 0 = top, row 1 = bottom)
  lcd.setCursor(0, 0);

  // Print text to row 0 starting at column 0
  // >>> TRY CHANGING THIS: change "Hello, World!" to your name
  lcd.print("Hello, World!");

  // Move cursor to the start of row 1 (bottom row)
  lcd.setCursor(0, 1);

  // Print a second line of text
  // >>> TRY CHANGING THIS: change to your school or class name
  lcd.print("Arduino Mega!");
}

void loop() {
  // Nothing in loop for now — the LCD keeps showing text
  // until new text is written or the board is reset.

  // In future sessions we will update the display every few
  // seconds with live sensor readings!
}


// ===== CHALLENGE =====
// 1. SCROLLING: Make text scroll across the screen using
//    lcd.scrollDisplayLeft() in a for-loop with a delay.
//    Example:
//      for (int i = 0; i < 16; i++) {
//        lcd.scrollDisplayLeft();
//        delay(300);
//      }
//
// 2. CURSOR BLINK: Add lcd.blink() after lcd.backlight() and
//    watch the cursor flash. Then try lcd.cursor() for an
//    underline cursor.
//
// 3. COUNTDOWN: Print a countdown from 10 to 0 on row 2,
//    updating every second. Use a for-loop, lcd.setCursor(0,1),
//    lcd.print(i), and delay(1000). Don't forget lcd.clear()
//    or the old digit will stay there!
//
// 4. CUSTOM CHARACTER: Look up "LiquidCrystal_I2C createChar"
//    to draw a tiny custom graphic (like a heart or smiley).
```

---

## 5. Student Worksheet

### Session 19 — LCD Displays (16×2 I2C)
**Name(s):** _________________________ **Date:** _________ **Kit #:** ______

**Objectives:** Wire an I2C LCD to the Arduino Mega, write text to it, and explain why displays are important output systems.

---

#### What I Already Know (Warm-Up)

Answer these before the session starts:

1. Name two devices (not phones or computers) that display information on a small screen.
   > ___________________________________________________________________

2. How many wires does a normal headphone plug use? Why does using fewer wires matter in electronics?
   > ___________________________________________________________________

3. What does "output" mean in science? Give an example of an output from a system you use every day.
   > ___________________________________________________________________

---

#### Build It — Step by Step

Follow along with your teacher. Check each box when done.

- [ ] **Step 1:** Identify the 4-pin header on the I2C backpack: GND · VCC · SDA · SCL
- [ ] **Step 2:** Connect **black** wire: Arduino GND → Backpack GND
- [ ] **Step 3:** Connect **red** wire: Arduino 5V → Backpack VCC
- [ ] **Step 4:** Connect **blue** wire: Arduino **pin 20** → Backpack SDA
- [ ] **Step 5:** Connect **yellow** wire: Arduino **pin 21** → Backpack SCL
- [ ] **Step 6:** Partner check — have your partner verify all 4 wires before powering on
- [ ] **Step 7:** Enter and upload the code
- [ ] **Step 8:** If screen is blank, adjust the contrast pot with a screwdriver

**Which I2C address worked for your LCD?** ☐ 0x27   ☐ 0x3F

---

#### Predict!

Answer BEFORE you upload the code:

1. What do you think will appear on the LCD when the code runs?
   > ___________________________________________________________________

2. What do you think will happen if you swap the SDA and SCL wires?
   > ___________________________________________________________________

3. The LCD has 16 columns and 2 rows. If you try to print a 20-character message starting at column 0, what do you think happens to the last 4 characters?
   > ___________________________________________________________________

---

#### Observe / Data Table

Record your observations as you experiment:

| Experiment | What I Changed | What Happened |
|-----------|---------------|--------------|
| 1 — Basic Hello World | Nothing (baseline) | |
| 2 — Change text on row 1 | Changed "Hello, World!" to: ________ | |
| 3 — Change text on row 2 | Changed "Arduino Mega!" to: ________ | |
| 4 — Try wrong I2C address | Changed 0x27 to 0x3F (or vice versa) | |
| 5 — Adjust contrast pot | Turned pot left / right | |

---

#### What Did You Notice?

1. What happened when you used the wrong I2C address? Why do you think I2C devices have addresses?
   > ___________________________________________________________________
   > ___________________________________________________________________

2. On the 16×2 LCD, what happens if you try to print more than 16 characters on one row?
   > ___________________________________________________________________

3. The contrast potentiometer is very sensitive — a tiny turn makes a big difference. What other device have you used with a similar tiny adjustment?
   > ___________________________________________________________________

4. The backpack uses only 2 signal wires (SDA and SCL) instead of 6+ wires needed to drive the LCD directly. In your own words, why is using fewer wires an advantage?
   > ___________________________________________________________________

---

#### Science Connection

An LCD is an **output transducer** — it converts electrical signals into human-readable information.

Think about information flow in a system:

```
SENSOR (input) → ARDUINO (process) → LCD (output) → HUMAN (reads it)
```

1. In your own words, why is the "output" step just as important as the "input" step?
   > ___________________________________________________________________

2. Before LCD displays existed, how did machines communicate information to humans? List two examples.
   > ___________________________________________________________________

3. I2C lets multiple devices share the same two wires. If you had an LCD and a temperature sensor both connected via I2C, how do you think the Arduino knows which device it is talking to?
   > ___________________________________________________________________

---

#### Challenge Extension

1. **Scrolling message:** Look up `lcd.scrollDisplayLeft()`. Make a message longer than 16 characters scroll across the screen.

2. **Timed display:** In `loop()`, make the LCD alternate between two different messages every 2 seconds. Use `lcd.clear()`, `lcd.setCursor()`, and `delay()`.

3. **Research:** Search "I2C scanner Arduino" and find the standard I2C scanner sketch. Explain to your partner how it works in 3 sentences.

---

## 6. Safety Notes

**This session's components and hazards:**

- **LCD Module:** Fragile glass panel — do not flex, press hard on the screen, or drop it. Handle by the PCB edges.
- **I2C Backpack Potentiometer:** Use a correctly-sized small flat-head screwdriver. Do not force the pot past its end stops (you will feel a slight resistance; stop there).
- **5V Power:** The LCD and backpack are 5V devices. Do not connect VCC to the Mega's 3.3V pin or a higher voltage source.
- **Wire polarity:** Double-check red → 5V and black → GND before powering on. Reversed power can damage the LCD backpack chip.
- **I2C pins 20/21 on Mega:** These pins are also used for hardware serial communication. Do not connect other devices that conflict with these pins.

**Build → Check → Power on:** Wire the circuit completely with USB unplugged. Have your partner verify the 4-wire connections before plugging in.

**If something goes wrong:**
- Screen stays fully dark with no backlight → unplug USB, check power wires.
- You smell anything unusual → unplug USB immediately and call the teacher.
- Backpack chip gets hot to the touch → power down, check wiring polarity.

---

## 7. Assessment Rubric

### Formative Check (not graded)

This is an exploratory session introducing a new component. Teacher observes for the following:

| Look-For | Not Yet | Got It |
|----------|---------|--------|
| Correct wiring: SDA → pin 20, SCL → pin 21 (NOT A4/A5) | Wires on wrong pins or reversed | Both wires correctly placed |
| Library installed and code compiles without error | Multiple compile errors; library missing | Code uploads successfully |
| Can explain the role of the LCD as an output transducer | "It just shows stuff" with no further explanation | Uses the term "output transducer" and links it to converting data to visible information |

*Teacher note: Circulate during the 15–35 min build phase. Any pair with wrong I2C address or SDA/SCL swap will have a blank screen — use this as a class teaching moment about troubleshooting.*

---

## 8. Differentiation

### Support

- **Pre-wired starter:** Provide a partially wired setup with the power wires (red/black) already connected; students only need to add SDA and SCL.
- **Address cheat sheet:** Give a printed card showing both common I2C addresses (0x27 and 0x3F) with a note: "Try 0x27 first; if blank, try 0x3F."
- **Code scaffold:** Provide the code with blanks: `lcd.setCursor(__, __);` and `lcd.print("__________");` for students to fill in.
- **Sentence starters for reflection:** "The LCD is an output transducer because it converts _______ into _______."

### Extension

- **I2C Scanner:** Have early finishers upload the standard I2C scanner sketch and document the address and all I2C devices found on the bus.
- **Custom characters:** Research `lcd.createChar()` and draw a custom 5×8 bitmap character (heart, arrow, thermometer icon).
- **Multiple messages:** Write a `loop()` that displays a different message every 3 seconds, cycling through at least 4 different messages using an array of strings.
- **Explain to peers:** Ask the extension student to teach the I2C addressing concept to one other pair using the "house number on a street" analogy.

### Visual / Kinesthetic Accommodations

- **Physical analogy for I2C:** Use a "telephone party line" analogy — one clock wire is the "talking stick" that tells everyone when to speak; each device waits for its address to be called.
- **Large-print wiring card:** Provide a printed color photo of the correct 4-wire connection for students who find the ASCII diagram hard to follow.
- **Hands-on contrast adjustment:** Let all students take a turn adjusting the contrast pot — the satisfying visual feedback makes the concept memorable.
- **Glossary card:** Key terms with pictures — LCD, I2C, SDA, SCL, I2C address, output transducer, backpack.
