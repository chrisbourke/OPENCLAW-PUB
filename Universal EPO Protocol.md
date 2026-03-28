System Upgrade: Universal EPO Protocol (v2.1 - Folder Isolation)

You are now being upgraded with the Universal EPO protocol. You must house all logic and state for this protocol within a dedicated directory: /Universal_EPO_Protocol/.

1. Skill Initialisation:
Create the directory /Universal_EPO_Protocol/ if it does not exist. This folder will be your "Command Center" for the following files:

EPO_RULES.md: A copy of these instructions for your internal reference.

EPO_CONFIG.json: Active session settings (Persistence, Intervals, Targets).

MISSION.md: The live state machine and task list.

2. Command: EPO-ON (The Interactive Wizard)
When triggered, extract the channel and user_id from metadata as the NOTIFY_TARGET.

The Configuration Dialog:

Step 1 (Mission): "Which project or mission are we initialising?"

Step 2 (Tasks): "List the primary objectives for this mission."

Step 3 (Persistence): "How long should I remain active?" [Until Finished, 2h, 8h, Something Else]

Step 4 (Cron Interval): "How often should I wake up to process state?" [5m, 15m, 30m, Something Else]

Step 5 (Notifications): "Enable outbound status pings?" [On, Off]

Step 6 (Notify Frequency): "How often should I ping you?" [Every Heartbeat, Milestones Only, 60m, Something Else]

3. Execution & Monitoring:

Save all selections to /Universal_EPO_Protocol/EPO_CONFIG.json.

Set a cron job to wake you up based on Step 4.

On every "Wake Up," you must read /Universal_EPO_Protocol/MISSION.md to determine your next action.

If the "Persistence" limit is reached, or the mission is marked "Complete" in MISSION.md, auto-trigger EPO-OFF.

4. Command: EPO-OFF

Kill all associated cron tasks.

Archive the current MISSION.md into /Universal_EPO_Protocol/ARCHIVES/.

Send a final "Mission Deployment Ended" report to the NOTIFY_TARGET.

Confirm once you have created the directory structure and filed these rules into /Universal_EPO_Protocol/EPO_RULES.md.