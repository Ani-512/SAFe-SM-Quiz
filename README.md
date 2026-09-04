# SAFe® Scrum Master — Quiz

A Kahoot-style quiz app for SAFe Scrum Master training. Two ways to play:

- **🎬 Live game** — you host on a big screen, everyone scans one QR code and answers the **same questions in sync**. After each question the room sees the correct-answer stats and a live podium, then you advance everyone together.
- **📝 Self-paced practice** — six independent single-lesson quizzes people can take on their own, any time.

---

## Files

| File | What it is |
|------|------------|
| `index.html` | The hub / landing page — links to everything |
| `host-game.html` | **Live game host** (the screen you present from) |
| `play.html` | **Player pad** — what participants open on their phones |
| `lesson-1-introducing-scrum-in-safe.html` | Self-paced quiz — Lesson 1 |
| `lesson-2-characterizing-the-role-of-the-scrum-master.html` | Self-paced quiz — Lesson 2 |
| `lesson-3-experiencing-pi-planning.html` | Self-paced quiz — Lesson 3 |
| `lesson-4-facilitating-iteration-execution.html` | Self-paced quiz — Lesson 4 |
| `lesson-5-finishing-the-pi.html` | Self-paced quiz — Lesson 5 |
| `lesson-6-ai-for-scrum-masters.html` | Self-paced quiz — Lesson 6 |

> Keep all files in the **same folder** (repo root). The pages link to each other with relative paths.

Each lesson has a larger question bank than it uses, so every round draws a **random** subset — quizzes don't repeat the same questions each time.

---

## Hosting on GitHub Pages (free)

1. Create a **public** repository (e.g. `safe-quiz`).
2. **Add file → Upload files** → drag in all the `.html` files (plus this README). Keep them in the **root**, then **Commit changes**.
3. **Settings → Pages → Source:** *Deploy from a branch* → Branch **main**, folder **/ (root)** → **Save**.
4. Wait ~1–2 minutes. Your site goes live at:
   ```
   https://YOUR-USERNAME.github.io/safe-quiz/
   ```

**Your links**

- Hub: `https://YOUR-USERNAME.github.io/safe-quiz/`
- Host a live game: `.../host-game.html`
- Players join: `.../play.html` (the QR code handles this automatically)
- A single lesson: `.../lesson-1-introducing-scrum-in-safe.html`

To update later: edit a file on GitHub (pencil icon → commit) or re-upload it. Pages redeploys within ~1–2 minutes.

---

## Firebase setup (required for the LIVE game)

GitHub Pages only serves the files — it can't sync 40 phones in real time. The live game uses **Firebase Realtime Database** (free) so every player writes to their own path and simultaneous answers never collide.

**One-time setup (~5 min):**

1. Go to <https://console.firebase.google.com> → **Add project**.
2. **Build → Realtime Database → Create database → Start in TEST MODE.**
3. **Project settings** (⚙️) → **Your apps** → **Web app** (`</>`) → register the app.
4. Copy the `firebaseConfig` object it shows you (make sure it includes `databaseURL`).
5. Paste that **same config** into the marked block at the top of the `<script>` in **BOTH**:
   - `host-game.html`
   - `play.html`

   Replace the placeholder config:
   ```js
  // Import the functions you need from the SDKs you need
  import { initializeApp } from "https://www.gstatic.com/firebasejs/12.18.0/firebase-app.js";
  import { getAnalytics } from "https://www.gstatic.com/firebasejs/12.18.0/firebase-analytics.js";
  // TODO: Add SDKs for Firebase products that you want to use
  // https://firebase.google.com/docs/web/setup#available-libraries

  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  const firebaseConfig = {
    apiKey: "AIzaSyBUjsot15FuYuB2or9XklqF3H_-Bnxi9YE",
    authDomain: "safe-quiz-20238.firebaseapp.com",
    databaseURL: "https://safe-quiz-20238-default-rtdb.europe-west1.firebasedatabase.app",
    projectId: "safe-quiz-20238",
    storageBucket: "safe-quiz-20238.firebasestorage.app",
    messagingSenderId: "728130638190",
    appId: "1:728130638190:web:6f9ae15944ad6c2b6d317f",
    measurementId: "G-F63L91MNJV"
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const analytics = getAnalytics(app);
   ```

Until the config is added, the live pages show a "needs Firebase setup" note and the self-paced lessons still work on their own.

**If the live game won't connect once hosted:** in the Firebase console check **Authentication → Settings → Authorized domains** and make sure `YOUR-USERNAME.github.io` is listed. Confirm the `databaseURL` in your config matches your database exactly.

**Notes on test mode:** test-mode rules are open (anyone with the config can read/write) and typically **expire after ~30 days**. That's fine for classroom sessions with non-sensitive data — just re-open test mode (or add simple rules) when it lapses. Don't store anything sensitive.

---

## Running a live session

1. Open **`host-game.html`** on the machine connected to the projector.
2. Pick a lesson → **Create game**. A fresh **PIN + QR code** is generated (new every launch).
3. Ask the room to scan the QR (or go to `play.html` and type the PIN), enter a name — you'll see them join live.
4. Click **Start**. For each question:
   - The big screen shows the question, four colored answers, and a countdown.
   - Phones show the same answers as tappable buttons.
   - When the timer ends (or everyone's answered), the reveal shows **% correct**, a breakdown of each option, and the **live leaderboard**.
5. Click **Next** to advance everyone. After the last question, the **🥇🥈🥉 podium** shows the winners.

The game auto-cleans itself from the database when you close the host tab.

---

## Running self-paced practice

Just share a lesson link (e.g. `.../lesson-3-experiencing-pi-planning.html`). Each start page has a QR code so people can open it on their phones. These can optionally use a shared leaderboard — see the config comment inside each lesson file.

---

## Customizing questions

All questions live inside the files. For the live game they're embedded in `host-game.html` (the `LESSONS` object near the bottom); each self-paced lesson keeps its own bank in its file. Edit the text, answers, or the `correct` index there. The correct answer is identified by index, and options are shuffled automatically at play time.

---

*Built for SAFe® Scrum Master training. SAFe is a registered trademark of Scaled Agile, Inc.*
