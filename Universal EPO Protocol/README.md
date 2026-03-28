# Universal EPO Protocol

**Version:** 2.4  
**Status:** System Prompt Feature  
**Scope:** OpenClaw Agent Orchestration

---

## Overview

The Universal EPO Protocol is an additive system upgrade for OpenClaw that introduces a toggleable **background daemon mode**. When active, the agent can autonomously execute a defined mission — waking at scheduled intervals, processing state, and reporting progress — without requiring continuous user input.

EPO stands for **Extended Persistent Orchestration**.

This protocol does not override or modify any existing rules, permissions, or configurations defined in `AGENTS.md` or the current session context. It is strictly additive. Where any conflict exists between EPO behaviour and user-defined rules, **user-defined rules take precedence.**

---

## How It Works

By default, the agent operates as a **Reactive Agent** — responding to prompts, taking no autonomous background action. EPO adds a second mode:

```
REACTIVE MODE (default)  ──[EPO-ON]──▶  DAEMON MODE  ──[EPO-OFF]──▶  REACTIVE MODE
```

In Daemon Mode, the agent:
- Reads the mission brief on every scheduled wake
- Executes the next logical task using `session_spawn`
- Updates mission state to disk
- Sends optional progress notifications to your configured channel
- Automatically shuts down when the persistence duration elapses

---

## Quick Start

### 1. Install the Prompt

Copy the contents of `EPO_PROTOCOL_v2.4.md` into your OpenClaw system prompt, or append it to your existing `AGENTS.md` via the OpenClaw prompt management interface.

### 2. Activate

In any session, type:

```
EPO-ON
```

The agent will run through a six-step configuration wizard, then begin background orchestration.

### 3. Deactivate

```
EPO-OFF
```

The agent will gracefully close all background sessions, archive the mission, and send a Final Mission Report.

---

## Configuration Wizard (EPO-ON)

The wizard collects six pieces of configuration before activating daemon mode.

| Step | Parameter | Description |
|------|-----------|-------------|
| 1 | **Mission Name** | Free text — identifies the project or task |
| 2 | **Objectives** | List of primary goals or "Next Moves" |
| 3 | **Persistence Duration** | How long daemon mode stays active |
| 4 | **Wake Interval** | How often the agent checks mission state |
| 5 | **Notifications** | Enable/disable outbound status pings |
| 6 | **Notify Frequency** | How often pings are sent (if enabled) |

**Persistence options:** Until Finished / 2 Hours / 8 Hours / Custom  
**Wake interval options:** 5 mins / 15 mins / 30 mins / Custom  
**Notify frequency options:** Every Heartbeat / Milestones & Blockers Only / Every 60 mins / Custom

Selecting **"Something Else"** at any step pauses the wizard and waits for free-text input before continuing.

### Platform UI Behaviour

The wizard adapts its UI to your connected channel:

- **Telegram / Slack / Discord** — options are presented as clickable buttons via `openclaw message send --buttons`
- **TUI / WhatsApp / Signal** — options are presented as a numbered list

---

## File Structure

All EPO state is isolated to a single folder:

```
/Universal_EPO_Protocol/
├── EPO_CONFIG.json       # Wizard output and runtime configuration
├── MISSION.md            # Live mission brief, updated on every wake
└── ARCHIVE/              # Completed mission logs
```

No files are written outside this directory.

---

## Background Behaviour

On every scheduled wake, the agent follows this sequence:

1. Read `MISSION.md` to determine the next action
2. Execute the next task via `session_spawn` (keeps orchestrator context clean)
3. Write updated mission state back to `MISSION.md`
4. Check whether the persistence duration has elapsed — auto-trigger `EPO-OFF` if so
5. Send a status ping to `NOTIFY_TARGET` if notification conditions are met

---

## EPO-OFF Sequence

Whether triggered manually or automatically on timeout, EPO-OFF:

1. Completes or gracefully pauses any active sub-task
2. Terminates all EPO-registered cron tasks
3. Terminates all EPO `session_spawn` background sessions
4. Finalises and archives `MISSION.md` to `/ARCHIVE/`
5. Sends a **Final Mission Report** to `NOTIFY_TARGET` containing:
   - Mission name and total active duration
   - Objectives: completed / in-progress / blocked
   - Notable outputs and file paths created
6. Confirms deactivation and returns to Reactive Mode

---

## Notifications

The agent extracts `channel` and `user_id` from session metadata at the start of EPO-ON and uses these as the `NOTIFY_TARGET` throughout the mission. You do not need to configure this manually.

Notification content varies by frequency setting:

| Setting | What's sent |
|---------|-------------|
| Every Heartbeat | A brief status update on every wake cycle |
| Milestones & Blockers Only | Pings only when an objective is completed or the agent is stuck |
| Every 60 mins | A summary ping regardless of wake interval |

---

## Important Notes

- **Non-destructive by design.** EPO does not modify `AGENTS.md`, existing cron tasks, or any configuration outside `/Universal_EPO_Protocol/`.
- **session_spawn is required.** Sub-tasks are run in separate sessions to keep the orchestrator context clean. Ensure `session_spawn` is available in your OpenClaw environment.
- **Mission continuity.** If a session is interrupted, the agent will re-read `MISSION.md` on the next wake and continue from the last recorded state.
- **Custom intervals.** Any step accepting "Something Else" pauses the wizard for free-text input — useful for non-standard durations or intervals.

---

## Version History

| Version | Notes |
|---------|-------|
| 2.0 | Initial release — EPO-ON wizard, basic background loop, notification support |
| 2.3 | Added scoped authorisation model, folder isolation, MISSION.md read pattern |
| 2.4 | Restored UI platform logic and "Something Else" handler; clarified non-override principle; added conditional Step 6; structured Final Mission Report |

---

## License

Part of the OpenClaw agent configuration ecosystem. Internal use.
