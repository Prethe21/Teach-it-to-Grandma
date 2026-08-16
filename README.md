<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6C47FF,100:FF6B9D&height=220&section=header&text=Teach%20It%20To%20Grandma&fontSize=46&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Explain%20it%20out%20loud.%20If%20Grandma%20gets%20it%2C%20you%20understood%20it.&descAlignY=58&descSize=17&descColor=F5F0FF" width="100%" alt="Teach It To Grandma" />

[![Live demo](https://img.shields.io/badge/▶_LIVE_DEMO-titanom--hackathon--8xts.vercel.app-6C47FF?style=for-the-badge&logo=vercel&logoColor=white)](https://titanom-hackathon-8xts.vercel.app/)

[![ElevenLabs Sonderpreis](https://img.shields.io/badge/🏆_ElevenLabs_Sonderpreis-Best_Project_Built_With_ElevenLabs-FF6B9D?style=for-the-badge)](https://hack.titanom.com/)

*3 Monate ElevenLabs Scale · für das beste Projekt, das ElevenLabs nutzt*

![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8-646CFF?style=flat-square&logo=vite&logoColor=white)
![Express](https://img.shields.io/badge/Express-5-000000?style=flat-square&logo=express&logoColor=white)
![ElevenLabs](https://img.shields.io/badge/ElevenLabs-Conversational_AI-1a1a2e?style=flat-square)
![Upstash](https://img.shields.io/badge/Upstash-Redis-00E9A3?style=flat-square&logo=redis&logoColor=white)
![Vercel](https://img.shields.io/badge/Deployed_on-Vercel-000000?style=flat-square&logo=vercel&logoColor=white)

</div>

<br/>

## Table of contents

- [What is this](#what-is-this)
- [See it in 30 seconds](#see-it-in-30-seconds)
- [Meet the six learners](#meet-the-six-learners)
- [Under the hood](#under-the-hood)
- [One lesson, start to finish](#one-lesson-start-to-finish)
- [Everything it can do](#everything-it-can-do)
- [Tech stack](#tech-stack)
- [Running it locally](#running-it-locally)
- [Deploying](#deploying)
- [Project structure](#project-structure)
- [The team and the hackathon](#the-team-and-the-hackathon)

<br/>

## What is this

Everyone knows the Feynman technique: if you can't explain something simply, you don't
really understand it. Every app that claims to teach it just asks you to *type* an
explanation and pattern-matches it against a rubric.

**Teach It To Grandma makes you say it out loud, to someone who can push back.**

Pick any topic. An AI grandmother — voice, personality, and all — listens live and asks
the questions a real beginner would ask: *"but what does that word mean, darling?"*, *"how
did you get from that to this?"*. Switch on Misconception Ambush mode and she'll
occasionally state something confidently wrong, just to see if you catch it. When you're
done, a separate grading pass reads the whole
conversation and judges whether each point was **genuinely explained**, not just
name-dropped — the difference between a checklist lighting up green and someone actually
learning something.

> The moment this app is built around: the keyword bar reads 4 / 4, and the AI says she
> didn't follow a word of it. That gap is the entire product.

<br/>

## See it in 30 seconds

```mermaid
flowchart LR
    A["📝 Say any topic"] --> B["🧠 AI writes the lesson<br/>(points · keywords · misconceptions)"]
    B --> C["🎙️ Explain it out loud<br/>to your chosen learner"]
    C --> D{"Do they<br/>push back?"}
    D -- "jargon, gaps, a planted lie" --> C
    D -- "they're convinced" --> E["📊 AI grades what was<br/>ACTUALLY explained"]
    E --> F["🏆 Score · XP · notes<br/>· Teach-Off leaderboard"]

    style A fill:#6C47FF,color:#fff,stroke:none
    style B fill:#8B5CF6,color:#fff,stroke:none
    style C fill:#A855F7,color:#fff,stroke:none
    style D fill:#1a1a2e,color:#fff,stroke:none
    style E fill:#EC4899,color:#fff,stroke:none
    style F fill:#FF6B9D,color:#fff,stroke:none
```

<br/>

## Meet the six learners

One ElevenLabs agent, six personas — each with its own voice, vocabulary, question
style, and **grading strictness**. The Manager wants the bottom line; the Professor
quotes your own contradictions back at you. All six share one unbreakable rule: they are
the learner, never the teacher. They will never define a term for you.

<div align="center">

| | | | | | |
|:---:|:---:|:---:|:---:|:---:|:---:|
| <img src="frontend/public/peep-grandma.png" width="90"/> | <img src="frontend/public/peep-child.png" width="90"/> | <img src="frontend/public/peep-student.png" width="90"/> | <img src="frontend/public/peep-manager.png" width="90"/> | <img src="frontend/public/peep-expert.png" width="90"/> | <img src="frontend/public/peep-professor.png" width="90"/> |
| **Grandma** | **Mia**, 7 | **Sam** | **Marcus** | **Victor** | **Prof. Ellis** |
| Knows nothing.<br/>Loves you anyway. | Asks *why*.<br/>Then asks why again. | Knows the words,<br/>not the how. | Wants the<br/>bottom line. | Knows the field<br/>next door. | Remembers<br/>everything you said. |
| Beginner | Beginner | Intermediate | Intermediate | Advanced | Advanced |

</div>

<br/>

## Under the hood

```mermaid
flowchart TB
    subgraph client["🖥️ Browser — React + Vite"]
        UI["Landing → Session → Recap<br/>+ Quiz mode"]
    end

    subgraph voice["🎙️ ElevenLabs"]
        Agent["Conversational AI Agent<br/>persona · voice · greeting overridden per learner"]
    end

    subgraph server["⚙️ Grading server — Express"]
        Lesson["/api/lesson"]
        Grade["/api/grade"]
        FastExtra["/api/challenge · /api/mirror<br/>/api/face"]
        DeepExtra["/api/explainback · /api/jury"]
        Multi["/api/teachoff · /api/quiz"]
    end

    subgraph brains["🧠 TitanomGPT — OpenAI-compatible"]
        Fast["gemini-3.1-flash-lite<br/>fast grading, ~1.7s"]
        Deep["claude-4.5-sonnet<br/>jury & closed-world recall"]
    end

    Redis[("🗄️ Upstash Redis<br/>Teach-Off boards · quiz state")]

    UI <-->|"live voice · WebRTC"| Agent
    UI --> Lesson --> Fast
    UI --> Grade --> Fast
    UI -.-> FastExtra --> Fast
    UI -.-> DeepExtra --> Deep
    UI --> Multi --> Fast
    Multi <--> Redis

    style client fill:#6C47FF22,stroke:#6C47FF
    style voice fill:#EC489922,stroke:#EC4899
    style server fill:#8B5CF622,stroke:#8B5CF6
    style brains fill:#1a1a2e22,stroke:#1a1a2e
```

Two things that shape every design decision here:

- **Grading never blocks the demo.** If the AI grader is slow or fails, the recap still
  renders from the live in-browser keyword check — a worse verdict, never a stuck screen.
- **The score is computed, never model-emitted.** The AI answers yes/no per point with
  reasoning; the headline number is arithmetic over those booleans, so it's reproducible
  and every input is something an engineer can point at.

<br/>

## One lesson, start to finish

```mermaid
sequenceDiagram
    actor You
    participant App as React App
    participant Learner as ElevenLabs Agent
    participant Server as Grading Server
    participant AI as TitanomGPT

    You->>App: Pick a topic + a learner
    App->>Server: POST /api/lesson
    Server->>AI: generate points, keywords, misconceptions
    AI-->>Server: lesson JSON
    Server-->>App: lesson + checklist
    App->>Learner: start voice session (persona & voice override)

    loop the lesson
        You->>Learner: explain it out loud
        Learner-->>You: asks, doubts, occasionally lies on purpose
    end

    App->>Server: POST /api/grade (full transcript)
    Server->>AI: did they explain it, or just name it?
    AI-->>Server: verdict + reasoning, per point
    Server-->>App: score, XP, Grandma's Notes
    App-->>You: Recap — the score, and exactly why
```

<br/>

## Everything it can do

<table>
<tr><td width="50%" valign="top">

**The core loop**
- Any topic, typed free-text — AI writes the lesson
- Live voice conversation, not a chat window
- Real-time keyword checklist while you talk
- AI grading of genuine understanding, not keyword-matching
- Self-correction bonus for catching your own mistakes
- Your own identity — compose a name and a face once, reused every lesson

**Six learners, one agent**
- Distinct voice, vocabulary, and grading strictness each
- Her mood and face react live as you talk — confused, curious, impressed
- Difficulty prediction: guesses which points will trip you up
  *before* you start, then scores itself against what happened

**Pressure-testing**
- Misconception Ambush *(opt-in)* — a confidently wrong claim to catch
- Topic-specific challenge cards, fired mid-conversation
- Confidence gap — predict your score, then see the truth
- Blind spots — ground you never went near, not just badly

</td><td width="50%" valign="top">

**The verdict**
- Grandma's Notes — your strongest quote, one concrete thing to fix
- She explains it *back* — only what survived, gaps and all
- Mirror mode — she retells it with planted errors, you catch them
- Delivery analysis — pace, filler, hedging, from the transcript
- One shareable recap card, composed from numbers already on screen

**Progression**
- A Feynman Score computed from real signals, never guessed
- Itemised XP with the actual quote that earned it
- Achievements that stay locked — with their condition visible —
  until they're honestly earned

**Compare & compete**
- Teach-Off — two people, the *same* generated lesson, one board
- A four-persona AI jury judging one explanation by different standards
- A live 2-player quiz mode, on the clock
- Full German-language mode — lesson, grading, and voice together

</td></tr>
</table>

<br/>

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Frontend | React 19 + Vite | `@elevenlabs/react` for the voice session, no framework overhead |
| Voice | ElevenLabs Conversational AI | One agent, six personas via per-session prompt/voice overrides |
| Grading server | Express 5 | Two models: a fast one for the live loop, a stronger one for judgement calls |
| Model access | TitanomGPT (OpenAI-compatible) | `gemini-3.1-flash-lite` for speed, `claude-4.5-sonnet` where a wrong answer needs to be subtle to be dangerous |
| Persistence | Upstash Redis | Teach-Off boards and quiz state survive serverless cold starts |
| Hosting | Vercel (×2 projects) | Zero-config detection for Vite and Express from one repo |

<br/>

## Running it locally

```bash
# terminal 1 — grading server
cd server
npm install
npm run dev        # http://localhost:3001

# terminal 2 — frontend
cd frontend
npm install
npm run dev         # http://localhost:5173
```

The grading server reads `TITANOM_API_KEY` from a project-root `.env` (gitignored —
never put it in the frontend). Point the frontend at a deployed server instead of
localhost by setting `VITE_GRADING_API` in `frontend/.env.local`.

Run `./smoke-test.sh` before a live demo to sanity-check both processes are up and the
grading endpoint responds.

<br/>

## Deploying

Two Vercel projects from this one repo — `server` and `frontend` as separate root
directories, connected to Upstash Redis for shared state. Full walkthrough, including the
CORS and health-check gotchas, is in [`DEPLOY.md`](DEPLOY.md).

<br/>

## Project structure

```
.
├── frontend/                 React + Vite client
│   ├── src/
│   │   ├── views/             Landing · Intro · Session · Recap · Quiz
│   │   ├── components/        KeyPrompt · ThemeToggle · Thinking · JargonDebt
│   │   ├── characters.js      the six personas, as data
│   │   ├── features.js        every optional feature, one flag each
│   │   └── ...
│   └── public/                 the peep character art
├── server/                   Express grading API
│   ├── index.js                every /api route
│   ├── store.js                Redis-backed Teach-Off & quiz state
│   └── tts.js
├── DEPLOY.md                 deployment walkthrough
├── TESTING.md                 test plan & smoke test notes
└── smoke-test.sh
```

<br/>

## The team and the hackathon

Built overnight at **Student Hackathon 2026 — Titanom × DeutschlandGPT**, Titanom
Headquarter, Germering, Germany. Theme: *"Eine Nacht, acht Teams, ein Thema: Bildung ×
KI"* — one night, eight teams, one theme: education × AI.

<div align="center">

### 🏆 ElevenLabs Sonderpreis
**Best Project Built With ElevenLabs**
3 Monate ElevenLabs Scale

</div>

<br/>

<div align="center">
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:6C47FF,100:FF6B9D&height=120&section=footer" width="100%" />
</div>
