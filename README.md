# OpenCode Workflow

**Stop wrestling with AI assistants. Start collaborating with a development team.**

You know the drill: ask an AI to review your code, wait, then ask it to check security, wait again, then ask about tests. Three questions, three context switches, ten minutes of your life you won't get back.

This workflow flips that script. Ask once. Multiple specialists analyze your code in parallel. You get a synthesized report while you're still sipping your coffee.

## What You Get

| Component | Count | What It Does |
|-----------|-------|--------------|
| **Agents** | 7 | Orchestrator + 6 specialists (security, tests, docs...) |
| **Commands** | 12 | /review, /commit, /architect, /rapid, /debug... |
| **Skills** | 7 | Domain knowledge for APIs, testing, architecture... |
| **Plugins** | 5 | Auto-format, security scans, notifications... |

## Quick Look

```
Your request ──► Orchestrator ──┬──► Code Reviewer    ──┐
                                ├──► Security Auditor ──┼──► Synthesized
                                ├──► Test Architect   ──┤    Report
                                └──► Debugger         ──┘
                                      (all parallel)
```

---

## Installation

OpenCode looks for workflow files in `.opencode/` inside your project directory. This repo uses different folder names (`agents/` instead of `agent/`) so it's easier to browse on GitHub.

### Full Install

```bash
# Clone the workflow
git clone https://github.com/CloudAI-X/opencode-workflow.git

# Create .opencode directory in your project
mkdir -p your-project/.opencode

# Copy all components (note: folder names change!)
cp -r opencode-workflow/agents your-project/.opencode/agent
cp -r opencode-workflow/commands your-project/.opencode/command
cp -r opencode-workflow/skills your-project/.opencode/skill
cp -r opencode-workflow/plugins your-project/.opencode/plugin
```

### Partial Install

Don't need everything? Pick what you want:

```bash
# Just agents and commands (no plugins or skills)
cp -r opencode-workflow/agents your-project/.opencode/agent
cp -r opencode-workflow/commands your-project/.opencode/command
```

### Folder Mapping

| This Repository | Your Project |
|-----------------|--------------|
| `agents/` | `.opencode/agent/` |
| `commands/` | `.opencode/command/` |
| `skills/` | `.opencode/skill/` |
| `plugins/` | `.opencode/plugin/` |

### Verify It Works

```bash
cd your-project
opencode
# Type / and Tab - should see commands like /review, /commit
# Press Tab repeatedly - should see Orchestrator as a primary agent
# Type @ and Tab - should see subagents like @code-reviewer
```

---

## How to Use

### Primary Agents (Tab to switch)

Press **Tab** to cycle between primary agents:

| Agent | What It Does |
|-------|--------------|
| **build** | Default. Full development work. |
| **plan** | Analysis only, no file changes. |
| **orchestrator** | Coordinates complex multi-step tasks. |

### Subagents (@mention)

Invoke specialists by mentioning them:

```
@security-auditor Check the auth module for vulnerabilities
```

```
@code-reviewer Review the changes I just made
```

| Subagent | Focus | Has Bash? | Can Edit? |
|----------|-------|-----------|-----------|
| `@code-reviewer` | Quality, patterns, maintainability | No | No |
| `@debugger` | Bug investigation, root cause | Yes | No |
| `@security-auditor` | OWASP Top 10, vulnerabilities | No | No |
| `@refactorer` | Clean code, design patterns | No | Yes |
| `@test-architect` | Test strategy, coverage | No | Yes |
| `@docs-writer` | README, API docs, guides | No | Yes |

### Commands (/ to invoke)

Quick workflows for common tasks:

| Command | What It Does |
|---------|--------------|
| `/review` | Multi-perspective code review |
| `/commit` | Generate conventional commit message |
| `/architect` | High-level design session |
| `/rapid` | Fast iteration, minimal ceremony |
| `/debug` | Systematic bug investigation |
| `/refactor` | Code cleanup workflow |
| `/security-audit` | OWASP vulnerability check |
| `/test-design` | Plan test coverage |
| `/docs` | Generate documentation |
| `/parallel` | Run multiple tasks at once |
| `/verify-changes` | Lint → Type → Build → Test → Review |
| `/mentor` | Educational explanations |

---

## The Philosophy

### Orchestrator Pattern

The orchestrator doesn't try to do everything itself. It follows a 6-phase workflow:

```
UNDERSTAND → PLAN → DELEGATE → INTEGRATE → VERIFY → DELIVER
```

1. **Understand** the request and context
2. **Plan** the approach, identify specialists needed
3. **Delegate** to subagents (in parallel when possible)
4. **Integrate** their findings into a coherent response
5. **Verify** the work meets requirements
6. **Deliver** the final result

### Parallel Execution

This is the key insight: **independent tasks should run simultaneously**.

Five 30-second reviews take 30 seconds total, not 2.5 minutes.

The trick is putting all subagent calls in a **single message**. Separate messages = sequential. Single message with multiple calls = parallel.

### Adversarial Verification

One reviewer misses things. Multiple reviewers with different mandates catch more:

- Security auditor asks: "How can an attacker break this?"
- Code reviewer asks: "Is this maintainable?"
- Test architect asks: "Is this actually tested?"

The tension between perspectives surfaces important tradeoffs.

### Guardrails Without Friction

Plugins protect without blocking your workflow:

- **security-scan**: Blocks edits to `.env`, credentials, keys
- **auto-format**: Runs Prettier/Black after edits (non-blocking)
- **verification**: Reminds you to test after editing 3+ files
- **notifications**: macOS notification when work finishes
- **parallel-guard**: Educates about parallel execution patterns

---

## Customization

### Add Your Own Agent

Create `your-project/.opencode/agent/my-agent.md`:

```yaml
---
description: What this agent does
mode: subagent
model: anthropic/claude-sonnet-4-20250514
tools:
  write: false
  edit: false
  bash: false
---
Your custom instructions here...
```

### Add Your Own Command

Create `your-project/.opencode/command/my-command.md`:

```yaml
---
description: What this command does
agent: build
---
Your command prompt here...

Use $ARGUMENTS for user input.
```

### Add Your Own Skill

Create `your-project/.opencode/skill/my-skill/SKILL.md`:

```yaml
---
name: my-skill
description: Domain knowledge for...
---
Knowledge content here...
```

---

## Troubleshooting

**Agents not appearing after Tab?**
- Check that `.opencode/agent/` exists (singular `agent`, not `agents`)
- Verify markdown files have proper YAML frontmatter with `---` delimiters

**Commands not showing with /?**
- Commands go in `.opencode/command/` (singular)
- File name becomes the command name (`commit.md` → `/commit`)

**Plugins causing errors?**
- Check for TypeScript syntax errors
- Plugins must export an async function returning a hooks object
- Use optional chaining (`?.`) when accessing potentially undefined properties

**Skills not loading?**
- Each skill needs its own folder with `SKILL.md` inside
- The `name` field in frontmatter should match the folder name

---

## Credits

Inspired by [claude-workflow-v2](https://github.com/anthropics/claude-workflow-v2) patterns. Built for [OpenCode CLI](https://opencode.ai).

Created by [@cloudxdev](https://x.com/cloudxdev)

## License

MIT
