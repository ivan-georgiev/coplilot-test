# Effective Agent Design Principles

This document captures the principles and patterns we've established for creating well-structured, maintainable, and effective AI agents.

---

## Core Design Philosophy

### 1. Concise Over Verbose

**Principle**: Keep agent definitions small and focused. Aim for ~50-100 lines for the main agent file.

**Why it matters**:
- Easier to understand and maintain
- Reduces duplication and inconsistency
- Improves LLM comprehension and adherence
- Faster iteration cycles

**How to achieve**:
- Use resource files for detailed implementation strategies
- Extract repetitive patterns into separate documents
- Remove redundant explanations and examples
- Follow the "shell + strategy" pattern

---

## Architectural Patterns

### 2. Shell + Strategy Pattern

**Structure**:
```
agent-name.agent.md (50-100 lines)
  ├─ Core identity & purpose
  ├─ Tool list
  ├─ High-level workflow (3-5 steps)
  ├─ Core principles (5-10 items)
  └─ References to strategy files

resources/strategies/
  ├─ strategy-1.md (detailed steps)
  ├─ strategy-2.md (detailed steps)
  └─ strategy-n.md (detailed steps)
```

**Benefits**:
- Main agent file stays concise
- Strategy files can be deep without bloating agent definition
- Easy to add new strategies without modifying agent
- Clear separation between orchestration and implementation

---

### 3. Single Responsibility

**Principle**: Each agent should have one clear, focused purpose.

**Guidelines**:
- Agent name should reflect its single responsibility
- Description should be one sentence
- If you need "and" or "or" in the description, consider splitting

**Anti-pattern**: Agents that "do everything" become impossible to maintain and unpredictable in behavior.

---

## Agent-to-Agent Communication

### 4. Protocol-Based Handoffs

**Principle**: Use structured protocol blocks in conversation history instead of YAML variable interpolation.

**Protocol Format**:
```
────────────────────────────────────────────
🔬 [PROTOCOL NAME]
────────────────────────────────────────────
Key: Value
Key: Value
Key: [Array, Of, Values]
────────────────────────────────────────────
```

**Why protocols work better**:
- Full conversation context preserved automatically
- Easy to parse with clear visual delimiters
- Flexible: can add fields without changing handoff config
- Debuggable: users can see what's being passed
- No variable interpolation errors

**Handoff Agent Instructions**:
- **Sending Agent**: Output the protocol block, suggest handoff button
- **Receiving Agent**: Search conversation history for protocol delimiter, parse key-value pairs

---

### 5. Explicit Parsing Instructions

**Principle**: Tell receiving agents exactly what to look for in conversation history.

**Pattern**:
```markdown
### Phase 0: Parse Handoff Context

If invoked via handoff, search conversation history for:

```
🔬 PROTOCOL_NAME
────────────────────────────────────────────
[key-value pairs]
────────────────────────────────────────────
```

Extract:
- Key1: [purpose]
- Key2: [purpose]
- Key3: [purpose]

Use this context to:
- [Action 1]
- [Action 2]
```

**Why it matters**: LLMs can overlook details in long transcripts unless explicitly told where to look.

---

## Workflow Design

### 6. Clear Step-by-Step Workflows

**Principle**: Break agent execution into numbered, sequential steps.

**Structure**:
```markdown
## Workflow

### Step 1: [Action Verb] + [Object]
- Sub-action 1
- Sub-action 2

### Step 2: [Action Verb] + [Object]
- Sub-action 1
- Sub-action 2

### Step 3: [Action Verb] + [Object]
- Sub-action 1
- Sub-action 2
```

**Guidelines**:
- Use action verbs: Identify, Gather, Generate, Validate, Parse, Execute
- Keep steps at high level (details in strategies)
- 3-5 steps is ideal
- Each step should have clear entry/exit conditions

---

### 7. Separation of Concerns

**Principle**: Keep orchestration logic separate from implementation details.

**What belongs in agent file**:
- ✅ Workflow coordination
- ✅ Tool selection and invocation
- ✅ Handoff preparation
- ✅ High-level decision making

**What belongs in strategy files**:
- ✅ Detailed implementation patterns
- ✅ Framework-specific code examples
- ✅ Technical best practices
- ✅ Common pitfalls and solutions

**What belongs in neither** (avoid duplication):
- ❌ Repeated examples across multiple files
- ❌ Framework documentation (link to external docs instead)
- ❌ Multiple explanations of the same concept

---

## Tool Configuration

### 8. Minimal Tool Lists

**Principle**: Only include tools the agent actually uses.

**Pattern**:
```yaml
tools:
  ['read/readFile', 'edit', 'search']  # Only what's needed
```

**Guidelines**:
- Don't include tools "just in case"
- Use wildcards for tool families: `'chrome-devtools/*'`
- MCP servers declared separately from tools
- Test that all listed tools are actually invoked

---

### 9. Intentional Model Assignment

**Principle**: Pick the model tier based on the agent's cognitive demands, not its position in the pipeline.

**Guidelines**:
- Use the most capable model (e.g., Opus) for agents that must reason across multiple domains, parse ambiguous input, or make high-stakes decisions (planner, researcher, architecture gate).
- Use a cost-efficient model (e.g., Sonnet) for agents that follow well-defined steps within a constrained scope (builder, reviewer, knowledge curator).
- Document the rationale when a model assignment does not follow the defaults.
- Reassess model assignments when an agent consistently fails at tasks within its scope.

---

## Content Organization

### 9. DRY Principle (Don't Repeat Yourself)

**Principle**: Define each concept, instruction, or example exactly once.

**How to achieve**:
- Identify duplicate content across files
- Extract common patterns to shared documents
- Use references instead of copying
- Centralize cross-cutting concerns in agent file

**Example**: If test focusing logic is identical across 5 strategies, move it to the agent file that orchestrates all strategies.

---

### 10. Progressive Disclosure

**Principle**: Present information in order of importance and frequency of use.

**Structure**:
```markdown
1. Core identity (what is this agent?)
2. Common workflows (what will users do most?)
3. Tool configuration (what can it access?)
4. Detailed strategies (how does it work in depth?)
5. Edge cases and advanced patterns (rarely needed)
```

**Benefits**:
- Users find what they need quickly
- LLM focuses on most important context first
- Reduces cognitive load

---

## Quality Indicators

### 11. Signs of a Well-Designed Agent

**Structural indicators**:
- [ ] Main agent file is under 150 lines
- [ ] Clear workflow with 3-5 numbered steps
- [ ] No duplicated instructions across files
- [ ] Single clear purpose in description
- [ ] Only necessary tools listed
- [ ] Strategy files referenced, not embedded

**Behavioral indicators**:
- [ ] Agent follows workflow consistently
- [ ] Handoffs work reliably
- [ ] Tool invocations are appropriate
- [ ] Output format is predictable
- [ ] Error handling is clear

---

## Common Anti-Patterns to Avoid

### ❌ The Everything Agent
- **Problem**: Agent tries to do too many unrelated things
- **Solution**: Split into focused agents with handoffs

### ❌ The Duplication Monster
- **Problem**: Same instructions repeated across multiple files (200+ lines duplicated)
- **Solution**: Extract to shared documents, use references

### ❌ The Verbose Manual
- **Problem**: Agent file is 800+ lines with examples, pitfalls, and repeated sections
- **Solution**: Use shell + strategy pattern, move details to resources

### ❌ The Tool Collector
- **Problem**: Agent lists 20+ tools but uses 3
- **Solution**: Include only actually-used tools

### ❌ The Variable Interpolation Trap
- **Problem**: Handoff fails because variables aren't populated correctly
- **Solution**: Use protocol blocks in conversation history

### ❌ The Implicit Assumption
- **Problem**: Receiving agent doesn't know what to look for in handoff context
- **Solution**: Add explicit parsing instructions ("search for X, extract Y")

---

## Iteration Strategy

### 12. How to Refactor Existing Agents

**Step 1: Measure current state**
- Count lines in agent file
- Identify duplicated sections (exact or near-identical text)
- List tools declared vs. actually used
- Check if purpose is clear and singular

**Step 2: Extract to strategies**
- Move detailed implementation to strategy files
- Keep only workflow + principles in agent file
- Use references instead of embedding

**Step 3: Eliminate duplication**
- Find repeated content (common mistakes, examples, etc.)
- Decide: Is this cross-cutting? → Move to agent file
- Decide: Is this strategy-specific? → Keep in one strategy only
- Remove all other copies

**Step 4: Validate**
- Test agent follows workflow
- Verify handoffs work
- Check tool invocations are appropriate
- Measure: Did line count drop 75-90%?

---

## Testing and Validation

### 13. How to Test Agent Handoffs

**Create a test harness agent**:
- Minimal agent that accepts protocol paste
- Immediately suggests handoff
- No other logic

**Validation checklist**:
- [ ] Protocol appears in conversation history
- [ ] Receiving agent finds protocol
- [ ] Receiving agent extracts all fields correctly
- [ ] Receiving agent acts on parsed context
- [ ] User can see what's being passed

---

## Summary: The Ideal Agent

A well-designed agent is:
- **Concise**: 50-150 lines in main file
- **Focused**: Single clear responsibility
- **Structured**: Clear workflow steps
- **Modular**: Strategies in separate files
- **DRY**: No duplicated content
- **Explicit**: Clear instructions for all phases
- **Protocol-based**: Uses conversation history for handoffs
- **Minimal**: Only necessary tools included

**Golden Rule**: If you can't explain what the agent does in one sentence, it's trying to do too much.
