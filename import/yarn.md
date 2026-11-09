# Yarn Spinner

For an agent. Read [`README.md`](README.md) and [`outline-format.md`](outline-format.md) first.

## What maps to what

| In Yarn | In the outline |
|---|---|
| A node (`title:` … `---` … `===`) | A `Dialogue:`, its title as the old ID. Group them into situations along the story's beats: a situation is a scene (SPEC §4.5) |
| `Name: line` | A line, with that speaker. Declare each with `## Character: Name` |
| A line with no name | `Narrator:` |
| `-> Option` | An option of a `Choice:`, its words the option's. Options with text indented under them: that's the option's flow |
| `<<jump Node>>` | `Exit:` out of this dialogue, and `On that exit:` beside it with `Go to:` a `Label:` on that node's dialogue |
| A node that jumps back to itself after a choice | A `Hub:`: options that come back |
| `<<declare $x = true>>`, `<<set $x to true>>` | `## Facts:`, old ID `$x`, and `Set: x = true` |
| `<<if $x>>` … `<<else>>` … `<<endif>>` around **a line only** | A variant of the line. A variant is a whole line, so write the whole sentence each way |
| The same around **more** | `If:`, with `Else:` |
| `-> Option <<if $x>>` | An option condition: `If:` straight under the option |
| `<<once>>` on an option | `Once used: disappears`, on a hub's option |
| `<<wait 2>>` | `Timing: 2` on the line before |
| `<<command …>>` | An engine event: `Event: command what="…"`, declared under `## Engine events`. It's the game's to run. Check with the designer what it does, and `Flag:` it |
| `{$name}` in text | A game value, `{player_name}`, declared under `## Game values`, if the game fills it in |
| Line tags `#line:abc123` | The line's old ID: `- [abc123] Anna: Hello.`, so recordings keyed to it can be found |
| Other tags `#happy`, `// a comment` | `VO:`, or `Note:` |

## Flag these

- `<<detour>>`, smart variables, node groups, `when:`, and saliency: they decide what plays at runtime in ways
  that have no one match here. Write what the story means, and `Flag:` it.
- A node nothing jumps to: it's probably started by the game. What starts it isn't in the Yarn. It's a storyline
  entry behind a `Wait:`, or a character's to say when talked to; `Flag:` which.
- Who a `Narrator:` line really belongs to, if you can tell but the Yarn doesn't say.

## A small piece

```text
title: Gate
---
Anna: Who goes there? #line:a1
-> Say your name
    <<set $met_anna to true>>
    Anna: Ah, it's you. #line:a2
    <<jump Inn>>
-> Stay quiet
    Anna: Well? #line:a3
    <<jump Gate>>
===

title: Inn
---
Quiet tonight. #line:b1
===
```

```outline
# At the gate
Source: Yarn Spinner
Flag: Gate jumps back to itself after “Stay quiet”: written here as a loop, so it asks again.

## Facts: Story
- [$met_anna] met_anna

## Character: Anna

## Situation: At the gate
- [Gate] Dialogue: Gate
  - Label: asking
  - [a1] Anna: Who goes there?
  - Choice:
    - Say your name
      - Set: met_anna = true
      - [a2] Anna: Ah, it's you.
      - Exit: to Inn
    - Stay quiet
      - [a3] Anna: Well?
      - Go to: asking
- On to Inn:
  - [Inn] Dialogue: Inn
    - [b1] Narrator: Quiet tonight.
    - Exit: done

## Storyline: At the gate
- Place: At the gate
- On done:
  - Exit: at the inn (finished)
```
