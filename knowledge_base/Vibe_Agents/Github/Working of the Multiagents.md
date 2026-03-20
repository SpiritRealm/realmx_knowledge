
### 1. How It Works Internally

When you launch a multi-agent team in GitHub Copilot, it operates differently than Claude Code running strictly in your local terminal.

- **The Orchestrator:** Your "Lead Architect" agent is powered by a massive backend system prompt. It takes your natural language request, maps your entire workspace context, and uses a tool called `agents` to spawn the subagents.
    
- **Isolated Environments:** If you use the autonomous "Copilot coding agent" command (`/delegate`), Copilot doesn't just edit your live files. It actually spins up an isolated, background development environment.
    
- **The Agentic Loop:** The Lead Agent assigns the HTML to the Designer and the JS to the Developer. They execute tool calls (like `editFiles` and `terminalLastCommand`) in that isolated environment, test the code, and fix syntax errors autonomously.
    
- **The Pull Request:** Once the team finishes building the feature, the Lead Agent bundles all their work and opens a draft Pull Request in your repository for you to review, ensuring your main branch is never broken by an AI hallucination.
    

### 2. Billing & Quota Costs

Because these agents operate on GitHub's cloud and use heavy models, they chew through resources quickly. Copilot measures agent usage in **Premium Requests**.

- **The Allowances:** * **Copilot Free:** 50 Premium Requests / month.
    
    - **Copilot Pro ($10/mo):** 300 Premium Requests / month.
        
    - **Copilot Pro+ ($39/mo):** 1,500 Premium Requests / month.
        
- **The Cost Multiplier:** Different models have different "Premium Request Multipliers." If you assign a lightweight model to a task, it might consume 1 request per turn. If you assign a heavy reasoner like GPT-5.4 or Claude Opus 4.6, it will consume a higher multiple of your quota per interaction.
    
- **The Multi-Agent Drain:** When your Lead Architect talks to your Designer, and the Designer writes code, and then hands it to the Developer—**every single tool call and handoff costs requests.** An autonomous session that takes 20 steps to build your Bento Grid will consume 20+ Premium Requests.
    
- **Overage:** If you run out of your monthly allowance on a Pro/Pro+ plan, Copilot charges **$0.04 per additional premium request**.
    

### 3. Assigning Specific Models (Yes, you can!)

You absolutely can map specific models to specific agents! GitHub currently supports a massive roster, including OpenAI's GPT-5.4, Anthropic's Claude 4.6, and Google's Gemini 3.1 Pro (which is the core model powering me!).

To do this, you just add the `model` property to the YAML frontmatter of your `.agent.md` files.

**For the UI Designer (Using Gemini for speed and massive context):**

YAML

```
---
name: UI Designer
description: Generates HTML structure and Tailwind CSS styling.
tools: ['editFiles']
model: gemini-3.1-pro
---
```

**For the Backend/Logic Developer (Using GPT-5.4 for heavy logic):**

YAML

```
---
name: Developer
description: Writes JavaScript/React logic and interactivity.
tools: ['editFiles', 'terminalLastCommand']
model: gpt-5.4
---
```

When the Lead Agent orchestrates the task, GitHub Copilot will automatically route the prompts to the specific models you assigned to those personas.

