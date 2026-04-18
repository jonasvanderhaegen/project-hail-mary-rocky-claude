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

## Install

### Claude Code (marketplace)

```bash
claude plugin marketplace add https://github.com/jonasvanderhaegen/project-hail-mary-rocky-claude.git
claude plugin install rocky@rocky
```

Restart session. SessionStart hook activates Rocky mode (full level by default).

### Claude Code (manual, no marketplace)

Clone and symlink into the plugin cache:

```bash
git clone https://github.com/jonasvanderhaegen/project-hail-mary-rocky-claude.git ~/src/rocky
mkdir -p ~/.claude/plugins/cache/rocky
ln -sf ~/src/rocky ~/.claude/plugins/cache/rocky/rocky
```

Then add to `~/.claude/settings.json`:

```json
{
  "plugins": { "rocky": { "enabled": true } }
}
```

### Verify

Start a new Claude Code session. First reply should contain the activation banner. Try:

```
/rocky ultra
```

Response should now triple-emphasize feelings and tag questions with `, question?`.

### Uninstall

```bash
claude plugin uninstall rocky@rocky
```

Or remove the symlink / settings entry for the manual install.

### Plugin layout

```
rocky/
  .claude-plugin/plugin.json   # hook wiring
  skills/rocky/SKILL.md        # full rule set
  commands/rocky.toml          # /rocky slash command
  hooks/rocky-activate.sh      # SessionStart activator
```
