# Twine, as Twee

For an agent. Read [`README.md`](README.md) and [`outline-format.md`](outline-format.md) first. Twine's own
HTML is hard to read: ask the designer to export it as Twee (Twine ▸ Build ▸ Export as Twee), or use a Twee
file. What a real conversion had to decide is in `Projects/README.md`, under *Kill the Dragon*.

## What's in a Twee file

`:: Name [tags] {position}` starts a passage, and everything up to the next is its body. `StoryTitle` and
`StoryData` are settings; a passage tagged `script` or `stylesheet` isn't story. `StoryData`'s `format` says
which macros to expect: **SugarCube**'s `<<set $x to true>>`, `<<if $x>>…<<else>>…<<endif>>`, or **Harlowe**'s
`(set: $x to true)`, `(if: $x)[…]`, `(else:)[…]`.

Use the **passage's name** as its old ID, on everything it becomes: `- [Go to the Ealdorman] Narrator: …`. A
variable's old ID is `$itsName`.

## What maps to what

| In Twine | In the outline |
|---|---|
| A passage's paragraphs | Lines, one per paragraph: `Narrator: …`, unless the text says who speaks |
| A passage with **two or more links** | A `Choice:`, the link's words the option's. They become the player's line, so nothing of the text is lost |
| A passage with **one link** | It just carries on. The link's words aren't kept: list them in one `Not mapped:` |
| A passage several links lead to | `Label:` on its first step, `Go to:` from the others |
| `[[Words\|Target]]` | An option with the words *Words*, leading to *Target* |
| `<<set $x to true>>` | `Set: x = true`, after the passage's words, since only Conditions may stand between an option and its line |
| `<<if>>` around **words only** | A variant of the line: `Variant if x: other words`. A variant is a whole line, so the two versions are each the whole paragraph |
| `<<if>>` that changes **the links** too | `If:`, with `Else:` |
| `<<if>>` around **one link** | An option condition: `If:` straight under the option |
| Variables | `## Facts:`, yes/no or a number. One quest's own go on its storyline: `Fact: name`, read as `quest.Code.name` |
| Text variables | **Not facts**: flag them. A name shown in the text is a game value: `{player_name}` |
| Your own split | Passages become **situations** along the story's beats, not one each: a situation is a scene (SPEC §4.5). Say how you split it in a `Note:` on the storyline |
| A hub passage you keep coming back to | A free-play situation with entries for arriving and for coming back (`Entries: arrived, back`), or a `Hub:` when it's a conversation |
| Endings | The storyline's exits, each `(finished)` or `(failed)`. Several endings sharing a passage: one situation, placed once per ending |

## Flag these

- **Setup passages** that ask the player about the game's state: it's a fact the game writes (`game writes`),
  with a named `## World state:` to start play mode from. `Not mapped:` the passage.
- **The story's bugs.** Twine shows no error for a misspelt variable. Compare every `$name` set with every
  `$name` read: `$HadsEyes` checked and `$HasEye` set means those options never show. Do what the story means,
  and `Flag:` each. So does `to` in an `<<if>>`, which sets instead of comparing.
- **Passages that are about the game, not the story:** *The End*, credits. `Not mapped:`.
- **Typos**, in one `Flag:`. The words stay unless the designer asks.

## A small piece

From `Sources/To Kill a Dragon.twee` (cut down):

```text
:: Go to the Ealdorman
<<set $TalkedToEaldorman to true >>
<<if $IsEaldormanandAngry is false>>The Ealdorman denies that there is any problem with the dragon.

[[Ask more about Lance]]
<<if $KnowAboutSister is true>>[[Ask about Lance's sister]]<<else>><<endif>><<else>> The Ealdorman refuses to talk with you.<<endif>>
[[Go back to the Village|VillageHub]]
```

```outline
# Kill the Dragon
Source: Twine, SugarCube
Flag: $IsEaldormanandAngry is checked but never set before: here it starts as no, so the Ealdorman talks to you.

## Situation: Talk to the Ealdorman
Present: The Ealdorman

- Dialogue: Talk to the Ealdorman
  - Set: quest.KillTheDragon.talked_to_ealdorman = true
  - If: quest.KillTheDragon.ealdorman_angry
    - [Go to the Ealdorman] Narrator: The Ealdorman refuses to talk with you.
    - Exit: back
  - [Go to the Ealdorman] Narrator: The Ealdorman denies that there is any problem with the dragon.
  - Choice:
    - Ask more about Lance
      - Exit: back
    - Ask about Lance's sister
      - If: quest.KillTheDragon.knows_about_sister
        - Exit: back
    - Go back to the Village
      - Exit: back

## Character: The Ealdorman
Code name: Ealdorman

## Storyline: Kill the Dragon
Code name: KillTheDragon
Fact [$IsEaldormanandAngry]: ealdorman_angry — The Ealdorman is angry with you
Fact [$TalkedToEaldorman]: talked_to_ealdorman — You talked to the Ealdorman
Fact [$KnowAboutSister]: knows_about_sister — You know about Lance's sister

- Place: Talk to the Ealdorman
- Exit: back (finished)
```
