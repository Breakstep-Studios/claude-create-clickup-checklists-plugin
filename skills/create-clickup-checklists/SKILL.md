---
name: create-clickup-checklists
description: Create checklists and checklist items on a ClickUp task through the ClickUp REST API. Use when asked to add a checklist (e.g. AC's or RAID's) or checklist items to a ClickUp task, since the ClickUp MCP can't create checklists.
---

# Create ClickUp Checklists

**The ClickUp MCP can't create checklists.** Use the REST API with the token in `~/.clickup/token`. Never print the token.

```bash
TOKEN=$(cat ~/.clickup/token); API=https://api.clickup.com/api/v2

# create the checklist; the response has checklist.id
curl -s -X POST "$API/task/<task_id>/checklist" -H "Authorization: $TOKEN" -H "Content-Type: application/json" -d '{"name":"<checklist name>"}'

# add one item per call, in order
curl -s -X POST "$API/checklist/<checklist_id>/checklist_item" -H "Authorization: $TOKEN" -H "Content-Type: application/json" -d '{"name":"<item>"}'
```

If `~/.clickup/token` is missing, tell the user to set it up (see the plugin README) instead of asking for the token.
