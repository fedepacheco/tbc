---
name: the-office-rpg
description: |
  Stress-tests a message, doc or deck before you send it by predicting how up
  to 3 stakeholders will react, playing it out as a short RPG, and ending with
  a scorecard of specific edits. Use when the user says "play the office RPG",
  "stress-test this before I send it", "how will [people] react to this",
  "war-game this update", or "red-team my email". Also runs a sample game with
  fictional characters for "try the office RPG", "demo mode" or "guide me".
  Do NOT use to simply draft a message, to evaluate anyone's performance, or
  to profile people by any protected characteristic.
license: MIT
compatibility: Built for Microsoft 365 Copilot Cowork (reads people, mail, chat and calendar context). The sample game with the demo cast works in any Agent Skills host.
metadata:
  category: communication
  icon: PeopleTeam
  version: "3.4"
  author: Fede Pacheco
---

# 🎮 The Office RPG

Rehearse how people will react before you hit send. Three steps:
**1 Setup → 2 Play → 3 Scorecard.**

**The #1 rule: less text, more clicking.** Every screen is short enough to
read in 5 seconds, and every choice is a button. If a screen needs more than
2 sentences, cut it.

## When to Use
- "Play the Office RPG" / "try the office RPG" / "demo mode" / "guide me"
- "Stress-test / red-team / war-game this before I send it"
- "How will [person] / [my VP and the partner lead] react to this?"

## When NOT to Use
- Just writing or polishing a message: use a writing skill instead
- Judging or ranking anyone's performance or competence: decline, point to manager/HR
- Profiling people by any protected characteristic: decline
- Building a deck or document from scratch: use the document skills

## Display rules (MANDATORY)
Cowork shows the user only **the question card** (`AskUserQuestion`) and **the
final message** of a turn. Text written between tool calls is hidden in the
collapsed work log. So:
1. **Every choice is a card with buttons.** Never ask the user to type a keyword.
2. **Everything the user must read during play is inside the card.** No chat text before a card.
3. **Card question: max ~35 words, one paragraph, one character.**
4. **Card header (max ~12 chars):** name + meter, e.g. `Priya ▰▰▱▱▱`. Meter is plain text, never colored emoji squares.
5. **Max 3 options per card.** Label = emoji + 1-2 words. Description = max ~12 words.
6. **Only the Scorecard is a chat message**, written last, then the turn ends.

## Step 1 — Setup (ONE card, every game)
Every game starts with ONE setup card. It always holds the **Turns** and **Crowd**
questions, plus only the questions still open. One card with up to 3 short
questions, never a chain of cards.

| Question | header | Ask it when | Options (max 3) |
|---|---|---|---|
| How do you want to start? | `Start` | No content yet ("try the office RPG", "demo", "guide me") | `🎬 Try a sample`: "Budget ask, fictional cast. Reads nothing." · `📎 Use my own`: "Your email or file. Nothing gets sent." |
| 🎯 Goal: [≤10 words]. Who should react? (multiSelect) | `Players` | Content given, players NOT clear (see **Picking players**) | Up to 3 suggested people: emoji + first name, description `role · why` (≤8 words). Add `🎬 Demo cast`: "Priya, Marc, Michael stand in." only if fewer than 3 people were found. |
| How many turns each? | `Turns` | **Always**, unless the user already said a mode | `⚡ Quick`: "1 turn each (recommended)" · `🎯 Standard`: "2 turns, 2 weeks of context" · `🔬 Deep`: "3 turns + a recent meeting" |
| How tough is the crowd? | `Crowd` | **Always**, unless the user already said a difficulty | `😊 Friendly`: "Gentle pushback" · `😐 Realistic`: "How they'd really react (recommended)" · `😤 Tough`: "Every weak spot gets hit" |

**Skip the whole card** only when nothing is open: content, players, mode and difficulty are all given, or the user says "just go" (then Quick + Realistic).

**First card of a fresh game** (no content): question 1 is `Start`; its question text is `🎮 Get feedback before you send: 1 Setup · 2 Play · 3 Scorecard. How do you want to start?` Then Turns and Crowd in the same card.
- `🎬 Try a sample` → play the budget ask from [references/demo-scenarios.md](references/demo-scenarios.md) with Priya, Marc and Michael, using the chosen Turns and Crowd. Tips ON. Go to Step 2.
- `📎 Use my own` → keep the chosen Turns and Crowd, write ONE short final message and end the turn: `Paste it, attach a file, or name an email (like "my last email to Priya").` When the content arrives, ask only `Players` if still open; otherwise go straight to play.

**Content given** (pasted, attached, or a named email found with ONE `ExecuteSql` over `messages`): infer the goal, then pick players.

### Picking players
**Players are clear → don't ask `Players`.** Clear means either:
- the user named them ("with Priya, Marc and Michael"), or
- the content is an email with 1-3 people on To (drop the user).
Put the goal and the cast in the Turns question instead: `🎯 Goal: [≤10 words] · 🎭 Playing: Priya, Marc. How many turns each?` The user can say "swap Marc for Nina" at any time.

**Players aren't clear → suggest them from work context, then ask `Players`.** Stop at the first step that finds 3 people:
1. Names in the content itself (and Cc on an email), resolved with `SearchPeople`.
2. Work context: ONE `SearchM365` (email, teams, meetings) on the content's topic/title keywords from the last 30 days. Take the people the user most often exchanged with on that topic. Resolve with `SearchPeople`.
3. The user's manager (`GetMyDetails`) as the likely approver.
4. Still short → fill with `🎬 Demo cast`.
Each suggestion's description says why: `VP · approves budgets like this`, `on 4 threads about this`, `your manager`. Never guess an email.

If the goal is unclear, say so in the first question (`🎯 The ask isn't clear yet, so readers won't get it either.`) and flag it again on the Scorecard.

Then run recon for the chosen mode and go to Step 2.

**What the choices do:**
| Mode | Turns each | Recon |
|---|---|---|
| ⚡ Quick (default) | 1 | Role + up to 5 recent messages |
| 🎯 Standard | 2 | Last 2 weeks of mail, chats, meetings |
| 🔬 Deep | 3 | Standard + 1 relevant meeting transcript |

Crowd changes tone and how hard objections push, never the evidence. The user can still say a mode or crowd mid-game ("tough crowd"); apply it from the next card.

## Step 2 — Play (one card per character)
For each character, ONE card:
- header: `Priya ▰▰▱▱▱`
- question: `🎭 Priya (predicted): "What do you need from me, and what does $40K buy us?"`
  - 1-2 sentences, max ~25 words, objection first. Nothing else in the question.
  - Sample game, FIRST play card only: add `💡 Reframe usually wins.`
- options, exactly 3:
  - `🤝 Concede` / `🛡️ Hold firm` / `🔄 Reframe`, each description = what the user would say, ≤12 words, in quotes.
- The user can also type their own reply in the card.

Next character = a fresh card. Never mention the previous character. In Standard/Deep, the same character's next turn opens with their reaction (`"Clearer. But…"`) and the meter moves.

## Step 3 — Scorecard (final message, then END the turn)
Keep it to what fits on one screen:

```
## 🏆 Scorecard
| Player | Buy-in | Your move | Why |
|---|---|---|---|
| 🚀 Priya | ▰▰▱▱▱ → ▰▰▰▰▱ | 🔄 Reframe | A clear ask + a number is her language |

**🛠️ Top fixes**
1. Line 1: "wanted to share some thoughts" → "Need your OK on $40K by Fri" (Priya: no clear ask)
2. …
3. …

Want me to apply these? Nothing gets sent.
```
- "Why" ≤10 words. Max 3 fixes, each quoting the line to change and naming who it fixes.
- Real people: add one line under the table, `🔍 Based on: [1-2 sources] · confidence [high/medium/low]`. Sample: `🎬 Fictional characters.`
- If "apply" is accepted: Outlook draft or tracked-changes copy only.

## Recon (real people only — skip entirely for the sample)
Stop as soon as you have enough signal. Do not repeat searches.
1. Resolve each person with `SearchPeople`. Never guess an email.
2. In one `core-RunScript` for all players, returning only a short evidence table:
   - Mail: `ExecuteSql` over `messages WHERE sender = '<address>'` with a `receivedDateTime` range; read `bodyPreview` only.
   - Chats: `SearchM365` (teams) for the person + topic.
   - Meetings (Standard+): `ExecuteSql` over `calendarView`.
   - Transcript (Deep only, 1 meeting): `ListMeetingTranscripts` then `GetMeetingTranscript`.
3. Tag what they care about with evidence + confidence (high / medium / low).

## Demo cast
The sample, "demo / fake people", naming Priya, Marc or Michael, or `🎬 Demo cast` uses [references/demo-personas.md](references/demo-personas.md): 🚀 Priya (approver), 🧭 Marc (carries the work), 📎 Michael (wildcard). Always these 3, never others. No people, mail, calendar or search calls.

## Error Handling (when things go wrong)
| Situation | Do this |
|---|---|
| Name matches several people | Put the matches as options in the Players card. |
| Person external / not in directory / thin signal | Play from role + content, confidence low; say so on the Scorecard. |
| More than 3 people (e.g. a big To line) | Not clear: suggest the top 3 via **Picking players**. |
| File won't open | Say which file, ask the user to paste the text. |
| User closes a card or says "skip" | Jump to the Scorecard with what you have. |
| User asks what X "really thinks" of them, or about performance | Decline briefly; offer to keep playing the work reaction. |

## Guardrails
- **Professional reactions only**: budget, risk, scope, timeline, credibility. Never personal, medical or character judgments.
- **Predicted, not real**: label every simulated line as predicted.
- **No performance evaluation** and **no profiling by protected characteristics**.
- **Ground every claim**; never fabricate evidence, quotes or profiles. If signal is thin, say so.
- **Privacy**: use people's data only to predict their reaction to this content; never surface unrelated details.
- **Never send anything.**

See [references/example-run.md](references/example-run.md) for the exact screens. The visual guide is [assets/how-it-works.png](assets/how-it-works.png) (for READMEs and sharing; never load or show it during play).
