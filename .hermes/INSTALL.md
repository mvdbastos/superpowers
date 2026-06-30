# Installing Superpowers for Hermes Agent

Enable superpowers skills in [Hermes Agent](https://github.com/NousResearch/hermes-agent) via the built-in skills system.

## Prerequisites

- Hermes Agent installed (`pip install hermes-agent` or from source)
- Git

## Installation

**Option A — Symlink (recommended, auto-updates with `git pull`):**

```bash
git clone https://github.com/obra/superpowers.git ~/.skills/superpowers
ln -s ~/.skills/superpowers/skills/* ~/.hermes/skills/
```

**Windows (PowerShell):**
```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.skills\superpowers"
Get-ChildItem "$env:USERPROFILE\.skills\superpowers\skills" -Directory | ForEach-Object {
    cmd /c mklink /J "$env:USERPROFILE\.hermes\skills\$($_.Name)" $_.FullName
}
```

**Windows Native (PowerShell plain, multiple symlinks):**
```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.skills\superpowers"
Get-ChildItem "$env:USERPROFILE\.skills\superpowers\skills" -Directory | ForEach-Object {
    cmd /c mklink /J "$env:LOCALAPPDATA\hermes\skills\superpowers\$($_.Name)" $_.FullName
}
```

<!-- symlynk $env:USERPROFILE\.skills\superpowers to $env:LOCALAPPDATA\hermes\skills\superpowers -->
**Windows Native (PowerShell grouped, single symlink):**
```powershell
git clone https://github.com/obra/superpowers.git "$env:USERPROFILE\.skills\superpowers"
cmd /c mklink /J "$env:LOCALAPPDATA\hermes\skills\superpowers" "$env:USERPROFILE\.skills\superpowers\skills"
```

**Option B — Copy (standalone, no git dependency after install):**

```bash
git clone https://github.com/obra/superpowers.git /tmp/superpowers
cp -r /tmp/superpowers/skills/* ~/.hermes/skills/
rm -rf /tmp/superpowers
```

## Verify

```bash
hermes skills list
```

You should see superpowers skills alongside the built-in ones (e.g. `test-driven-development`, `brainstorming`, `writing-plans`).

Load any skill in a chat session:

```
/skill test-driven-development
```

## Updating

If you used the symlink method:

```bash
cd ~/.skills/superpowers && git pull
```

Skills update instantly through the symlinks.

## Uninstalling

```bash
# Remove symlinks
for skill in ~/.hermes/skills/*; do
    [ -L "$skill" ] && readlink "$skill" | grep -q superpowers && rm "$skill"
done

# Optionally delete the clone
rm -rf ~/.skills/superpowers
```

## How it works

Hermes Agent loads skills from `~/.hermes/skills/`. Each skill is a directory
with a `SKILL.md` file (YAML frontmatter + markdown body) — the same format
Superpowers uses natively. No conversion or adapter needed.

Skills are loaded on demand via `/skill <name>` in the CLI, or automatically
when the agent detects a matching task pattern. Hermes also supports skill
preloading via `hermes -s test-driven-development` for session-wide activation.

## Compatibility

Hermes Agent has built-in equivalents for several Superpowers skills.
Installing Superpowers will add its versions alongside the built-ins.
If both exist, you can load either by name — Superpowers skills will
appear in `hermes skills list` just like any other skill.
