# articy:draft, its JSON export

For an agent. Read [`README.md`](README.md) and [`outline-format.md`](outline-format.md) first. A whole
conversion done this way: `fixtures/The Fallen Leaves.outline.md`, from `Sources/Articy export json/`.

## What's in an export

| File | Holds |
|---|---|
| `package_*_objects.json` | `Objects`: everything in the flow and the entities. Each has a `Type` and `Properties`, and `Template` when it has one |
| `package_*_localization.json` | The texts: `Properties.Text` is a key into this, and the words are at `[key][""].Text`. So are `DisplayName`, `MenuText` |
| `global_variables.json` | `GlobalVariables`: the variable sets (`Namespace`), each with its `Variables` |
| `object_definitions.json` | What each `Type` is: a template's type (`German`) has `Class: DialogueFragment` |
| `hierarchy.json` | The project tree |
| `Assets/` | Images. An `Asset` object's `AssetRef` is its path |

What matters on a flow object: `Id`, `TechnicalName`, `Parent` (what it's inside), `Speaker` (an entity's ID),
`Text`, `MenuText`, `StageDirections`, and the pins: `OutputPins[].Connections[].Target` is where the arrows
go, and `InputPins[].Connections[].Target`, on a flow fragment, is its first thing inside. **A pin's `Text`
is its script**: a condition on an input pin, an instruction on an output pin. A `Jump` has `Target`.

Use the object's `Id` as its old ID, and keep the technical name beside it, as that's how people search in
articy: `- [0x0100000000000801 | technicalName=DFr_48958CB9] Zorka: Hello.`

## What maps to what

| In articy | In the outline |
|---|---|
| A global variable set | `## Facts: its name`, one fact per variable, old ID `Namespace.Variable` |
| A Boolean variable | `Name: yes/no = no` |
| An Integer variable | `Name: whole number = 0` |
| A String variable | **Not a fact**: facts are yes/no or numbers. If the game watches it, an engine event with a text argument. If it holds one of a few values, a number or a yes/no fact per value. `Flag:` which you did |
| A dialogue fragment (any type whose `Class` is `DialogueFragment`) | A line: `Speaker: its Text`, word for word |
| Its stage directions | `VO:` under the line |
| A fragment whose arrows go to fragments with menu texts | A `Choice:`, each `MenuText` an option's words, the fragment itself its line. **A single reply with a menu text is a Choice of one**, so the menu text isn't lost |
| A hub | `Hub:`. The option's words are the `MenuText` of the fragment it leads to |
| A jump | `Go to:`, with the jump's ID as its old ID, and a `Label:` on where it lands |
| A fragment with several arrows in | `Label:` on it, and `Go to:` from all but the one written first |
| A condition (object, or a script on an input pin) | `If:`, its script written in Storyliner's words: `ObjectVariables.HaveKey == true` is `HaveKey` |
| An instruction (object, or a script on an output pin) | `Set:`, the same way |
| A flow fragment holding a story beat | A situation: `## Situation:` with `Old ID:` |
| A flow fragment holding the chapter or quest | A storyline: `## Storyline:` |
| A flow fragment inside a situation | A situation placed inside it, when it's a scene of its own; else a `Dialogue:` of its canvas, with the fragment's ID as its old ID |
| A run of fragments between flow fragments | A `Dialogue:`, named for what happens in it |
| An entity | `## Character:`, its `TechnicalName` in a `Technical note:`, its `Id` as `Old ID:` |
| A template (an entity's, a dialogue's, a fragment's) | `## Template:` with `For:` the kind it's on and `Groups:`; each of its features a `## Field group:`, each property a field of the type it holds — a localizable string is player-facing text only if the player reads it. The object says `Template: name` |
| A template's property values | `Field name: value` on the entity, storyline or situation, or as a bullet under the line or dialogue — the words, not the numbers |
| An enumeration property | A `choice (…)` field, its choices the enumeration's words: `object_definitions.json` has them. A value is its word |
| An entity type of the project's own (items, creatures) | `## Kind: Item`, then `## Item: name` for each |
| A preview image | `Resource: portraits/Name.png from Assets/Images/…` and `Field Portrait: portraits/Name.png` |

## What's usually missing: flag it

- **Where the game takes over.** articy leaves gaps in a flow, where the player does something: an objective,
  or fragments nothing connects. That's a `Wait:` for a trigger. **The export doesn't name the trigger, so you
  do, and flag it**; what the fragment says goes in a comment on the `Wait:`. A *Quest* or objective fragment
  is the journal's: its quest becomes a `## Journal quest:` — made by hand, or `Follows:` the storyline it
  belongs to — its objectives `- Objective:` bullets, and the fragment a `Wait:` followed by the
  `Journal objective:` or `Journal entry:` it stands for. Its old ID goes on the `Wait:`. (*The Fallen Leaves*'
  outline, written before 4c, keeps them as `Journal:` comments, in a comment category of that name: that
  still works.)
- **Fragments with no arrow in.** What starts them isn't in the export. Put them where the canvas's order says,
  behind a `Wait:`, and `Flag:` it.
- **Spoken language.** A template or colour saying which language a line is in: `VO: Spoken in German.` on the
  dialogue, and on the lines that differ, until spoken languages come (stage 5). Keep each line's beside its
  old ID: `[0x01 | spokenIn=German]`.
- **Variables nothing uses**, and text nobody reaches: `Flag:` them. Don't wire them in.
- **Typos.** One `Flag:` listing them. The words stay.
- **`Asset`, `UserFolder` objects, an empty variable set:** nothing to bring. An empty set gets a `Not mapped:`.

## A small piece

From the export (cut down):

```json
{ "Type": "German", "Properties": { "Id": "0x0100000000000D12", "TechnicalName": "DFr_A1", "Speaker": "0x0100000000000244", "Text": "DFr_A1.Text",
  "OutputPins": [{ "Connections": [{ "Target": "0x0100000000000D1E" }, { "Target": "0x0100000000000D30" }] }] } }
```

with its text *“Do you have the syringe?”*, and two replies with the menu texts *Not yet.* and *Yes, here it is.*

```outline
# The syringe
Source: articy:draft JSON export 2.1
Comment category: Journal
Flag: The trigger back_with_morphine is named here; the export doesn't name it.

## Triggers
- back_with_morphine — Zorka talks to Doctor Fechner about the syringe

## Character: Doctor Friedrich Fechner
Code name: FriedrichFechner
Old ID: 0x0100000000000244
Technical note: articy: Ntt_5DB60977

## Character: Zorka
Old ID: 0x01000000000001F3

## Situation: Day 2 Doctor Fechner
Old ID: 0x0100000000000B2D
Present: FriedrichFechner, Zorka

- Label: bring
- [0x0100000000000D0B] Wait: back_with_morphine
  - Journal: Bring the morphine syringe to Mr. Szakasits.
- Dialogue: The syringe
  - VO: Spoken in German.
  - [0x0100000000000D12 | technicalName=DFr_A1 | spokenIn=German] FriedrichFechner: Do you have the syringe?
  - Choice:
    - Not yet.
      - [0x0100000000000D1E | technicalName=DFr_A2 | spokenIn=German] Zorka: Not yet, I'm sorry.
      - Exit: not yet
    - Yes, here it is.
      - [0x0100000000000D30 | technicalName=DFr_A3 | spokenIn=German] Zorka: Yes, here it is.
      - Exit: done
- On not yet:
  - Go to: bring
- On done:
  - Exit: to be continued

## Storyline: Chapter One
Code name: ChapterOne
Old ID: 0x01000000000006CD

- Place: Day 2 Doctor Fechner
- Exit: to be continued (finished)
  - TODO: The chapter isn't finished in the export.
```
