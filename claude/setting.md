If you want to skip all Claude Code permission confirmation prompts and automatically select “Yes,” there are two main approaches.

---

## 1. Skip all prompts with a CLI flag

The easiest way is to add the `--dangerously-skip-permissions` flag when starting Claude.
This skips all permission prompts and executes immediately.

```bash
# Start the normal interactive REPL without permission prompts
claude --dangerously-skip-permissions

# Similarly in print mode or one-shot mode
claude -p --dangerously-skip-permissions “npm run build && git commit -am ‘update’”
```

> **⚠️ Caution**
> Destructive commands will also be executed, so only use this in a trusted environment (such as a temporary development container) ([Anthropic][1]) ([Anthropic][2])

---

## 2. Register permission rules in the configuration file

This method allows you to set a “Always allow this tool” rule on a per-project or global basis.
Add the following `permissions.allow` to `~/.claude/settings.json` (global) or `.claude/settings.json` in the project root directory.

```jsonc
// Example: ~/.claude/settings.json
{
  "permissions": {
    "allow": [
      "Bash(*)",      // Allow all shell commands
      "Edit(*)",     // Allow all file edits
      "Write(*)",    // Allow all file creations/overwrites
      "WebFetch",    // Allow fetching of arbitrary URLs
      "WebSearch",   // Allow web searches
      "Read",        // Allow file reads
      "Glob", "Grep","LS","MultiEdit","NotebookEdit","TodoWrite"
    ]
  }
}
```

Once the settings are applied, you can start `claude` as usual, and you will no longer be prompted for confirmation when invoking the relevant tools ([Anthropic][3]).

---

### Summary

* **Quickly skip all prompts** → `claude --dangerously-skip-permissions`
* **Permanently auto-allow specific tools** → Add rules to `permissions.allow` in `settings.json`


[1]: https://docs.anthropic.com/en/docs/claude-code/cli-usage "CLI usage and controls - Anthropic"
[2]: https://docs.anthropic.com/en/docs/claude-code/security "Manage permissions and security - Anthropic"
[3]: https://docs.anthropic.com/en/docs/claude-code/settings "Claude Code settings - Anthropic"
