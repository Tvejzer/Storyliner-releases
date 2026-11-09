# Miro boards, documents, loose notes

For an agent. Read [`README.md`](README.md) and [`outline-format.md`](outline-format.md) first. `Sources/Plot
outline.md` and what became of it (`fixtures/README.md`, `fixtures/Tallon-outline.md`) are a whole example.

There's no tool to map from. **You read, and write an outline; the designer checks it.** So this guide is mostly
about what not to invent.

## How to read them

| In the notes | In the outline |
|---|---|
| A sticky, a heading, a paragraph of events | A situation, when it's a scene or a beat of gameplay (SPEC §4.5). Keep the notes' own names |
| An arrow, “then”, “if you do X” | The arrows: steps one after another, `Choice:`, `If:`, `On exit:` |
| A list of things the player can pick | `Choice:`, or `Hub:` if they can come back and pick another |
| Quoted speech with a name | A line with that speaker |
| Text that tells what happens | `Narrator:`, word for word, the way the Tallon fixture does: it's what a playable prototype can show until someone writes the scene |
| “FACT”, “you learn that…”, “remember that…” | A fact, its sentence as the display name: `- knows_duke_negotiates — You learn that the Duke is negotiating` |
| Something in a character list | `## Character:`, the role as a field or in `Description:` |
| A big story with parts that unlock each other | Several storylines, joined by `Available when:`; `Projects/README.md` shows why Tallon became six |
| A note that isn't story: the premise, the theme, “I'm thinking about…” | `Description:`, or `Note:`, on what it's about |
| A date, a colour, who wrote a sticky, anything about the board itself | Leave it, with a `Not mapped:` if anyone could want it |

## What not to invent

Notes have holes. **They stay holes**: finding them is what the validator is for, and an invented line hides one.

- **A branch nobody wrote:** `Stop: The outline stops here.`
- **A line nobody wrote:** `Narrator: TODO: who you talk to at the celebration, and what they say.`, with the
  `TODO:` under it. Never write dialogue the notes don't have, unless the designer asked you to draft it; then
  say so with a `Flag:` on each.
- **A fork that depends on something not written:** `If undecided: how the elves' conversation goes`, both ways
  kept. It counts as false, so play takes the written path.
- **A fact you need that the notes don't mark:** make it, and `Flag:` it.
- **Two notes that disagree:** follow the later or the more detailed, and `Flag:` the other.

## Counting

No reader can count notes for you, so say what you counted, for the report: `Came in: 40 lines, 8 choices, 3
facts, 25 stickies`. Then go through what you left out, and give each a `Not mapped:`. The designer's check is the
real one here: **ask them to read the outline** before you import it. It's meant to be read.

## A small piece

From a board: *Customs — your weapon is confiscated and your papers aren't good enough. FIGHT: combat, you get
in but the guards want you (FACT: wanted). CONTACT: dialogue + cutscene, not written yet.*

```outline
# Rebellion in Tallon
Source: the Miro board Quests
Came in: 2 lines, 1 choice, 1 fact, 1 sticky

## Facts: Facts
- player_is_wanted — Player is wanted

## Situation: Customs
Description: Customs — the 1st obstacle.

- Dialogue: Customs
  - Narrator: Your weapon is confiscated, and your documents aren't good enough for border control to let you in.
  - Choice:
    - Fight your way through
      - Narrator: A combat sequence. You run through the customs and get into the city — but the guards are looking for you.
      - Set: player_is_wanted = true
      - Exit: into the city
    - Find the Secret Service contact
      - Narrator: A dialogue, then a cutscene. You get into the city.
        - TODO: The dialogue with the contact and the cutscene are not written.
      - Exit: into the city

## Storyline: Rebellion in Tallon
- Place: Customs
- On into the city:
  - Exit: act 2 (finished)
    - Flag: The sticky ends here. Act 2 isn't written.
```
