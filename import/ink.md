# Ink

For an agent. Read [`README.md`](README.md) and [`outline-format.md`](outline-format.md) first.

## What maps to what

| In Ink | In the outline |
|---|---|
| A knot `=== name ===`, a stitch `= name` | A `Dialogue:`. Its old ID is `knot`, or `knot.stitch`. Group them into situations along the story's beats: a situation is a scene (SPEC §4.5) |
| Text | Lines. `Narrator:`, or the speaker when the writing says: `Anna: Hello` written as text, a tag `# speaker: Anna` |
| `*` once-only choices | Options that disappear: `Once used: disappears` |
| `+` sticky choices | Options that stay: `Once used: stays` |
| Choices the flow comes back to (`-> knot` back to where they are, or a gather `-` after a loop) | A `Hub:`. A plain menu it never comes back to is a `Choice:` |
| A choice's text `* Hello[, there] friend` | The menu's words before and in the brackets are the option's (*Hello, there*), the words before and after them the line said (*Hello friend*) |
| `-> DONE`, `-> END` | `Exit:` to whatever comes next, or a storyline's exit |
| A divert `-> knot` | `Exit:` out of this dialogue, and `On that exit:` beside it with `Go to:` a `Label:` on the knot's dialogue. Inside one knot: `Go to:` |
| A gather `-` | Nothing to write: what's open carries on to the next step |
| `VAR x = true`, `VAR n = 0` | `## Facts:`, yes/no or a number, old ID the variable's name |
| `~ x = true`, `~ n += 1` | `Set:` |
| `{x: …}` around **words only** | A variant of the line. A variant is a whole line, so write the whole sentence each way |
| `{x: -> a}`, `{x: … | …}` changing the flow | `If:`, with `Else:` |
| A conditional choice `* {x} Hello` | An option condition: `If:` straight under the option |
| `{shuffle: … }`, `{cycle: … }`, `{stopping: … }` **that is all someone says**, on being talked to or in passing | A pool under the character (SPEC §4.12): `Entry pool:` or `One-liner pool:`; shuffle is `(random)`, cycle `(in order, loops)`, stopping `(in order)` |
| The same **in the middle of a flow** | **There's no such thing here:** pools are only for those lists, and inside a dialogue only variants and conditions decide (SPEC §4.12). Flag it. Write its first alternative as the line, the others in the flag |
| `CONST` | A fact the flow never sets, or the plain value. `Flag:` it |
| `EXTERNAL`, functions, tunnels `->->`, threads `<-` | **Flag them**: what they do has to be written out, or be the game's: engine events and triggers |
| Tags `# …` | A comment: `Technical note:`, `VO:` |

## Flag these

- Text with a variable in it, `{name}`: it's a game value, `{player_name}`, if the game fills it in. If the
  story computes it, flag it.
- `LIST`s: yes/no facts, one per item, or phases if they're stages of something. Flag which.
- Who speaks, if the text doesn't say. Don't invent speakers: `Narrator:`, and `Flag:` it.

## A small piece

```text
VAR met_anna = false

=== gate ===
Who goes there?
* [Say your name]
    ~ met_anna = true
    Ah, it's you.
    -> inn
+ [Stay quiet]
    Well?
    -> gate

=== inn ===
{shuffle:
- Quiet tonight.
- Busy tonight.
}
-> END
```

The shuffle here is what the innkeeper says when you come in. It isn't in a conversation's flow, so it's her entry pool.

```outline
# At the gate
Source: Ink
Flag: Who speaks isn't in the Ink: the lines at the gate are given to Anna.
Not mapped [inn]: all it holds is the shuffle, now Anna's entry pool.

## Facts: Story
- [met_anna] met_anna

## Character: Anna
- Entry pool: At the inn (random)
  - Quiet tonight.
  - Busy tonight.

## Situation: Gate
- [gate] Dialogue: Gate
  - Label: asking
  - Anna: Who goes there?
  - Hub:
    - Remembers: this conversation
    - Say your name
      - Once used: disappears
      - Player: Say your name.
      - Set: met_anna = true
      - Anna: Ah, it's you.
      - Break hub
    - Stay quiet
      - Once used: stays
      - Anna: Well?
  - Exit: to the inn

## Storyline: At the gate
- Place: Gate
- On to the inn:
  - Exit: at the inn (finished)
```
