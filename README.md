# 🛸 Universal EPO Protocol (v2.1)
### Enhanced Persistent Orchestration for OpenClaw

The **Universal EPO Protocol** is a behavioural skill layer for OpenClaw agents. It transforms a standard reactive AI into a **proactive mission orchestrator** capable of managing long-running tasks, background "heartbeats," and cross-channel notifications (Telegram, Slack, Discord) without losing context.

---

## 🧠 Core Philosophy
Standard LLM agents are reactive—they wait for you to speak. **EPO** changes the dynamic:
* **Persistence**: The agent "wakes itself up" using Cron to check on tasks.
* **Context Safety**: Uses `session_spawn` to prevent "context poisoning" in long sessions.
* **State Management**: Centralises all mission data in a dedicated folder structure.
* **User Guardrails**: An interactive wizard ensures the agent knows exactly when to bug you and when to stay quiet.

---

## 🛠️ Installation
To "flash" this skill into your OpenClaw instance:
1.  Copy the **Setup Prompt** found in `prompt.md` in this repo.
2.  Paste it into your OpenClaw chat (TUI or Mobile).
3.  The agent will automatically create the `/Universal_EPO_Protocol/` directory and initialise the system.

---

## ⌨️ Command Reference

| Command | Action | Output |
| :--- | :--- | :--- |
| `EPO-ON` | Triggers the Interactive Setup Wizard. | Starts background orchestration. |
| `EPO-OFF` | Kills active Cron tasks and archives the mission. | Final Mission Report. |

---

## 🚀 The EPO-ON Wizard
When you run `EPO-ON`, the agent initiates a 6-step handshake:
1.  **Mission Branding**: Define the project name.
2.  **Task Boarding**: Define the initial "Next Moves."
3.  **Persistence**: Set a "Dead Man's Switch" (e.g., run for 4 hours then auto-kill).
4.  **Cron Interval**: Set how often the agent "thinks" (e.g., every 15 mins).
5.  **Channel Sync**: Automatically detects if you're on Telegram, Slack, etc.
6.  **Notification Logic**: Choose to see "Everything" or just "Milestones & Blockers."

> [!TIP]
> On supported platforms like **Telegram**, the wizard uses **Inline Buttons** for a native app-like experience.

---

## 📁 Directory Structure
Once initialised, the protocol maintains itself in:
```text
/Universal_EPO_Protocol/
├── EPO_RULES.md       # The "SOP" the agent follows.
├── EPO_CONFIG.json    # Your active session settings.
├── MISSION.md         # The live "Brain" of the current task.
└── /ARCHIVES/         # Completed mission logs.
```

---

## 🤝 Contributing
This was developed to solve the "context drift" and "reactive dormancy" issues in OpenClaw 2026.3. If you find a way to harden the logic further, feel free to submit a PR!

