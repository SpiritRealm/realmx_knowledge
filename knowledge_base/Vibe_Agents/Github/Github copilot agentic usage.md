
### Step 1: The Setup & Folder Structure

Instead of `.claude/agents/`, GitHub Copilot looks for its custom agents in a `.github/agents/` folder at the root of your repository.

Copilot uses `.agent.md` files to define personas. To make agents work together, you create a "Lead/Coordinator" agent and give it explicit permission to invoke other agents as tools.

### Step 2: Create the Subagents (The Workers)

Create these two files in your `.github/agents/` folder. Notice the YAML frontmatter at the top—this tells Copilot what tools the agent is allowed to use.

**1. `ui-designer.agent.md`**

Markdown

```
---
name: Designer
description: Generates HTML structure and Tailwind CSS styling.
tools: ['editFiles', 'terminalLastCommand']
---
You are an expert UI/UX Designer.
- Focus strictly on HTML/JSX structure and Tailwind CSS classes.
- Make minimal, focused edits. Do not write complex logic.
```

**2. `frontend-dev.agent.md`**

Markdown

```
---
name: Developer
description: Writes JavaScript/React logic and interactivity.
tools: ['editFiles', 'terminalLastCommand']
---
You are a Senior Frontend Engineer.
- Wire up interactive elements like smooth scrolling and dark mode.
- Respect the styling choices made by the designer.
```

### Step 3: Create the Lead Agent (The Orchestrator)

Now, you create the boss. The key here is the `agents: []` array in the YAML frontmatter and adding `'agent'` to its tools. This explicitly gives this agent the power to spawn the subagents you just created.

**Create `lead-architect.agent.md`:**

Markdown

```
---
name: Lead Architect
description: Coordinates the Designer and Developer to build full features.
tools: ['agent']
agents: ['Designer', 'Developer']
---
You are the Lead Architect. For each task you receive:
1. Use the `Designer` agent to scaffold the UI and styling.
2. Once complete, use the `Developer` agent to add the logic and interactivity.
Coordinate these handoffs carefully and ensure the final code works together.
```

### Step 4: Execution & Monitoring (Mission Control)

Once your files are saved, here is how you launch the swarm in VS Code:

1. Open the **Copilot Chat View** in VS Code.
    
2. Type `@` in the chat box, and you will see your new custom agents appear in the dropdown.
    
3. Select `@Lead Architect` and give it your prompt: _"Build a Bento Grid portfolio with a dark mode toggle."_
    

**How it handles multi-tasking (The "Agent Sessions" View):**

This is where GitHub Copilot currently outshines Claude Code's terminal output.

GitHub recently introduced the **Agent Sessions View** (you can open this from the VS Code sidebar). When your Lead Architect spawns the Designer and Developer, they show up here as separate, trackable background tasks.

If you want them working on completely different features simultaneously without stepping on each other's toes, Copilot actually spins up **Git Worktrees** in the background. This means the UI designer can be editing `index.html` in one isolated folder instance, while the developer works on `script.js` in another, preventing merge conflicts while they work in parallel!

### Copilot vs. Claude Code (The OpenRouter Factor)

There is one catch based on your previous setup: **GitHub Copilot does not allow you to easily hijack its API requests to route them through OpenRouter's free models.** While Copilot now lets you choose between GPT-5 Codex, Claude 4.5 Sonnet, and Gemini 3 Pro, these are tied directly to your paid GitHub Copilot Pro/Enterprise subscription.

