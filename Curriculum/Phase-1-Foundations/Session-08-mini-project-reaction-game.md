# Week 4, Session 8 — Mini-Project: LED Reaction Game ⏱️ (GRADED)

**Phase:** Phase 1 — Foundations (Capstone of Phase 1)

## Learning Objectives
- Combine everything from Phase 1: LED **output**, button **input**, resistors, variables, `if`-statements, and the Serial Monitor.
- Measure **time** in code using `millis()` and report it to the Serial Monitor.
- Apply the **scientific method** to a real experiment: measure and compare human **reaction time**.
- Collaborate to build, test, and debug an independent project.

## Science Curriculum Link
**Circuit theory review + Data & the Scientific Method.** Students reuse Ohm's Law (resistors protect the LED), digital input/output, and electrical logic. They also collect **real data** (reaction times in milliseconds), forming and testing a hypothesis (e.g., "reaction time improves with practice"), previewing Phase 2's data focus.

## Materials Checklist (per pair)
- Arduino Mega 2560 + USB cable — 1
- Breadboard — 1
- LED (green) — 1
- 220 Ω resistor — 1
- Pushbutton (tactile) — 1
- 10 kΩ resistor — 1 (pull-down for the button)
- Jumper wires (red, black, 2 signal colors) — ~5
- Computer with Arduino IDE + Serial Monitor — 1

---

## TEACHER GUIDE (45-minute breakdown)

### 0–5 min — Hook / Warm-Up
**Demo / Ask:** Hold a ruler vertically and have a student catch it when you drop it (classic reaction-time trick). *"How fast are your reflexes? Could we build a machine that measures exactly how quick you are — in milliseconds?"*
Tell them: today they build a **reaction-timer game** that does exactly that, using everything from Phase 1.

### 5–15 min — Direct Instruction
**Concept review + new tool `millis()`:**
- Quick recap (call-and-response): What does a 220 Ω resistor do? What does `digitalRead` return? What does an `if` statement do?
- **New idea — `millis()`:** the Arduino has an internal stopwatch that counts **milliseconds since power-on**. `millis()` reads it.
- **The trick to measure reaction time:**
  1. Turn the LED ON and record the start time: `startTime = millis();`
  2. Wait for the player to press the button.
  3. The instant they press, record the end time and subtract: `reaction = millis() - startTime;`
- **Analogy — a stopwatch:** Start the watch when the light flashes, stop it when they hit the button. The difference is their reaction time.
- **Fairness (scientific method):** add a **random delay** before the LED lights so players can't cheat by anticipating.

### 15–35 min — Hands-On Build + Code
**This is a graded build — students work as independently as possible; teacher circulates.**
1. **Power down.** *Build → Check → Power on.*
2. Build the **LED circuit:** pin **8 → 220 Ω → LED(+long) → LED(–short) → GND**.
3. Build the **button circuit:** one side to **5V**, the other side to **pin 2** *and* through a **10 kΩ** pull-down resistor to **GND**.
4. **Partner check**, then **power on**.
5. Enter the sketch below, Upload, and open the **Serial Monitor** (magnifying glass icon, 9600 baud).
6. Play 5 rounds; record times.

### 35–42 min — Testing & Debugging
**Success looks like:** after a random pause the LED lights; pressing the button prints a reaction time (ms) to the Serial Monitor; a too-early press prints a "cheated!" message.

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Nothing prints in Serial Monitor | Baud rate mismatch | Set Serial Monitor to **9600** |
| Button "presses itself" / random numbers | Missing 10 kΩ pull-down | Add pull-down resistor to GND |
| LED never lights | LED backwards / no resistor / GND | Power down; flip LED; check 220 Ω + GND |
| Time is always 0 or huge | Start/stop logic order wrong | Check `startTime` is set right before waiting |

### 42–45 min — Reflection / Exit Ticket
**Exit ticket:** *"Write your fastest reaction time. Did your times get faster with practice? What is one variable you kept the same to make it a fair test?"*

---

## CIRCUIT DIAGRAM

```
   ARDUINO MEGA
  ┌────────────┐
  │   Pin 8    ├──[220Ω]──►|── LED ──┐
  │            │                     │
  │   GND      ├─────────────────────┴────────────┐ (LED short leg → GND)
  │            │                                   │
  │   5V       ├────────────────┐                  │
  │            │            [ BUTTON ]             │
  │   Pin 2    ├──────┬─────────┘ (other side)     │
  │            │      │                            │
  │            │   [10kΩ]                          │
  │   GND      ├──────┴────────────────────────────┘ (pull-down to GND)
  └────────────┘
```

**Fritzing-style build description:**
- **LED side:** Pin **8** → **yellow** wire → **220 Ω** → LED **long leg (+)**; LED **short leg (–)** → **black** wire → **GND** rail.
- **Button side:** **5V** (red) → one button leg. Opposite button leg → **green** wire to pin **2**, AND through a **10 kΩ** resistor → **GND** (this pull-down keeps pin 2 at 0V until pressed).
- All grounds share the breadboard **GND rail** back to an Arduino **GND**.

[Google image search: **"Arduino pushbutton pull-down resistor 10k LED reaction game breadboard"**]

---

## ARDUINO CODE

```cpp
/*
  ===================================================================
  SESSION 8 — LED REACTION-TIME GAME (Phase 1 Mini-Project)
  THE SCIENCE:
  This combines a digital OUTPUT (LED, protected by a 220Ω resistor)
  and a digital INPUT (button with a 10kΩ pull-down resistor).
  The Arduino's built-in stopwatch, millis(), counts milliseconds.
  By recording the time the LED turns on and the time the button is
  pressed, we measure HUMAN REACTION TIME — real experimental data!
  ===================================================================
*/

int ledPin = 8;       // Green LED (through 220Ω)
int buttonPin = 2;    // Pushbutton (with 10kΩ pull-down)

long startTime;       // When the LED turned ON (milliseconds)
long reactionTime;    // How long the player took to press

void setup() {
  pinMode(ledPin, OUTPUT);     // LED pin sends power out
  pinMode(buttonPin, INPUT);   // Button pin reads HIGH/LOW
  Serial.begin(9600);          // Open the Serial Monitor at 9600 baud
  Serial.println("REACTION GAME! Press the button when the LED lights.");
  randomSeed(analogRead(A0));  // Make the random delay different each game
}

void loop() {
  Serial.println("Get ready...");
  digitalWrite(ledPin, LOW);          // LED off while we wait

  // >>> TRY CHANGING THIS: random wait between 2 and 5 seconds
  int waitTime = random(2000, 5000);
  delay(waitTime);

  // Cheat check: if they are already holding the button, call it out
  if (digitalRead(buttonPin) == HIGH) {
    Serial.println("Too early — no cheating! Try again.");
    delay(1500);
    return;                            // Restart the loop
  }

  digitalWrite(ledPin, HIGH);          // LIGHT ON — go!
  startTime = millis();                // Start the stopwatch

  // Wait here until the button is pressed
  while (digitalRead(buttonPin) == LOW) {
    // do nothing — just keep checking
  }

  reactionTime = millis() - startTime; // Stop the stopwatch
  digitalWrite(ledPin, LOW);           // Turn the LED off

  Serial.print("Your reaction time: ");
  Serial.print(reactionTime);
  Serial.println(" milliseconds");
  Serial.println("------------------------------");
  delay(2000);                         // Pause before the next round
}

/*
  // ===== CHALLENGE =====
  // 1) Keep a "best score": store the smallest reactionTime in a
  //    variable and print "NEW RECORD!" when it is beaten.
  // 2) Two-player mode: add a second button + LED and a winner check.
  // 3) Add a buzzer (preview of Phase 2) that beeps when the LED lights.
  // 4) FAIR TEST: have one partner test left hand vs right hand, 5 tries
  //    each. Average them. Which hand is faster? Is the difference real?
*/
```

---

## STUDENT WORKSHEET

### ⏱️ How Fast Are You? — The Reaction Game
**Objectives:** Build a working reaction timer and run a fair experiment on your reflexes.

### What I Already Know
1. Why does the button need a **pull-down resistor**? What would happen without it?
2. What does an `if` statement let your program do?
3. What units does `millis()` measure time in?

### Build It — Step by Step
1. **Power down.** Build the LED circuit (pin 8 → 220 Ω → LED → GND).
2. Build the button circuit (5V → button → pin 2, with 10 kΩ to GND).
3. **Partner check**, **power on**, upload, open the **Serial Monitor (9600)**.
4. Play 5 rounds. Press the button the instant the LED lights!

### Predict! (before playing)
- What do you think your reaction time will be in milliseconds? (1 second = 1000 ms.)
- Do you think your time will get **faster** after a few tries? Why?

### Observe / Data Table
| Round | Reaction time (ms) — You | Reaction time (ms) — Partner |
|-------|--------------------------|------------------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |
| **Average** | | |

### What Did You Notice?
1. What was your fastest time? Your slowest?
2. Did practice make you faster? What's your evidence?
3. What happened if you pressed too early?
4. What could make the game *unfair*, and how did the random delay help?

### Science Connection
**How does this relate to data and the scientific method?** Write a hypothesis you could test with this device (e.g., "I react faster with my dominant hand"). What would you keep the same to make it a **fair test**?

### Challenge Extension
Add a "best score" record, OR turn it into a 2-player race with a second button and LED. Describe what you changed.

---

## SAFETY NOTES
- **Build → Check → Power on.** Rewire only with USB unplugged.
- Keep the **220 Ω** on the LED and the **10 kΩ** pull-down on the button.
- Don't smash the button — press firmly, not violently (the legs can bend/break).
- Watch for short circuits: 5V should reach the button, never go straight to GND.
- Any heat or burning smell → **unplug immediately**, tell the teacher.

---

## ASSESSMENT RUBRIC (GRADED — Phase 1 Mini-Project)

| Criterion | 1 — Beginning | 2 — Developing | 3 — Proficient | 4 — Exemplary |
|-----------|---------------|----------------|----------------|----------------|
| **Circuit Construction** | Circuit incomplete; needs major help | Builds with hints; one wiring error | LED + button circuits correct, resistors in place | Neat, correct, color-coded wiring; helps another pair |
| **Code Functionality** | Code won't compile/upload | Uploads but timer or button logic faulty | Game works: random delay, times the press, prints ms | Adds a working CHALLENGE (best score / 2-player) |
| **Science Understanding** | Cannot explain the parts | Explains LED OR button, not both | Explains output, input, resistors, and what reaction time means | Explains it AND designs a fair test/hypothesis with controls |
| **Collaboration** | One partner does all work | Uneven roles | Both share build & code, swap roles | Strong teamwork; clear shared decisions; supports others |
| **Reflection Quality** | Blank/one word | Surface answers | Thoughtful answers + a testable hypothesis | Insightful; identifies variables and proposes a next experiment |

**Score:** ___ /20  (≥17 Exemplary · 13–16 Proficient · 9–12 Developing · ≤8 Beginning)

---

## DIFFERENTIATION
**Support:**
- Provide the LED + button circuits **pre-built** so the pair focuses on running the experiment and reading data.
- Give the code with the timing lines (`startTime`, `reactionTime`) as `// FILL IN` blanks and a hint card.
- Sentence starters for the hypothesis: "I think ___ will be faster than ___ because ___."

**Extension (early finishers):**
- Implement best-score memory and a 2-player mode.
- Run a proper experiment: 10 trials each, average, and decide if a difference (e.g., dominant vs non-dominant hand) is **real or just luck**.
- Add a buzzer "start beep" (bridges into Phase 2).

**Visual / Kinesthetic:**
- Do the **ruler-drop reaction test** by hand first, then compare to the electronic version.
- Color-code: yellow = LED signal, green = button signal, red = 5V, black = GND.
- Provide a large-print flowchart of the game loop (Wait → Random delay → Light ON → Press → Show time).
