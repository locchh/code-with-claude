# Claude Code with [Plannotator](https://github.com/backnotprop/plannotator/tree/main/apps/hook)

## Quick Start

```bash
# Install the plannotator command
curl -fsSL https://plannotator.ai/install.sh | bash


# Run claude code
claude

# Then in Claude Code
/plugin marketplace add backnotprop/plannotator
/plugin install plannotator@plannotator

# IMPORTANT: Restart Claude Code after plugin install
```

If you prefer not to use the plugin system, add this to your `~/.claude/settings.json` or `./.claude/settings.json`:

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": "ExitPlanMode",
        "hooks": [
          {
            "type": "command",
            "command": "plannotator",
            "timeout": 1800
          }
        ]
      }
    ]
  }
}
```


## How It Works

When Claude Code calls `ExitPlanMode`, this hook intercepts and:

- Opens Plannotator UI in your browser
- Lets you annotate the plan visually
- Approve → Claude proceeds with implementation
- Request changes → Your annotations are sent back to Claude
- On resubmission → Plan Diff shows what changed since the last version


## Tip - Toggle Plannotator per prompt

Instead of always opening Plannotator, use keyword detection so it only activates when you ask for it. Replace the hook config above with:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.prompt' | grep -qi 'plannotator' && touch /tmp/.plannotator-session || true; exit 0"
          }
        ]
      }
    ],
    "PermissionRequest": [
      {
        "matcher": "ExitPlanMode",
        "hooks": [
          {
            "type": "command",
            "command": "[ -f /tmp/.plannotator-session ] && rm /tmp/.plannotator-session && plannotator || echo '{\"hookSpecificOutput\":{\"hookEventName\":\"PermissionRequest\",\"permissionDecision\":\"allow\"}}'",
            "timeout": 1800
          }
        ]
      }
    ]
  }
}
```

Now mention `plannotator` in your prompt to activate it, or omit it to auto-approve:

```
# Auto-executes without review
create a plan to add user authentication

# Opens Plannotator UI
use plannotator to plan the payment integration
```

Mechanism:

```
In our setup they work together:                                                                             
                                     
  You type prompt  →  UserPromptSubmit hook runs
                      (detects "plannotator" → creates flag file)
                             ↓
  Claude plans...
                             ↓
  Claude calls ExitPlanMode  →  PermissionRequest hook runs
                                 (flag exists? → open Plannotator)
                                 (flag absent? → auto-approve)

  UserPromptSubmit is the detector, PermissionRequest is the gatekeeper.
```