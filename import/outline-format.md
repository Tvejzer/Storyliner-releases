# The outline format

A plain text file, `Name.outline.md`, written with **names instead of IDs**. Storyliner makes the IDs,
line IDs and code names, lays the canvases out and checks the result. It's strict on purpose: a mistake is
never guessed at. Every mistake comes back with its line number, and nothing is made until they're fixed.

```outline
# Getting into the inn
Source: my notes
Description: A small example.

## Facts: Things
- has_key — The player has the key

## Character: Anna
Description: The innkeeper.

## Situation: At the gate
Present: Anna

- Dialogue: Greeting
  - Anna: Who goes there?
    - Variant if has_key: Oh, it's you.
  - Choice:
    - Show the key
      - If: has_key
        - Player: Here, the key.
        - Set: has_key = false
        - Exit: inside
    - Turn back
      - Exit: away

## Storyline: Getting in
- Place: At the gate
- On inside:
  - Exit: got in (finished)
- On away:
  - Exit: walked off (failed)
```

## How it's laid out

- `# Name` — the first line: the project's name.
- `## Kind: name` — a block. Order doesn't matter: a block may name things declared further down.
- `Key: value` — a setting of the project (before the first `##`) or of a block.
- `- something` — a bullet: a step, or an item of a list. **Two spaces deeper** is inside the one above.
- An indented line with no bullet carries on the text above it, on a new line.
- A line starting with `>` is a note to whoever reads the outline. Nobody else sees it.
- **`[old ID]`** at the start of a bullet keeps the other tool's ID beside the new one in the mapping file:
  `- [0x0100000000000881] Zorka: Hello.` Anything else to keep goes after a `|`:
  `- [0x01 | technicalName=DFr_8 | spokenIn=German] Zorka: Hello.` A block takes `Old ID: …` instead, and a
  key has its old ID before the colon: `Fact [$Angry]: angry`, `Not mapped [Setup]: …`. One ID may be on
  several things: a passage that became three lines.

Names find things by their display name or code name, exact first, then ignoring case.

## The project

| Key | Means |
|---|---|
| `Source:` | Where this came from, for the report |
| `Came in:` | What you counted, when no reader can: `40 lines, 8 choices, 3 variables, 12 stickies`. `--source` counts instead |
| `Comment category:` | A category of comment of the project's own: `Comment category: Journal`. Then `Journal: …` is a comment |
| `Source language:` | `de`; `en` unless given |
| `Menu text on options:` | `yes`: an option's line may have `Menu:` (SPEC §4.10). `no` unless given |
| `Player starts in:` | A place |
| `Resource:` | `Resource: portraits/Zorka.png from Assets/Images/Zorka.png`: the command copies it into `resources/`, from the source's folder, else from beside the outline. The editor can't: there, the report lists it |

## Comments, anywhere

On the project, on a block, or as a bullet under what they're about:

| Write | Becomes |
|---|---|
| `Description: …` | Its description |
| `Note: …` | A design note |
| `Technical note: …` | A technical note |
| `VO: …` | A VO direction |
| `TODO: …` | A TODO, and a row under *Needs your hand* |
| `Flag: …` | The same, and it's how you tell the designer you decided something |
| `Not mapped: …` | No comment: a row under *Couldn't be mapped*. `- [old ID] Not mapped: …`, or `Not mapped [old ID]: …` as a key, accounts for that ID |
| `Translator note: …` | A note for translators (SPEC §4.23, §12) — on a text table's row, a codex page, an objective |
| `Journal: …` | A comment in a category you declared |

Under a step that holds steps — a dialogue, an `If:` — a comment bullet belongs to that step.

## Declarations

```outline
# Declarations
Comment category: Journal

## Facts: Things
> A fact list. Each bullet: code_name: type = start, game writes — Display name; all but the code name optional.
- has_key — The player has the key
- gold: whole number = 15 — Gold in the purse
- courage: decimal number = 0.5
- is_raining: yes/no = yes, game writes
  - Technical note: The weather system writes this.

## Character fields
> Also: Location fields, Faction fields. A field's type: text (unless given), number, yes/no, image.
- Role
- Age: number
- Portrait: image

## Character: Golrik Londerbau
Code name: Golrik
Type: main
Field Role: Duke
Field Age: 54
Starts in: The Keep
Found in: The Keep, Tallon
Old ID: Ntt_C1486E9B
Description: Wants to be governor of the Frontier.

## Character: Guard
Type: generic
- Entry pool: Everyday (in order, loops)
  - Move along.
  - Quiet day.
- Entry pool: Dragon
  - If: gold > 100
  - It's the end of us.
  - Did you see the smoke?
- One-liner pool: Weather
  - Looks like rain.

## Location: Tallon

## Location: The Keep
Inside: Tallon
Available: no

## Faction: Rebels
Start reputation: 10

## Phases: Day
> A track of phases. Owner: game (unless given), a storyline or a situation.
Owner: game
- day_1 — Day 1
  - Color: #3f78c4
- day_2 — Day 2

## Triggers
- door_opened — The door opens
  - Technical note: The door's script fires this.

## Engine events
> Argument types: text, yes/no, whole, decimal, character, location.
- PlaySound(what: text) — Play a sound
- SpawnAt(who: character, where: location)

## Game values
- player_name: text = Zorka — The player's name

## World state: Rainy morning
> A named world state, to start play mode from.
- is_raining = true
- gold = 3
- Phase: day_2
- Quest: The errand = taken

## Situation: Errand
- Dialogue: Errand
  - Narrator: You have an errand.
  - Exit: done

## Storyline: The errand
- Place: Errand
- On done:
  - Exit: done (finished)
```

A character's code name is made from its name unless you give one. Under a character, bullets are
**pools** of entry lines or one-liners (SPEC §4.12): `Entry pool:` or `One-liner pool:`, a name, and
`(random)`, `(in order)` or `(in order, loops)`, random unless given. Each bullet inside is a line he says;
`If: condition` is the pool's condition. Pools exist only here: inside a dialogue, variants and conditions
decide (SPEC §4.12).

## Templates

A template is a set of fields for one kind of element (SPEC §4.21): a merchant's prices, a quest's reward, a
line's emotion. Fields are made in **field groups**; a **template** is a list of groups, for a kind of entity
or for storylines, situations, dialogues or lines. Each kind has a default template — named after it,
*Character*, *Line* — which a `## Template:` of that name adds groups to.

```outline
# Templates
## Kind: Item
> A kind of your own: its entities are "## Item: name" blocks.
Code name: Item

## Field group: Shop
> Each bullet a field: Name: type = default, settings — tooltip. Under it: Code name:, Tooltip:, comments.
- Stock: list of references to Item
- Price markup: number = 1, from 0.5 to 3, changes in play, on canvas — What he charges over the price
- Mood: choice (calm, angry, afraid) = calm, changes in play
- Shop sign: player-facing text, at most 40 letters
- Opens when: condition
- Trusted: yes/no, required
  - Code name: IsTrusted

## Template: Merchant
For: Character
Groups: Shop

## Field group: Delivery
- Volume: choice (whisper, shout), on canvas
- Recording note

## Template: Line
For: Line
Groups: Delivery

## Field group: Quest
- Reward: number, on canvas
- Journal title: player-facing text

## Template: Storyline
For: Storyline
Groups: Quest

## Facts: Weather
- is_raining: yes/no

## Item: Silver sword

## Character: Borvin
Template: Merchant
Field Stock: Silver sword
Field Price markup: 1.5
Field Mood: angry
Field Shop sign: Borvin's blades
Field Opens when: not is_raining
Field Trusted: yes

## Situation: The smithy
- Dialogue: Haggle
  - Borvin: What'll it be?
    - Field Volume: shout
  - Player: Just looking.
  - Exit: done

## Storyline: Buy a sword
Field Reward: 100
Field Journal title: A sword for the road
- Place: The smithy
- On done:
  - Exit: done (finished)
```

- **A field's type**: `text` (unless given) — notes for the team; `player-facing text` — what the player reads,
  given a line ID and translated; `number`; `yes/no`; `choice (a, b, c)`; `reference` or `list of references`,
  with `to Item` or `to Character or Location` for the kinds it allows; `file` (or `image`), a path in
  `resources/`; `condition`.
- **After the type**: `= default`, then any of `required`, `changes in play` (yes/no, number, choice and
  reference only), `on canvas`, `from 1 to 5`, `at least 0`, `at most 3` — a number's — or `at most 40 letters`
  — a text's. `— words` at the end is its tooltip. A code name is made from its name unless a `Code name:`
  bullet under it gives one. Player-facing text has no default.
- **On an element**: `Template: name` puts it on that template — the default, unless given — and
  `Field name: value` sets a value. An entity, storyline or situation writes both as keys; a line or a
  dialogue as bullets under it, before its steps. A value is written as a person reads it: a choice or an
  entity by its name, a list of entities with commas, a condition in Storyliner's words, `yes` or `no`.
- **`## Character fields`** still works: its fields go into the kind's default template, in a group named
  after the kind. So do `## Location fields`, `## Faction fields` and `## Item fields` for a kind of your own.

## The journal, the codex and interface text

The **journal** is what the player reads about his quests; the **codex**, what he's learned about the world;
**interface text**, the game's words outside the story — menus, the HUD (SPEC §4.22, §4.23). All of it is
player-facing: each piece gets a line ID and is translated.

```outline
# The journal
## Journal categories
> The journal's headings, in order. A quest's Category: makes one that isn't here.
- Main
- Side

## Facts: Things
- slain

## Character: Elder
> Each "- Codex:" bullet a page of his codex entry, in the order the player reads them.
- Codex: He keeps the village.
- Codex: He fought the beast once, and lost.
  - Unlocked when: slain
  - Translator note: “Lost”: he barely lived.

## Journal quest: The beast of the woods
> Follows: makes it follow a storyline — active when it's taken, done when it finishes, failed when it fails.
Follows: Hunt
Category: Main
Code name: Beast
Description: A notice offers coin for the beast.
- Objective: Talk to the elder
- Objective: Deal with the beast

## Journal quest: Water for the well
> With no Follows:, it's made by hand: only Journal steps show it, finish it or fail it.
Category: Side

## Text table: Main menu
> Each bullet a row: key: text. A key uses letters, digits, _ and ., unique in the project.
Code name: MainMenu
- menu.new_game: New game
  - Longest: 20
  - Screenshot: ui/main-menu.png
  - Translator note: A button.
- menu.quit: Quit

## Situation: The elder
- Dialogue: Talk
  - Elder: Kill it.
  - Journal entry: The beast of the woods — The elder wants it dead.
  - Journal objective: Talk to the elder (done)
  - Codex unlock: Elder
  - Journal show: Water for the well
  - Journal finish: Water for the well
  - Exit: done

## Storyline: Hunt
- Place: The elder
- On done:
  - Journal objective: The beast of the woods › Deal with the beast
  - Exit: done (finished)
```

- **`## Journal quest: title`** — the title is what the player reads. `Follows: storyline` makes it follow
  that storyline; `Wait for a Journal node: yes` keeps it hidden until a `Journal show:` (only with
  `Follows:`). `Description:` is the player's, not a note for the team. Each `- Objective: words` is an
  objective, in the order the player sees them.
- **Journal steps** stand anywhere a step can, each one node doing one thing:
  - `Journal show: quest` · `Journal finish: quest` · `Journal fail: quest` — a quest that follows its
    storyline shows, finishes and fails with it, so these are for one made by hand (`show` also for one that
    waits);
  - `Journal entry: quest — the words`, a dash between;
  - `Journal objective: objective (done)` — `(shown)` unless given, or `(failed)`; `quest › objective` where
    two quests have one of that name;
  - `Codex unlock: entity` — its first page — or `Codex unlock: entity, page 2`.
- **`- Codex: text`** under any entity's block is a page of its codex; `Unlocked when: condition` under it, and
  comments. A page with neither a `Codex unlock:` nor a condition is never unlocked: the editor marks it.
- **`## Text table: name`** — `Code name:` the string table the game reads; each row `key: text`, with
  `Longest: letters`, `Screenshot: file in resources/` and comments under it.

## Storylines and situations

```outline
# Storylines and situations

## Facts: Things
- ready

## Situation: Boat
Mode: cutscene
Present: Santer
Place: The harbour
Entries: in, swimming
Exits: arrived, drowned
Description: You arrive.

- Dialogue: On board
  - Santer: Tallon, at last.
  - Exit: arrived
- Entry: swimming
- Dialogue: In the water
  - Narrator: You swim.
  - Exit: drowned

## Character: Santer

## Location: The harbour

## Storyline: Rebellion in Tallon
Code name: Rebellion
Available when: ready
Fact: met_santer: yes/no — You met Santer
On finish: ready = false
Description: The main quest.

- Entry: start
- Place: Boat → in
- On arrived:
  - Exit: arrived (finished)
- On drowned:
  - Exit: drowned (failed)
```

| Key | On | Means |
|---|---|---|
| `Mode:` | situation | scene (unless given), cutscene or free play: a label for now |
| `Present:` | situation | The characters there but silent, by name, with commas |
| `Place:` | situation | The place it's set |
| `Entries:`, `Exits:` | both | Pins listed up front, in that order, with `(finished)` or `(failed)` on a storyline's exits. Needed only for a pin nothing reaches yet; a pin is otherwise made when it's first used |
| `Code name:` | storyline | Used in conditions, `quest.Rebellion.finished`. Made from its name unless given |
| `Available when:` | storyline | Its condition |
| `Available at the start:` | storyline | `no`: a Quest node makes it available. `yes` unless given |
| `Fact:` | storyline | One of its own quest facts, written as in a fact list: `quest.Rebellion.met_santer` |
| `On finish:`, `On fail:` | storyline | Assignments |

## Steps

The bullets of a storyline, a situation or a dialogue are its steps. **Each leads to the next**, so a run of
bullets is a chain of arrows.

| Step | On | Means |
|---|---|---|
| `Anna: words` | dialogue | A line, by a character, `Narrator`, `Player` or `UI`. `Say Anna: words` is the same, for a speaker whose name reads as a step |
| `Choice:` | dialogue | Options; what's left open at the end of each goes on to the next step |
| `Hub:` | dialogue | Options that come back: a branch that ends returns to the hub by itself (SPEC §4.11). What follows the hub is where its exit, and any `Break hub`, go |
| `Break hub` | dialogue | Leaves the hub the flow is in |
| `Dialogue: name` | situation, dialogue | A dialogue; its steps are inside it |
| `Place: situation` | storyline, situation | A placed situation. `Place: In jail → with Winkel` names the entry, needed when it has several |
| `On exit name:` | right after `Dialogue:` or `Place:` | What happens after that way out; its steps are inside it |
| `If: condition` | all | A Condition; the steps inside are its True |
| `Else:` | right after `If:` | Its False. Without one, False goes on to the next step |
| `If undecided: why` | all | A Condition not written yet: it counts as false, and it's in the report |
| `Set: a = 1; b += 2` | all | An Execution node |
| `Wait: trigger` | all | A Wait for trigger node |
| `Event: PlaySound what="boom"` | all | An Engine event node; arguments `name=value`, with commas |
| `Quest: take Name` | all | A Quest node: `take`, `make available`, `make unavailable`, or `end Name → its exit`. Inside it: `Override`, `Set: …` |
| `Phase start: name`, `Phase end: name` | all | Phase nodes |
| `Journal show: quest`, `Journal entry: quest — words`, `Journal objective: objective (done)`, `Journal finish: quest`, `Journal fail: quest`, `Codex unlock: entity, page 2` | all | A Journal node (*The journal, the codex and interface text*, above) |
| `Entry: name` | storyline, situation | Starts a new flow from this entry pin. A body with no `Entry:` first starts from `in` (a storyline: `start`) |
| `Exit: name` | all | Leaves through this exit pin. On a storyline `Exit: name (finished)` or `(failed)`. `(ends the conversation)` forces the talk's end |
| `Label: name` | all | Names the next step, for `Go to:` |
| `Go to: name` | all | An arrow to that label, on this canvas only. `Go to: jail → with Winkel`, for a placed situation with several entries |
| `Stop: why` | all | The flow ends here: a dead end, kept, marked TODO, and in the report |

**Every flow ends in** `Exit:`, `Go to:`, or `Stop:` — except on a situation, where a dialogue's or placed
situation's way out with nothing after it leads out under the same name. Nothing else is assumed.

Where things go is SPEC §4.1: lines, Choice, Hub and Break hub only in a dialogue; a storyline holds placed
situations, not dialogues.

### Under a line

```outline
# Under a line

## Facts: Things
- dragon_dead

## Character: Lance

## Situation: Talk
- Dialogue: Talk
  - Lance: I'll kill it.
    - Variant if dragon_dead: It's dead!
    - Timing: 2.5, pause 1
    - VO: Shouted.
  - Exit: out

## Storyline: Talk
- Place: Talk
- On out:
  - Exit: done (finished)
```

`Variant if condition: words` — first true wins; `Menu: words` — with *Menu text on options* on; `Timing: 2.5`
or `Timing: 2.5, pause 1`, in seconds; comments. What comes *after* a line goes beside it, not under it.

### Under a Choice or a Hub

A bullet under `Choice:` or `Hub:` is an **option**, and its words are what the menu shows. If those words
would read as a comment or a setting, write `Option: TODO: someone to talk to`.

```outline
# Under a hub

## Facts: Things
- knows_about_sister

## Character: Ealdorman

## Situation: Asking
- Dialogue: Asking
  - Hub:
    - Remembers: for good
    - When all used: carries on
    - About the dragon
      - Player: What do you know about the dragon?
      - Ealdorman: Nothing.
    - About Lance's sister
      - If: knows_about_sister
        - Ealdorman: Leave.
    - Goodbye
      - Once used: stays
      - Break hub
  - Exit: done

## Storyline: Asking
- Place: Asking
- On done:
  - Exit: done (finished)
```

- **An option says something when it's picked:** the first line its arrow reaches (SPEC §4.10). If its
  steps don't start with a line, **its own words become the player's line**. That's *Goodbye* above: the
  player says “Goodbye”, then the hub is left.
- **`If:` straight under an option is an option condition** (SPEC §4.10), and only Conditions may stand
  between an option and its line. With no `Else:`, the option isn't shown while it's false. *About Lance's
  sister* above. If the steps inside the `If:` don't start with a line, the option's words are said there.
- `Once used: disappears`, `greys out` or `stays`. On a hub an option greys out unless told; on a Choice it
  doesn't matter.
- On a hub: `Remembers: this conversation` (unless told) or `for good`; `When all used: leaves` (unless told)
  or `carries on` (SPEC §4.11).

### Shared lines and loops

```outline
# Shared lines

## Character: Anna

## Situation: Loop
- Dialogue: Loop
  - Choice:
    - One
      - Anna: The first.
      - Go to: both
    - Two
      - Anna: The second.
      - Go to: both
  - Label: both
  - Anna: Either way.
  - Exit: out

## Storyline: Loop
- Place: Loop
- On out:
  - Exit: done (finished)
```

`Label:` names the next step; `Go to:` reaches it from anywhere on the same canvas, before or after it. To
reach another canvas, leave by an `Exit:` and carry on beside it with `On:`. That's how a dialogue's flow
gets back to something on the situation's canvas:

```outline
# Back to waiting

## Triggers
- came_back

## Character: Fechner

## Situation: The syringe
- Label: waiting
- Wait: came_back
- Dialogue: Asked again
  - Fechner: Do you have it?
  - Choice:
    - Not yet.
      - Exit: not yet
    - Here it is.
      - Exit: done
- On not yet:
  - Go to: waiting
- On done:
  - Exit: finished

## Storyline: The syringe
- Place: The syringe
- On finished:
  - Exit: done (finished)
```
