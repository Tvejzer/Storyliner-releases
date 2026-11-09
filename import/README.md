# The import kit

How quests written elsewhere come into Storyliner (SPEC §14). **Written for an AI agent**, such as Claude
Code, working for a designer: you read the other tool's files and write an **outline**; Storyliner turns
the outline into a real project, makes every ID, lays the canvases out, and tells you and the designer
what it found.

| Read | For |
|---|---|
| [`outline-format.md`](outline-format.md) | The format: everything an outline can say |
| [`articy.md`](articy.md) | articy:draft, its JSON export |
| [`twine.md`](twine.md) | Twine, as Twee |
| [`ink.md`](ink.md) | Ink |
| [`yarn.md`](yarn.md) | Yarn Spinner |
| [`notes.md`](notes.md) | Miro boards, documents, loose notes |

## What you do

1. **Read** `outline-format.md` and your tool's guide. Then read the source. Never change it.
2. **Write the outline**, `Name.outline.md`. A script that drafts it from the source is fine, for an
   export of any size; then read what it wrote, and correct it by hand.
3. **Check it**, as often as you like. Nothing is written:

   ```bash
   npm run import -- "Name.outline.md" --check --source "path/to/the/source"
   ```

   Every mistake is listed with its line. Correct them all. `--source` takes the other tool's file or
   folder, counts what's in it, and checks that **every ID in it is accounted for**: as an old ID on what
   it became, or under `Not mapped:`.
4. **Import it**, into a folder of its own:

   ```bash
   npm run import -- "Name.outline.md" --out "Projects/Name" --source "path/to/the/source"
   ```

   This writes the project, `import/Name.report.md` and `import/Name.ids.json`, the mapping file of old
   IDs. It refuses a folder that already holds a project unless you say `--replace`, which is for a
   project you made and nobody has touched: **once a designer has edited it, the file is the source**, and
   importing again would lose their work.
5. **Read the report**, and fix what's yours (below). The same outline and seed always make the same
   file, so a second run changes only what you changed.
6. **Give the designer the report.** Its *Needs your hand* is their list.

The designer can also bring an outline in by hand: **File ▸ Import an outline…** in the editor makes a new
project, or adds to the open one as one undo step, and shows the same report.

## The report

| Part | What to do with it |
|---|---|
| *What came in, what came out* | Do the source's counts and the project's make sense together? A dialogue fragment is one line, so 117 should be 117. A Twine passage can be several lines, so read those counts as a sanity check. |
| *Source IDs accounted for* | **Must be all of them.** One missing means something of the source fell out: bring it in, or say why not under `Not mapped:`. |
| *Couldn't be mapped* | What you left out, in your words, from `Not mapped:`. The designer reads this. |
| *Needs your hand* | Everything you flagged, every TODO, every flow that stops, every condition left undecided. |
| *What the checks found* | The validator's findings (SPEC §9), as the editor's Problems tab shows them. An error is a mistake of yours — fix the outline. A warning is either a mistake of yours or a hole in the source — a cut-off line, a fact set and never read — which you keep and flag. |

## Flag, don't guess

A conversion has to decide things, and the designer can't check what they aren't told about. **When the
source doesn't say, or says something odd, write it down instead of smoothing it over:**

- **`Flag: …`** on the element it's about, or on the project: it becomes a TODO on that element, and a row
  in the report. Use it for anything you invented (a trigger's name, what starts a scene), anything
  ambiguous, anything you decided the designer may decide otherwise.
- **`Not mapped: …`** for what didn't come in at all. Say why.
- **`If undecided: …`** for a branch that depends on something not written yet. It stays a question, and
  counts as false in play.
- **`Stop: …`** where the source's flow simply ends. Holes stay holes: finding them is what the validator
  is for, and an invented ending hides one.

And three things you never do on your own:

- **Never change the words.** Text comes in as written, typos included. List the typos in one `Flag:`;
  fixing them is rewriting, and rewriting is the designer's. If they have asked you to fix spelling, list
  every fix.
- **Never repair the source's logic.** A variable checked under another name than the one set, a condition
  that can never be true: do what the story clearly means if you can tell, and `Flag:` exactly what you
  did. *Kill the Dragon*'s Twine file had six such bugs (`Projects/README.md`).
- **Never leave the outline's format to make something fit.** What the format can't say gets a `Flag:`, or
  a `Not mapped:`. The designer adds it by hand.

## What the format doesn't cover yet

Wheel options and own dialogues (*Add dialogue to*, *Remove dialogue from*), a place's scene lists, Phase
switches, numbered Conditions, *asked again* variants and sections. Flag what you'd have needed them for.
Templates and their fields are covered (`outline-format.md`, *Templates*), and so are the journal, the codex
and interface text (*The journal, the codex and interface text*): a quest log's quests and objectives, notes
the story adds to them, encyclopedia pages, menu strings. Write them as they are, unflagged.

Examples of whole conversions: `fixtures/The Fallen Leaves.outline.md`, written from articy following
these guides. `Projects/README.md` says what the first conversions had to decide.
