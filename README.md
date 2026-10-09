# HVAC High Stakes — Skills Trades Lab

A poker-style classroom quiz for one teacher and up to eight individual students. All students receive the same multiple-choice question and answer order on a shared 30-second clock. All currency is virtual classroom chips.

## Quick start on GitHub Pages

Suggested repository name: **hvac-high-stakes**

1. Create that repository in your SkillsTradesLab GitHub account.
2. Upload the files **inside** this package to the repository root. `index.html`, `teacher.html`, `engine.js`, `app.js`, `bank.js`, and `styles.css` must be together. Keep the `vendor` folder and its files intact. Do not upload the ZIP as the only repository file.
3. In **Settings → Pages**, select **Deploy from a branch**, branch **main**, folder **/(root)**, and save.
4. Student URL: `https://skillstradeslab.github.io/hvac-high-stakes/`
5. Teacher URL: `https://skillstradeslab.github.io/hvac-high-stakes/teacher.html`
6. On the teacher page, choose chapters, skill levels, hand limit, wager mode, and rescue allowance. Open a **Live classroom** table. Copy the student link.
7. Each student uses their own device and browser tab, enters a name or initials, and joins. Wait until all eight seats show Connected. Start the game, then deal the first question when ready.

These URLs become live only after the repository files are uploaded and Pages finishes deployment.

## Teacher overview

The teacher page shows connected seats, balances, wagers locked, answers locked, accuracy, elimination, rescue activity, shared countdown, correct answer/explanation after each hand, and final standings. Export the per-hand results and final standings as a CSV before closing the table. No student email accounts are required.

The teacher is the host and keeps the game state. Keep the teacher tab open and the computer awake. A browser refresh can be recovered using **Resume table** on the teacher page in the same browser; students reconnect with the same tab. Recovery depends on local browser storage and successful network reconnection. A new/private browser cannot recover an old seat. There is no central database or cross-device teacher recovery.

## House rules

- Starting balance: **$1,500** each.
- Default: **fixed $100 per hand**. Optional teacher setting: students choose a wager of at least $100, in $50 increments, up to their available balance.
- Wager phase is teacher-paced. Students lock chips before seeing the question. An active student who has not locked a wager automatically wagers $100 when the teacher deals, including a disconnected active seat. No folding.
- A 3-second get-ready cue precedes the question. The answer window is exactly **30 seconds**, measured by the teacher's clock. A question is displayed on each device when that common start time arrives. Network latency prevents a guarantee of pixel-perfect simultaneous delivery. Answers must reach the teacher before the deadline.
- The first accepted answer locks. No changes. The clock still runs the full 30 seconds even if all students answer early.
- Correct: stake returned plus an equal profit. Basic bonus: $0; Applied: $50; Advanced: $100. A $100 Advanced win adds $200 net to the pre-hand balance.
- Wrong or no accepted answer by the deadline: wager is lost. A disconnected player who cannot submit in time loses that hand's committed stake.
- Below **$100** means out, because the next wager cannot be funded. Eliminated students can continue watching.
- Only a voluntary student rescue restores play. The donor transfers **half of current net winnings above $1,500**, rounded down to whole dollars. This means current balance minus starting balance, after prior gains, losses, and gifts. It is not half the entire balance or half lifetime payouts.
- The transfer must restore the recipient to $100 or more and leave the donor able to wager. Rescues happen between hands. Default allowance: one rescue received per student; teacher can choose two or three before play. The donor pays the transfer; new chips are not created.
- The teacher allows a rescue window after each reveal, then selects Next hand. At the chosen hand limit, or when fewer than two students can play at Next hand, the game ends. The teacher can end it between hands. No unlimited games.
- Highest ending balance wins. Correct answers break a balance tie; an exact tie is shared. Chip ranking includes wagers and gifts and is not a grade. Use the accuracy CSV for learning review.

## Replay and question bank

**269 original practice questions** grounded in the three supplied chapters:

| Chapter | Questions | Focus |
| --- | ---: | --- |
| 1 | 45 | Vapor-compression components; applications of the 25-item supplied homework quiz |
| 5 | 64 | Loads, relays/contactors, switches, protection, transformers, diagrams |
| 8 | 160 | Magnetism, motor types, capacitors, electronic motors, terminals, diagnosis |
| **Total** | **269** | **110 Basic, 83 Applied, 76 Advanced** |

There are 199 concept/application questions and 70 numerical practice forms. Numerical forms cover motor speed, parallel/series capacitors, terminal resistance, and the chapter's capacitor substitution guide. These forms vary values and are not 70 new topics. Original textbook figures are not reproduced. Questions assess the meaning and use of symbols in text; image-based symbol recognition is not included.

The pool is chapter-balanced by rotating between shuffled chapter groups until a smaller group runs out. Answer order is shuffled once per hand and shared by all students. There are no repeated question IDs within a game. The same teacher browser prioritizes unseen questions within each selected chapter, then reuses that chapter's older questions when needed. This keeps all selected chapters in the rotation even when one chapter's fresh questions run out. Switching browsers clears that replay history. If filters leave fewer questions than the selected hand count, the actual hand count is reduced to the available pool.

`questions.json` is the editable bank; `bank.js` is its browser-readable copy. After editing JSON, regenerate bank.js with `node tools/sync-bank.cjs`. Review questions and keys with `question-bank.html` before class. Answers are public repository files, so this is a classroom activity rather than a secure examination. Do not upload the source chapter PDFs to a public repository; this package contains only the original practice questions and references.

Capacitor substitution questions explicitly use the chapter's guide; the exact specified replacement and manufacturer data take priority for real equipment service. Synchronous-speed questions distinguish calculated field speed from loaded rotor speed.

## Connection check before class

The live game uses PeerJS 1.5.4, bundled in vendor/peerjs.min.js, and PeerJS Cloud signaling. WebRTC data connections carry game messages directly between teacher and students. GitHub Pages serves the interface; it is not a live multiplayer backend. No backend account or paid database is required by this build. Public signaling service availability is outside this project's control.

Before relying on it in class, join all eight intended devices on the actual school Wi-Fi, run a hand, verify that all answers are acknowledged, and export results. School firewalls or NAT configurations can prevent peer connections. If the network does not permit them, a self-hosted signaling/TURN service or a different multiplayer backend would be needed. This package does not provision such services.

For a same-computer rehearsal, choose **Same-computer test** on the teacher and **Same computer · tab test** on students. This uses BroadcastChannel and works only across tabs/windows on the same origin and browser storage partition. Use separate fresh tabs with independent seat tokens (not duplicated logged-in game tabs). It does not connect separate devices. You can serve locally with `python3 -m http.server 8000` and visit `http://localhost:8000/teacher.html`.

## Suggested teaching flow

Run 20 hands, choose all chapters, and use fixed $100 wagers for the first session. Discuss each revealed explanation before the next hand. For a later session, use only Applied/Advanced questions or allow student-selected wagers. Eliminated students can keep a reflection note about missed questions while watching; rescue decisions remain voluntary.

## Validation

Run `node tests/engine.test.cjs`. The tests cover timed acceptance, timeout loss, fixed/optional wagers, elimination, rescue boundaries, bank integrity, deck filtering, and round endings. Multiplayer UI checks use eight browser seats and a teacher in the same-computer transport. Live school-network behavior must be checked on that network.

## Files

- `index.html`: student entrance.
- `teacher.html`: teacher overview and host controls.
- `guide.html`: readable rules and setup overview.
- `question-bank.html`: teacher review/search of questions, keys, explanations, and references.
- `engine.js`: teacher-authoritative game rules.
- `app.js`: live room transport and interface.
- `styles.css`: responsive poker-table appearance.
- `questions.json`, `bank.js`: question pool.
- `tests/engine.test.cjs`: automated rule checks.
- `tools/sync-bank.cjs`: regenerate browser bank from JSON.

PeerJS is an external MIT-licensed dependency: https://github.com/peers/peerjs. Its pinned distribution and MIT license are bundled under vendor/; external fonts are optional and local font fallbacks are supplied.
