# CG for Claude Code

**A phone for your AI.** Install this, give Claude your mobile number, read it the code that
arrives, and from then on Claude can ring or text you the moment a long job is done, and have
CG phone people and businesses for you.

```
claude plugin marketplace add 711webservices/cg-plugin
claude plugin install cg@cg-plugin
```

Then, in Claude Code: **"set up CG"**.

No plugin? One line does the same:

```
claude mcp add --transport http --scope user cg-start https://connect.callcg.com/mcp/start
```

## What happens

1. Claude asks for your mobile number with the country code (US or UK mobile with a SIM).
2. CG texts you a 6-digit code (WhatsApp in the UK). You read it to Claude.
3. Claude gets your private CG link and adds it:
   `claude mcp add --transport http --scope user cg <link>`. Restart Claude Code.

That is the whole sign-up. Your number is your account; there is no password.

## Then say things like

- "Run the full test suite and **call me** when it's done."
- "Deploy, then **text me** the summary." / "**WhatsApp me** when the build is green."
- "**Call the dentist** and move my Thursday appointment to after 2."
- "**Find three plumbers** near me and get quotes for the kitchen leak."

When the job finishes, Claude calls `call_me` (CG rings and speaks one sentence, and texts
the details) or `text_me` (by SMS or WhatsApp, as you asked). For calls to other people, CG
dials, presses menu keys, waits on hold, talks to whoever answers, and reports back.

## Rules CG keeps

- CG never claims a call or text happened unless it did.
- CG phones the people in **your** contacts and **listed businesses**. A stranger's number
  is refused until you say who it is.
- Up to **10 calls a day to personal numbers**. Businesses and your own phone are not counted.
- Every call opens with "this is CG, <your name>'s AI assistant". Anyone can say "stop
  calling me" and CG will not call them again for you.
- Your link is the key to your account. Keep it out of repos. Text CG **"revoke my AI link"**
  to cut it.

## Price

A 7-day free trial (20 minutes of calls, 50 texts, 5 errands). After that, the CG plan at
[callcg.com](https://callcg.com). US and UK only for now.

## Other agents and apps

Cursor, Codex, Gemini CLI, VS Code, Windsurf, ChatGPT and Claude.ai take the same private
link. Any MCP client can run the two set-up tools at
`https://connect.callcg.com/mcp/start`; setup snippets for each are at
[callcg.com/agents](https://callcg.com/agents).

## For your project's CLAUDE.md

```
## Reaching me
CG is connected (MCP server `cg`). When I ask to be told when a job is done: finish, then
call `call_me` with a one-sentence `say` (or `text_me`, `via` sms/whatsapp when I named
one). Never report a call or text as made unless the tool result says so.
```

## What is in this repo

- `.mcp.json`: the set-up server (`cg-start`, two tools, no key).
- `skills/cg/SKILL.md`: how Claude sets you up and uses the `cg` tools honestly.
- `.claude-plugin/`: the plugin and marketplace manifests.

The CG service itself is not in this repo. Privacy and terms: [callcg.com/privacy](https://callcg.com/privacy),
[callcg.com/terms](https://callcg.com/terms).
