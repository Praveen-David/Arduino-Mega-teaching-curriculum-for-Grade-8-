# Week 18, Session 36 — Final Presentations & Reflection

**Phase:** Phase 4 — Capstone Science Project
**Session Type:** Final Presentations / Celebration / Reflection

## Learning Objectives
- **Present** their capstone science project to the class in a clear, organised 5-minute talk with live device demonstration.
- **Respond** to questions from peers and teacher about their science concept, data, and engineering decisions.
- **Evaluate** their own learning growth over the semester through a final written reflection.
- **Celebrate** the work of themselves and their peers with genuine recognition.

**Science Curriculum Link:** Scientific Communication & Integrated Science — The final presentations demonstrate mastery of the full scientific method applied to a real engineering challenge, integrating concepts from all four phases: circuits, sensors, systems, and the scientific method. Students communicate evidence-based conclusions — the final step of every scientific investigation.

## Materials Checklist (per team — prepared before session)
- [ ] Final project device, tested and working
- [ ] Presentation slides or poster (completed in Session 35)
- [ ] USB cable connected and device powered on before presentations begin
- [ ] Any printed data tables or graphs to show the audience
- [ ] Note cards if used for speaking points
- [ ] `Capstone-F-Final-Presentation-Rubric.md` — teacher uses for grading; students have their own copy

**Classroom setup (teacher, before students arrive):**
- [ ] Arrange seats in a U-shape or audience-facing format (not lab pairs) for the presentations
- [ ] Set up projector or display screen for slides
- [ ] Place each team's device on a desk at the front of the room (or on their own desk if doing stations)
- [ ] Print grading copies of `Capstone-F-Final-Presentation-Rubric.md` (one per team, for teacher to fill in)
- [ ] Optional: arrange for a guest audience (another class, parents, the school principal) — students rise to a real audience
- [ ] Have a timer visible (phone/board clock)
- [ ] Prepare small celebration — even just a round of applause protocol

---

## 2. Teacher Guide (45-Minute Breakdown)

### 0–5 min — Hook / Warm-Up

**Do not start with housekeeping. Start with energy.**

**Say this:**
> "Four weeks ago you had an idea and a box of components. Today you are presenting a device that does something real, connected to a science question you cared about enough to investigate. That is an extraordinary thing. Scientists and engineers spend their careers doing exactly this — and today you join that tradition. Let's make it great."

**Set the presentation norms (30 seconds):**
- Audience: phones away, eyes on the presenter, applaud after each presentation.
- Presenters: stand up, speak clearly, make eye contact, power on your device before you begin.
- Questions: ask something genuine — "I noticed that…" or "I'm curious about…"

**Quick logistics (1 minute):**
- Confirm the presentation order (write it on the board).
- Each team has **5 minutes** to present + **2 minutes** for Q&A = 7 minutes per team.
- With 4–6 teams, this uses 28–42 minutes, leaving time for reflection.

*(If you have more than 6 teams in the class, you may need to either extend the session, run simultaneous station presentations, or present some on an earlier day.)*

---

### 5–40 min — Direct Instruction / Hands-On: Presentations

**For each team presentation (7 minutes):**

**Teacher actions during each presentation:**
1. Start the timer as soon as the team begins.
2. Fill in the rubric (Capstone-F) in real time — specific observations, not just a number.
3. After 5 minutes, give a gentle signal (raise a hand) if the team needs to wrap up.
4. After the presentation: facilitate 2 minutes of Q&A. Ask one teacher question yourself to model the kind of question you want to see.

**Great teacher Q&A questions (choose based on the project):**
- "You said your hypothesis was [X]. What does that mean for the science — what would you conclude?"
- "What was the biggest engineering challenge and how did you solve it?"
- "If you could add one more sensor or feature, what would it be and why?"
- "Your data showed [Y]. Was that what you expected? If not, what could explain it?"
- "What real-world problem could your project help solve?"

**Audience participation:**
- After each team, ask the audience: "What is one thing you didn't know before that presentation?"
- Take 2 quick hands. This keeps the audience engaged and reinforces learning.

**Celebration after each team:**
- Lead a genuine, sustained round of applause. Name something specific: "That data graph was really clear" or "That live demo was impressive."

---

### 40–44 min — Reflection / Final Exit Ticket

**After all presentations, students write their final reflection in their project binders.** This is the most important piece of individual writing in the course — give it full attention.

**Written reflection (3 minutes — students write silently):**

Answer ALL of these:

1. **Learning growth:** Think back to Session 1. What is ONE thing you can do now that you definitely could not do then?
   `____________________________________________________________`

2. **Proudest moment:** What is the single moment in this project you are most proud of? Why?
   `____________________________________________________________`

3. **Scientific method:** Explain in 2–3 sentences how your project followed the scientific method from hypothesis to conclusion.
   `____________________________________________________________`
   `____________________________________________________________`

4. **What didn't go as planned:** Describe one thing that went wrong and what you learned from it.
   `____________________________________________________________`

5. **Real world:** How does your project connect to something that matters in the real world?
   `____________________________________________________________`

6. **Next steps:** If you had 4 more weeks to keep working on this project, what would you do?
   `____________________________________________________________`

---

### 44–45 min — Celebration & Close

**In the last 60 seconds:**

**Class shout-out:** Ask students to call out — no hands — one word to describe how they feel right now. Let the words wash over the room.

**Say this as your final statement (mean it):**
> "You walked in here with zero experience in circuits or code. You leave with a working device, real data, and a genuine science investigation. Keep building things. Keep asking questions. The world needs people who can do both."

---

## 3. Circuit Diagram

All circuits are finalised. Devices should be powered on and running for the presentations.

```
  SESSION 36: FINAL PRESENTATIONS
  =================================
  Devices are COMPLETE and powered on.

  PRESENTATION DAY SETUP CHECKLIST:
  ┌──────────────────────────────────────────────────────┐
  │ ✓ USB cable plugged in → device powered on           │
  │ ✓ Serial Monitor CLOSED (not visible to audience)   │
  │ ✓ LCD showing correct, readable data                │
  │ ✓ Device positioned so the class can see outputs    │
  │ ✓ Slides/poster displayed or ready to display       │
  │ ✓ No loose wires that could fall mid-presentation   │
  │ ✓ One team member knows how to trigger the demo     │
  │   (e.g., warm the DHT11 to show the servo respond)  │
  └──────────────────────────────────────────────────────┘

  DEMO MOMENT: Every presentation should have a LIVE DEMO.
  This is the moment when the device does something visible
  to the audience: servo moves, LED changes, LCD updates,
  buzzer sounds briefly.

  Plan your demo trigger in advance:
  - Temperature: hold a warm cupped hand near DHT11
  - Light: cover the LDR with a finger
  - Distance: move a hand toward the HC-SR04

  Practice the demo at least TWICE before the presentation.
```

[Google image search: "student science fair Arduino presentation showcase"]

---

## 4. Arduino Code

No new code today. Devices run their final project code from Sessions 32–34.

```cpp
// =============================================================
// SESSION 36: FINAL PRESENTATION DAY
//
// Your code is DONE. Before presenting, do a final check:
// =============================================================

// FINAL CODE CHECKLIST:
// [ ] No debug Serial.println() statements printing too fast
//     (they will fill the screen and look messy if Monitor is open)
// [ ] LCD shows the project name or a useful status message
//     at startup before readings begin
// [ ] Thresholds match the values from your data collection
//     (you may have adjusted them in Session 34)
// [ ] All magic numbers replaced with #define constants
// [ ] Every function and major code section has a comment
// [ ] The sketch file is saved with a good name:
//     "[YourProjectName]_Final.ino"

// PRESENTATION MOMENT — make your demo obvious:
// Consider adding a brief "celebration" when the threshold
// is crossed during the live demo:

if (temperature > TEMP_HIGH) {
  myServo.write(90);
  digitalWrite(LED_PIN, HIGH);
  // Flash the LED 3 times to make the threshold crossing obvious
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
    delay(100);
  }
  digitalWrite(LED_PIN, HIGH);   // leave LED on after flash
  lcd.setCursor(0, 1);
  lcd.print("THRESHOLD MET!  ");  // clear message on LCD row 2
}

// This makes the demo moment visually unmistakable for the audience.

// =============================================================
// Your complete project code belongs above this section.
// =============================================================
```

---

## 5. Student Worksheet

---
### Worksheet — Session 36: Final Presentations & Reflection

**Team Name:** _________________________ **Date:** _____________
**Team Members:** _________________________________ / _________________________________
**Project Name:** _________________________________

---

#### Audience Notes — While Watching Other Presentations

Fill in a row for each team that presents:

| Team | Project Name | Science Question | Most Interesting Data / Finding | One Question You Would Ask |
|------|-------------|-----------------|--------------------------------|---------------------------|
| | | | | |
| | | | | |
| | | | | |
| | | | | |
| | | | | |

---

#### Our Self-Assessment — Before the Presentation

Rate your team on each criterion from the rubric (1 = Beginning, 4 = Exemplary):

| Criterion | Our Self-Score (1–4) | Evidence / Reason |
|-----------|---------------------|-------------------|
| Science Concept | | |
| Engineering Design | | |
| Code Quality | | |
| Presentation Skills | | |
| Reflection | | |

**Total self-score:** ___/20

---

#### Predict!

1. What question are you most hoping someone asks you? (One you're prepared to answer really well!)
   `____________________________________________________________`

2. What is the ONE result from your data that you most want the audience to understand?
   `____________________________________________________________`

---

#### Observe / After Our Presentation

1. What question did the audience / teacher ask that you found most challenging?
   `____________________________________________________________`
   How did you answer it?
   `____________________________________________________________`

2. Did the live demo work as planned? If not, what happened?
   `____________________________________________________________`

3. How long did your presentation take? _______ minutes. (Was it within 4–6 minutes? _____)

---

#### Final Reflection — The Big One

*(Write thoughtfully — this is your most important piece of writing in the course)*

1. **Learning growth:** Compare yourself now to Session 1. What is ONE specific thing you can do now that you definitely could not do in Week 1?
   `____________________________________________________________`
   `____________________________________________________________`

2. **Proudest moment:** What moment — in any session, not just the capstone — are you most proud of? Be specific.
   `____________________________________________________________`
   `____________________________________________________________`

3. **Scientific method in your project:** In 3 sentences, describe how your project followed the scientific method from start to finish:
   `____________________________________________________________`
   `____________________________________________________________`
   `____________________________________________________________`

4. **What went wrong (and what you learned):** Every project has something that didn't go as planned. Describe one failure or unexpected result — and explain what you learned from it.
   `____________________________________________________________`
   `____________________________________________________________`

5. **Real world connection:** How does your project connect to a real-world technology, problem, or scientific question?
   `____________________________________________________________`
   `____________________________________________________________`

6. **Next steps:** If you had 4 more weeks, what would you add or change? Be specific about the components or code features you would use.
   `____________________________________________________________`
   `____________________________________________________________`

7. **Advice to next year's class:** In 1–2 sentences, what is the most important advice you would give to a student starting Session 1 next year?
   `____________________________________________________________`
   `____________________________________________________________`

---

#### What Did You Notice? — Looking at the Whole Semester

1. Which Phase (1 Foundations, 2 Sensors, 3 Systems, 4 Capstone) challenged you the most? Why?
   `____________________________________________________________`

2. Which session was your favourite to build? What made it memorable?
   `____________________________________________________________`

3. What is the most important SCIENCE concept you understood better because of this course?
   `____________________________________________________________`

---

#### Science Connection

> "Science is not a collection of facts — it is a way of asking questions and finding answers with evidence. Every time you asked 'what happens if I change this?' and then tested it and observed the result, you were doing science. The device you built is just a tool. The question you asked, and the answer you found, is the science."

In one sentence, what is the science your project contributed?
`____________________________________________________________`

---

#### Challenge Extension
*(Optional — for students who want to take this further)*

- Write a **one-page "project paper"** in the style of a science journal article: Abstract (2 sentences), Introduction (your science question and background), Method (how your experiment worked), Results (your data), Discussion (analysis and conclusion), Acknowledgements.
- This is what real scientists submit for publication. It is harder than it looks — and it is excellent practice.

---

## 6. Safety Notes

- **Presentation day finale:** All projects should be **powered down and components returned to kit boxes** at the end of this session. This is the final equipment check of the semester.
- **USB cables:** When the session ends, students unplug USB cables before removing the Arduino from the circuit. This is the correct shutdown procedure.
- **Return of components:** Use the final 5 minutes to ensure all components are returned to the correct kit box compartments. Loose LEDs and resistors are a tripping/ingestion hazard if left on the floor.
- **End-of-course inventory check:** Teacher should do a kit count after all teams return their components. Note any missing or damaged items for future classes.

| Situation | Action |
|-----------|--------|
| Device fails to power on at the start of the presentation | Stay calm; check USB connection; check that the Arduino IDE Serial Monitor is not blocking the COM port; re-upload code if needed |
| Presentation is significantly over time | Give a 30-second wrap-up signal at 5 minutes; at 6 minutes, politely intervene: "Let's move to your conclusion" |
| A student becomes upset or emotional during reflection | This is the end of a significant project — validate the feeling ("This was hard work and you should be proud") and give space |

---

## 7. Assessment Rubric

**GRADED SESSION — Use `Capstone-F-Final-Presentation-Rubric.md` for formal assessment.**

Teacher completes one rubric per team during their presentation. Key indicators:

| Criterion | Evidence to Look For |
|-----------|---------------------|
| Science Concept | Does the conclusion directly answer the science question? Are specific data values cited as evidence? |
| Engineering Design | Does the circuit work correctly? Are components used appropriately? Is the design explained clearly? |
| Code Quality | Is the code shown/described with correct use of libraries, constants, comments? Is the data logging evident? |
| Presentation Skills | Is the 5-minute structure followed? Do all members speak? Is the device demoed live? Is the pace and volume appropriate? |
| Reflection | Does the team honestly discuss what worked and what didn't? Is there a genuine "next steps" idea? |

**Self-Assessment:** Students complete the self-assessment column in the rubric independently after their presentation. Compare with teacher scores in a brief 1:1 or written feedback note.

**Final capstone grade components:**
| Deliverable | Weight within Capstone |
|-------------|----------------------|
| Planning Template (Capstone-C) | 20% |
| Build Log (Capstone-D, all 4 entries) | 30% |
| Final Presentation | 30% |
| Individual Reflection (this worksheet) | 20% |

---

## 8. Differentiation

### Support
- Allow students with presentation anxiety to present to a **smaller group** (half the class, or just the teacher and one other pair) rather than the full class.
- Provide a **presentation script template** with sentence starters for each section: "Our science question was... We hypothesized that... We tested this by... Our data showed... We concluded that... One thing we would change is..."
- For teams whose device is partially working: frame the presentation around **what they DID accomplish and what they learned from what didn't work** — the process and reflection are fully gradable even if the hardware outcome is incomplete.
- Allow note cards — clearly labelled with bullet points only (not full sentences) — so students focus on talking, not reading.

### Extension
- Challenge the student to write a **formal abstract** (150 words maximum) summarising their project for a hypothetical science fair submission.
- Ask the student to compare their project to a **real commercial or research product** that does something similar, and describe what engineering challenges the professionals would have faced that the student didn't (scale, reliability, power, cost).
- Invite strong students to be **"guest commentators"** during other teams' Q&A — they ask a genuine technical question. This deepens their own understanding and models engaged audience behaviour.

### Visual / Kinesthetic Accommodations
- For students who express themselves better physically: allow them to **demonstrate the device without speaking** first, then explain what the audience just saw.
- Provide a **physical presentation timer** (visible countdown clock) so speakers can pace themselves without having to look at the teacher for time signals.
- For students with reading/writing difficulties: the final reflection can be completed **verbally on a voice recording** and submitted that way — what matters is the thinking, not the handwriting.
- Seat students with hearing impairments close to the presenter. If possible, display key slides/data in large text.
