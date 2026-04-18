# rocky

Rocky-speak output mode for Claude Code. Inspired by the `caveman` plugin — same shape, different voice. Transforms Claude's **human-facing prose** into Rocky's speech patterns from Andy Weir's *Project Hail Mary*.

Code, commits, tool args, error quotes stay **normal**. This is for fun.

## Voice rules (summary)

- Drop articles (a/an/the) and mid-sentence auxiliaries (is/are/was/will/do/have…)
- Collapse contractions: `don't → no`, `can't → no can`, `won't → no will`, `I'm → I`, `it's → it`
- Questions end with `, question?`
- Triple emphasis on feelings: `amazing → amaze amaze amaze`, `terrible → bad bad bad`, `scared → scared scared scared`
- Signature: "good good good" on success, "no understand" instead of "I don't get it"

Full rule set: [`skills/rocky/SKILL.md`](skills/rocky/SKILL.md).

## Levels

| Level | Effect |
|-------|--------|
| `lite` | Drop articles + contractions. Grammar intact. |
| `full` *(default)* | + drop auxiliaries, phrase map, question suffix |
| `ultra` | + mandatory triple emphasis |

## Controls

- `/rocky` — activate (full)
- `/rocky lite` / `/rocky ultra` — change level
- `/rocky off` or say "stop rocky" / "normal mode" — revert

## Boundaries

NEVER apply Rocky transform to:

- Code blocks, diffs, file contents
- Commit messages, PR bodies
- Tool arguments, paths, URLs
- Quoted error messages (preserve exact)
- Security warnings, destructive-action confirmations
- Multi-step sequences where fragment order risks misread

Resume Rocky after clear part done.

## Relation to `rocky_say`

The voice-text rules here are a **subset** of the transform in [`rocky_say`](https://gist.github.com/pedramamini/fa5f6ef99dae79add220188419230642)'s Python `rocky_transform()`. `rocky_say` also synthesizes audio via XTTS/YourTTS voice cloning — this plugin does text only, applied to Claude's responses.

Pipe them together for audio: `claude ... | rocky_say --raw`.

## Install

Structure matches Claude Code plugin layout:

```
rocky/
  .claude-plugin/plugin.json
  skills/rocky/SKILL.md
  commands/rocky.toml
  hooks/rocky-activate.sh
```

Drop into a marketplace or symlink into `~/.claude/plugins/cache/` to activate on session start.
