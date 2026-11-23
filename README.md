# Create ClickUp Checklists Plugin for Claude Code

A Claude Code plugin that teaches Claude to create ClickUp checklists and checklist items through the ClickUp REST API. The ClickUp MCP can't create checklists.

## Installation

```
/plugin marketplace add Breakstep-Studios/claude-plugins
/plugin install create-clickup-checklists@breakstep-studios
```

## Prerequisites

A ClickUp OAuth token saved in `~/.clickup/token`. An OAuth app lets you limit access to the workspaces you choose.

1. In ClickUp, go to Settings → Apps → Create new app. Set the redirect URL to `https://localhost/clickup-link-callback`.
2. Open `https://app.clickup.com/api?client_id=<CLIENT_ID>&redirect_uri=https%3A%2F%2Flocalhost%2Fclickup-link-callback` and approve only the workspaces the agent should reach.
3. The browser lands on "site can't be reached", which is expected. Copy the value after `code=` (it expires quickly).
4. Trade the code for a token and save it without printing it (PowerShell):

```powershell
$clientId = Read-Host "Client ID"; $clientSecret = Read-Host "Client Secret"; $code = Read-Host "Code"
$response = Invoke-RestMethod -Method Post "https://api.clickup.com/api/v2/oauth/token?client_id=$clientId&client_secret=$clientSecret&code=$code"
New-Item -ItemType Directory -Force "$HOME\.clickup" | Out-Null
Set-Content -NoNewline -Encoding ascii "$HOME\.clickup\token" $response.access_token
icacls "$HOME\.clickup\token" /inheritance:r /grant:r "${env:USERNAME}:F"
```

## Usage

Ask Claude to add a checklist to a ClickUp task, e.g. "add these AC's as a checklist on ticket 868mn2umg".
