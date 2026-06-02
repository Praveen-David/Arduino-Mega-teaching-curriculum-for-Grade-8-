# Capstone-B: Project Idea Bank

## 10 Capstone Project Ideas for Your Arduino Mega

> These are **starting points**, not instructions. Each idea is a science question wrapped in engineering. Read through all 10, then pick the one that makes you think "I want to know the answer to that." Then make it your own.

---

## How to Read Each Entry

- **The Big Science Question** — the investigation at the heart of the project
- **Components** — what you need from your kit
- **How It Works** — a description of the device in action
- **Sample Hypothesis** — an example "If… then… because…" to inspire yours

---

## Idea 1: The Smart Greenhouse Monitor

**Catchy Name:** "Project GreenThumb"

**Science Concept:** Thermodynamics, plant biology, and feedback control — *Can a microcontroller maintain ideal growing conditions automatically?*

**The Big Science Question:**
How do temperature and light levels in a small enclosed space change over time, and can an automated system maintain them within a target range?

**Components from Kit:**
- DHT11 temperature/humidity sensor
- LDR photoresistor
- SG90 servo motor (controls a "vent flap" made from card)
- Blue or white LED (simulates grow light)
- LCD 16×2 I2C
- 220 Ω resistors, 10 kΩ resistor

**How It Works:**
The DHT11 monitors temperature inside a small cardboard "greenhouse" (a shoebox with a hole for the servo vent). When temperature exceeds the threshold, the servo opens the vent. The LDR monitors light level and turns on an LED "grow light" when it gets too dark. The LCD displays current temperature, humidity, and light level. Serial Monitor logs all readings as CSV data.

**Sample Hypothesis:**
IF the ambient temperature inside the model greenhouse rises above 28°C, THEN the servo vent will open and temperature will return to below 28°C within 60 seconds, BECAUSE forced convection through the open vent allows heat to escape the enclosed space faster than it accumulates.

**Extension Ideas:**
- Add a potentiometer to let the user set the temperature threshold manually
- Track how long the vent stays open — is that related to how hot it got?

---

## Idea 2: The Weather Station & Data Logger

**Catchy Name:** "Project SkyWatch"

**Science Concept:** Meteorology and thermodynamics — *Can a low-cost device track and log real weather patterns?*

**The Big Science Question:**
How do indoor temperature and humidity change throughout a school day, and what patterns emerge when the data is graphed?

**Components from Kit:**
- DHT11 temperature/humidity sensor
- LDR photoresistor (proxy for UV/cloud cover)
- Potentiometer (set alert thresholds)
- LCD 16×2 I2C
- LED indicator (red = above threshold, green = normal)
- Passive buzzer

**How It Works:**
The device continuously reads temperature, humidity, and light level. It displays rolling readings on the LCD. Every reading is logged to the Serial Monitor with a timestamp. Over the course of the class period, a data table is built. The team copies the Serial log to a spreadsheet and creates a line graph.

**Sample Hypothesis:**
IF indoor light levels (measured by the LDR) decrease while temperature remains constant, THEN this correlates with increased cloud cover outside, BECAUSE LDR readings drop proportionally as less solar light passes through windows.

**Extension Ideas:**
- Compare readings at different times of day (morning session vs. afternoon session)
- Add a "comfort index" calculation: if temp > 25°C AND humidity > 70%, display "UNCOMFORTABLE" on LCD

---

## Idea 3: The Human Reaction Time Experiment

**Catchy Name:** "Project Reflexes"

**Science Concept:** Neuroscience and statistics — *Is reaction time affected by practice, age, or the type of stimulus?*

**The Big Science Question:**
Does human reaction time improve with repeated trials, and is there a measurable difference between visual (LED) and auditory (buzzer) stimulus?

**Components from Kit:**
- 2 pushbuttons (one to trigger stimulus, one to record response)
- Red LED (visual stimulus)
- Passive buzzer (auditory stimulus)
- LCD 16×2 I2C
- 220 Ω resistors, 10 kΩ resistors

**How It Works:**
Player 1 presses Button A to start a random countdown (Arduino waits a random 1–4 seconds, then triggers the LED or buzzer). Player 2 presses Button B as fast as possible when they see/hear the stimulus. The Arduino measures the time between stimulus and response using `millis()` and displays it on the LCD. Repeat 10 trials and record all times.

**Sample Hypothesis:**
IF a person performs 10 consecutive reaction-time trials with the same stimulus type, THEN their average reaction time will decrease by at least 10% from the first 5 trials to the last 5, BECAUSE the human brain forms a predictive model that reduces neural processing time with repeated exposure.

**Extension Ideas:**
- Test different people (different ages if possible) and compare averages
- Test "distracted" trials (solve a mental maths problem while waiting) vs. "focused" trials

---

## Idea 4: The Light-Following Robot / LDR Servo Tracker

**Catchy Name:** "Project Sunflower"

**Science Concept:** Phototropism and servomechanisms — *How do plants optimise light collection, and can a machine do the same?*

**The Big Science Question:**
Can a servo-driven sensor array track a moving light source, and how accurately does it maintain alignment compared to a fixed sensor?

**Components from Kit:**
- 2 LDR photoresistors (one on each side of a divider — a "solar tracker")
- SG90 servo motor (rotates the sensor array toward light)
- LED (status indicator)
- LCD 16×2 I2C
- 10 kΩ resistors (voltage dividers for both LDRs)

**How It Works:**
Two LDRs are mounted on opposite sides of a thin cardboard divider, attached to the servo horn. The Arduino reads both LDRs and rotates the servo to balance the readings. When both LDRs read equal values, the sensor is pointing directly at the light source. The imbalance (difference between LDR readings) and servo angle are logged to Serial Monitor.

**Sample Hypothesis:**
IF a light source is moved from the left side to the right side of the tracker, THEN the servo will rotate to follow it within 2 seconds, BECAUSE the difference between LDR readings creates an error signal that drives the servo toward balance (a feedback control loop).

**Extension Ideas:**
- Calculate the total "tracking error" (angle off from ideal) over a 1-minute trial
- Compare tracking performance in bright light vs. dim light

---

## Idea 5: The Parking Distance Alarm

**Catchy Name:** "Project ParkAssist"

**Science Concept:** Echolocation and sound waves — *How does ultrasonic sensing replicate the biological sonar of bats?*

**The Big Science Question:**
How accurately can an ultrasonic sensor measure distance, and is the accuracy consistent across different target materials (soft vs. hard surfaces)?

**Components from Kit:**
- HC-SR04 ultrasonic distance sensor
- Passive buzzer (pitch changes with distance)
- 4 LEDs (green / yellow / yellow / red — distance indicator bar)
- LCD 16×2 I2C
- 220 Ω resistors

**How It Works:**
The HC-SR04 measures the distance to the nearest object. The buzzer tone frequency increases as the object gets closer (like a parking sensor beep). LEDs light up progressively — all green = far away, all red = very close. The LCD displays exact distance in cm. The Serial Monitor logs distance vs. time so accuracy can be measured.

**Sample Hypothesis:**
IF the target surface is hard and flat (like a book cover), THEN the HC-SR04 will measure distance with less than ±2 cm error at ranges from 5–50 cm, BECAUSE hard flat surfaces reflect ultrasonic waves more consistently than soft or curved surfaces.

**Extension Ideas:**
- Compare measured distance to ruler-measured distance across 10 different distances
- Test different materials: cardboard, foam, your hand, a glass bottle

---

## Idea 6: The Sound Level Meter

**Catchy Name:** "Project DeciMeter"

**Science Concept:** Sound waves and intensity — *How does sound energy (amplitude) vary in different classroom environments?*

**The Big Science Question:**
How does sound intensity change during different activities (silent work, group discussion, presentations), and are there times when noise exceeds a comfortable threshold?

**Components from Kit:**
- KY-038 sound sensor module (if available) OR a simple electret microphone module
  - *Alternative if no sound sensor: use the buzzer in "passive input" mode — not as good, but exploratory*
- LED bar (3–4 LEDs showing sound level)
- LCD 16×2 I2C
- Passive buzzer (optional: alert if too loud)
- 220 Ω resistors

**How It Works:**
The analog sound sensor gives a voltage proportional to sound level. The Arduino reads it on an analog pin and maps the value to a 4-LED bar display (quiet → loud). The LCD shows a running average. If sound exceeds a threshold for more than 5 seconds, a brief buzzer alert sounds. Serial Monitor logs readings every second for analysis.

**Sample Hypothesis:**
IF the class transitions from silent individual work to group discussion, THEN the sound sensor reading will increase by at least 30% on average (measured as ADC value on a 0–1023 scale), BECAUSE more simultaneous voices create more total sound pressure in the room.

**Extension Ideas:**
- Map out "quiet zones" vs. "loud zones" in the classroom by holding the sensor in different positions
- Calculate the percentage of time during a lesson that sound levels exceeded the "comfortable" threshold

---

## Idea 7: The Day/Night Light Cycle Simulator

**Catchy Name:** "Project Circadian"

**Science Concept:** Circadian rhythms and photoperiodism — *How do living organisms respond to light cycles, and can a device simulate them?*

**The Big Science Question:**
Can an Arduino device simulate a day/night cycle for a plant or small animal habitat, and does the LDR feedback keep the "dawn" and "dusk" transitions consistent even when ambient light changes?

**Components from Kit:**
- LDR photoresistor (measures ambient light)
- White LED (simulates sunlight — use PWM for "dawn" and "dusk" dimming)
- DHT11 (monitors temperature response to the light cycle)
- LCD 16×2 I2C
- 220 Ω resistor, 10 kΩ resistor

**How It Works:**
The Arduino uses PWM to gradually ramp the LED brightness from 0 (night) to 255 (full day) and back, simulating a 12-hour day cycle compressed to a 5-minute cycle. The LDR measures the LED output to confirm the cycle is working. The DHT11 records any temperature change caused by the LED (even a small one). All data is logged.

**Sample Hypothesis:**
IF the LED intensity increases from 0 to maximum over 60 seconds (simulating dawn), THEN the DHT11 temperature reading will increase by at least 0.5°C near the LED, BECAUSE the LED converts electrical energy to both light and heat energy, demonstrating the connection between light intensity and thermal energy.

**Extension Ideas:**
- Calculate how consistent the "dawn" and "dusk" ramps are across 3 consecutive cycles
- Discuss: how would this device need to change to actually regulate a real plant terrarium?

---

## Idea 8: The Energy Loss / Cooling Curve Experiment

**Catchy Name:** "Project Newton's Fridge"

**Science Concept:** Newton's Law of Cooling and heat transfer — *How does a warm object lose heat to its environment, and what affects the cooling rate?*

**The Big Science Question:**
How does the rate of cooling of a warm object change over time, and does insulation (wrapping in a material) slow the cooling rate measurably?

**Components from Kit:**
- DHT11 temperature sensor (placed near or touching the warm object — e.g., a warm cup of water)
- LED indicator (shows whether temperature is above/below threshold)
- LCD 16×2 I2C
- Serial Monitor (for data logging — time vs. temperature every 10 seconds)

**How It Works:**
A cup of warm water (from a tap — not boiling) is placed on the desk. The DHT11 is held against the cup or placed nearby. The Arduino records temperature every 10 seconds for 10 minutes. The data is copied to a spreadsheet and graphed — the temperature curve is compared to Newton's Law of Cooling (exponential decay). Then the experiment is repeated with the cup wrapped in a material (cloth, bubble wrap) to see how insulation changes the curve.

**Sample Hypothesis:**
IF a warm cup of water (starting at approximately 40°C) cools in an uninsulated condition, THEN the temperature will drop more rapidly in the first 2 minutes than in the last 2 minutes, BECAUSE Newton's Law of Cooling states that the rate of heat loss is proportional to the temperature difference between the object and its surroundings — which decreases over time.

**Extension Ideas:**
- Test 3 insulating materials and compare their cooling curves
- Calculate the "cooling constant" k for each material by fitting the data to an exponential curve

---

## Idea 9: The Theremin-Style Sound Instrument

**Catchy Name:** "Project AirWave"

**Science Concept:** Sound waves, frequency, and pitch — *How do continuous changes in a physical variable (distance) map to changes in sound wave frequency?*

**The Big Science Question:**
Can a sensor-controlled instrument produce a controllable musical scale, and how precisely can a person control pitch using distance or light alone?

**Components from Kit:**
- HC-SR04 ultrasonic sensor (hand distance → pitch)
- OR LDR photoresistor (light → pitch) — use one or both
- Passive buzzer
- LED strip or multiple LEDs (visual pitch indicator)
- LCD 16×2 I2C
- 220 Ω resistors

**How It Works:**
The HC-SR04 measures the distance from the player's hand (5–50 cm). The Arduino maps this distance to a frequency (e.g., 262 Hz = Middle C at 50 cm, up to 1047 Hz = High C at 5 cm). The buzzer plays the corresponding note. The player can "play" a melody by moving their hand. Pressing a button records one measurement per "note" for data analysis.

**Sample Hypothesis:**
IF the distance between the player's hand and the ultrasonic sensor decreases by half (e.g., from 40 cm to 20 cm), THEN the pitch of the note produced will increase by approximately one octave (double the frequency), BECAUSE frequency can be inversely mapped to distance and the musical octave is defined as a doubling of frequency.

**Extension Ideas:**
- Try to play a simple melody ("Mary Had a Little Lamb") and measure how consistently you hit each "note position"
- Calculate the frequency of each note in a C major scale and program precise positions for them

---

## Idea 10: The Smart Fan / Automatic Thermostat

**Catchy Name:** "Project CoolBreeze"

**Science Concept:** Feedback control systems and thermodynamics — *How does a feedback controller maintain a target temperature more efficiently than a simple on/off switch?*

**The Big Science Question:**
Does a proportional fan controller (speed varies with temperature) maintain a more stable temperature than an on/off controller (full speed or off), and is the temperature variance measurably smaller?

**Components from Kit:**
- DHT11 temperature/humidity sensor
- SG90 servo (simulates a fan speed controller — angle represents speed)
- Potentiometer (set the target temperature)
- LCD 16×2 I2C
- LEDs (status: normal / warning / hot)
- Passive buzzer (alert)

**How It Works:**
The potentiometer sets the target temperature. The DHT11 reads actual temperature. The controller compares them:
- **On/Off mode:** servo is either at 0° (fan off) or 180° (fan full speed)
- **Proportional mode:** servo angle is proportional to how far temperature is above the target
The LCD displays target and actual temperature and mode. Data is logged. Teams run trials in both modes and compare how close to the target temperature the system stays.

**Sample Hypothesis:**
IF a proportional controller is used (servo speed proportional to temperature error) instead of an on/off controller, THEN the temperature will stay within ±2°C of the target setpoint more consistently (in a higher percentage of readings), BECAUSE a proportional controller reduces "hunting" (oscillating above and below the target) by making small corrections rather than full on/full off transitions.

**Extension Ideas:**
- Calculate the average deviation from the target temperature for each control mode
- Research "PID control" — what would adding Integral and Derivative terms do?

---

## Not Finding What You're Looking For?

These 10 ideas cover the major science themes in the course. But if you have a completely original idea — great! As long as it:
- Uses components from the kit
- Has a testable science question
- Collects measurable data
- Is achievable in 4 weeks

...then bring it to your teacher in Session 29. Original ideas are always welcome.

**The most important thing isn't which idea you choose — it's how deeply you investigate it.**

---

*Teacher note: Photocopy and laminate a set of these cards for teams to physically handle during the brainstorm. Each idea works well at the Proficient level with one sensor + one output. The extension ideas push teams toward Exemplary. Ideas 1, 2, 5, and 10 have the most "out-of-the-box" presentation appeal if you invite a guest audience.*
