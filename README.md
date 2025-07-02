# Cursor-Rules

A compilation of all the rules that I'm going to be using in future.


### How rules work

- Large language models do not retain memory between completions. 

- Rules solve this by providing persistent, reusable context at the prompt level.

- When a rule is applied, its contents are included at the start of the model context. 
  
- This gives the AI consistent guidance whether it is generating code, interpreting edits, or helping with a workflow.

## Types of rules : 

### 1. User rules (Global Rules) :

- Global rules(Rules for AI) is global. It's carried over to every project you do with cursor.
  
- User rules are defined in Cursor `Settings` > `Rules`.

- They apply to all projects and are always included in your model context.

- Use them to:
  - Set response language or tone
  - Add personal style preferences
  
**Example** :

```
Please reply in a concise style. Avoid unnecessary repetition or filler language.
```

**NOTE** : User rules do not support MDC, they are plain text only.

### 2. Project rules :

- Project rules live in `.cursor/rules`. 
  
- Each rule is stored as a file and version-controlled. 
  
- They can be scoped using path patterns, invoked manually, or included based on relevance. 
  
- Subdirectories can include their own `.cursor/rules` directory scoped to that folder.

- Use project rules to:
    - Encode domain-specific knowledge about your codebase.
    - Automate project-specific workflows or templates.
    - Standardize style or architecture decisions.

## Rule type

Each rule file is written in MDC (.mdc), a lightweight format that supports metadata and content in a single file. Rules supports the following types:

<table>
    <tr>
        <td>Rule Type</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>Always</td>
        <td>Always included in the model context</td>
    </tr>
    <tr>
        <td>Auto Attached</td>
        <td>Included when files matching a glob pattern are referenced</td>
    </tr>
    <tr>
        <td>Agent Requested	</td>
        <td>Rule is available to the AI, which decides whether to include it. Must provide a description</td>
    </tr>
    <tr>
        <td>Manual</td>
        <td>Only included when explicitly mentioned using @ruleName</td>
    </tr>
</table>

#### Example MDC rule : 

```mdc
---
description: RPC Service boilerplate
globs: 
alwaysApply: false
---

- Use our internal RPC pattern when defining services
- Always use snake_case for service names.

@service-template.ts
```

### Nested rules :

You can organize rules by placing them in .cursor/rules directories throughout your project structure. For example :

```mdc
project/
  .cursor/rules/        # Project-wide rules
  backend/
    server/
      .cursor/rules/    # Backend-specific rules
  frontend/
    .cursor/rules/      # Frontend-specific rules
```

Nested rules are:

- Automatically attached when files in their directory are referenced.
- Still available in the context picker and agent-accessible rules list.
- Perfect for organizing domain-specific rules closer to their relevant code.

This is particularly useful in monorepos or projects with distinct components that need their own specific guidance.