# clawd-matsci

A mod for [Claude Code](https://code.claude.com): Clawd, the small creature from the
Claude Code banner, runs around in a strip above your prompt and reacts to the session.

**In this version Clawd is the MatSci octopus**, a blue octopus with wiggling
tentacles, all the time. Other looks (`/clawd emote ...`) take over for a while, then the
octopus comes back. The small Clawds for subagents stay small Clawds. The octopus is two
rows taller than Clawd, so the strip is 5 rows high instead of 4; in a terminal too short
for that, Clawd shows as Clawd. The plain version is
[Probst1nator/clawd](https://github.com/Probst1nator/clawd). Install one or the other:
both are the plugin `clawd`.

- It hops when you send a prompt.
- It holds up a scroll while Claude only reads, and stacks a brick for every other tool
  call. The next prompt kicks the pile over.
- It trips when a tool call fails and celebrates when a turn ends.
- Each subagent gets a small Clawd of its own, which runs off when the subagent is done.
- Between events it wanders, jumps and chases sparks. After 3 idle minutes it falls asleep.

This is a fan project. Anthropic did not make it and does not endorse it.

## Install

```bash
claude plugin marketplace add AutomatedAlchemy/clawd-matsci
claude plugin install clawd@clawd-matsci
```

Start a new session. `/clawd help` explains the rest. `/clawd off` hides Clawd (it
stays hidden in later sessions until `/clawd on`), and
`claude plugin disable clawd@clawd-matsci` turns the mod off.

Tested with Claude Code 2.1.292. The mod uses plugin hooks that draw into the terminal,
so older versions may not load it.

## Commands

| Command | What it does |
|---|---|
| `/clawd` | what Clawd is doing now |
| `/clawd help` | every command; `help uml` draws how Clawd behaves |
| `/clawd jump 3` | play an act now, up to 5 times in a row |
| `/clawd a1 wave` | a subagent's small Clawd plays an act |
| `/clawd list acts` | the acts; also `made`, `emotes`, `minis` |
| `/clawd on`, `/clawd off` | show or hide Clawd |
| `/clawd autopick on`, `off` | let a model choose what Clawd plays (see below) |

While you type `/clawd `, a list above the prompt shows what fits, and a space writes out
a word that fits one name.

## Model calls

By default the mod makes no model calls. Clawd picks random acts by itself.

`/clawd autopick on` hands that choice to a model (Sonnet). It reads what happens in the
session and picks what Clawd and the small Clawds play: after each prompt and turn, and
every 10 to 60 seconds. In a busy session that is about 100 calls an hour of about 1,500
tokens each, and it may have Opus write up to 5 new acts or looks a day. All of it runs
through your own Claude Code login and counts against your usage. `/clawd autopick off`
stops it. The setting is remembered.

`/clawd create <name> <what it looks like>` has Opus draw a new emote, a look Clawd takes
for a while, and `/clawd change <emote> <what to change>` redraws one. Each runs only when
you type it.

## Where it keeps things

New acts, emotes, their preview images and the autopick traces (`/clawd trace`) go to
`~/.claude/clawd/`, or `$CLAUDE_CONFIG_DIR/clawd/` when that is set. A plugin update
leaves that folder alone. Writing preview images needs `python3` on the PATH.

## Development

`plugin/` is the mod: `hooks/register.tsx` holds the hooks, `hooks/clawd-sim.ts` the
world, the animation and the drawing. Check it with `claude plugin validate plugin` and
`claude plugin test plugin`.

This repository gets release snapshots from a private working copy. Issues are welcome.

## License

MIT, see [LICENSE](LICENSE).
