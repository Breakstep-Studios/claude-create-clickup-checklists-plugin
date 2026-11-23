# Create ClickUp Checklists Plugin for Claude Code

**The ClickUp MCP can't create checklists.** This plugin exists to fix that: it teaches Claude to create ClickUp checklists and checklist items (e.g. AC's and RAID's) through the ClickUp API instead.

## Installation

Paste these into Claude Code:

```
/plugin marketplace add Breakstep-Studios/claude-plugins
/plugin install create-clickup-checklists@breakstep-studios
```

## Where the token lives

Claude needs a ClickUp token to talk to the ClickUp API. It reads it from a file in your home folder:

```
~/.clickup/token
```

For example, if your Windows user is `John`:

```
C:\Users\John\.clickup\token
```

The file holds one line: the token. You only set it up once (see below).

## One-time setup: create the token

You'll create a small ClickUp "app" and approve it. This lets you choose which workspaces Claude can reach.

**Step 1. Create the app in ClickUp**

1. In ClickUp, click your avatar → **Settings** → **Apps** → **Create new app**.
2. Give it any name, e.g. `Claude`.
3. Set the redirect URL to:
   ```
   https://localhost/clickup-link-callback
   ```
   ClickUp will then show just `https://localhost`. That's normal.
4. Leave this page open. You'll need the **Client ID** and **Client Secret** shown on it.

**Step 2. Approve the app**

Open this link in your browser, with `<CLIENT_ID>` replaced by your Client ID:

```
https://app.clickup.com/api?client_id=<CLIENT_ID>&redirect_uri=https%3A%2F%2Flocalhost%2Fclickup-link-callback
```

Pick **only** the workspaces Claude should be able to reach, then approve.

**Step 3. Copy the code**

Your browser now shows **"This site can't be reached"**. That means it worked.

Copy the part of the address after `code=`. For example, from:

```
https://localhost/clickup-link-callback?code=ABC123XYZ
```

copy `ABC123XYZ`. The code only works once and expires after a few minutes, so do step 4 straight away.

**Step 4. Save the token**

Open **PowerShell**, paste the code below and press Enter. It asks for three things: paste your **Client ID**, **Client Secret** and the **code** from step 3 when prompted.

```powershell
$clientId = Read-Host "Client ID"; $clientSecret = Read-Host "Client Secret"; $code = Read-Host "Code"
$response = Invoke-RestMethod -Method Post "https://api.clickup.com/api/v2/oauth/token?client_id=$clientId&client_secret=$clientSecret&code=$code"
New-Item -ItemType Directory -Force "$HOME\.clickup" | Out-Null
Set-Content -NoNewline -Encoding ascii "$HOME\.clickup\token" $response.access_token
icacls "$HOME\.clickup\token" /inheritance:r /grant:r "${env:USERNAME}:F"
```

This trades the code for a token, saves it to `~/.clickup/token` without showing it on screen, and locks the file so only your account can read it.

**It worked if you see:**

```
processed file: C:\Users\John\.clickup\token
Successfully processed 1 files; Failed processing 0 files
```

**If you see a red error instead**, the code expired. Go back to step 2 and do steps 2 to 4 again quickly.

## Usage

Just ask Claude, for example:

```
Add these AC's as a checklist on ClickUp ticket 868mn2umg.
```

## Keep the token safe

- Treat the token like a password. Never paste it into a chat.
- To cut Claude's access, remove the app in ClickUp under **Settings** → **Apps**.
