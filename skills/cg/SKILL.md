---
name: cg
description: Use when the person wants to be called, texted or WhatsApped when a job finishes, wants CG to phone a person or a business for them, or says "set up CG" / "connect CG" / "connect my phone". Covers the one-time set-up (cg_signup, a code to their mobile, cg_verify, one claude mcp add) and how to use the cg tools without ever claiming a call or text that did not happen.
---

# CG: a phone for this person's Claude

CG (callcg.com) gives you a phone. Through the `cg` MCP server you can ring or text the
person's own mobile, and have CG phone people and businesses for them: it dials, presses
menu keys, waits on hold, talks to the person who answers and reports back.

## Set-up (once)

Only the `cg-start` server is present until this is done. It has two tools.

1. Ask for their mobile number **with the country code**. CG works for US and UK mobiles
   with a SIM. Never guess or reuse a number from the codebase.
2. Call `cg_signup(phone)`. A 6-digit code goes to that phone (text message in the US,
   WhatsApp in the UK). Tell them to read it to you.
3. Call `cg_verify(phone, code)`. The answer holds their **private CG link** and the exact
   command. Run it:
   `claude mcp add --transport http --scope user cg <link>`
   If you added `cg-start` yourself with `claude mcp add`, remove it
   (`claude mcp remove -s user cg-start`); the plugin's copy can stay.
4. Tell them to restart Claude Code so the `cg` tools load. Say they are connected only
   after `cg_verify` answered and the add command succeeded.

The link is the key to their account. Do not paste it into files, commits or chat logs
beyond the command itself. If it leaks they text CG "revoke my AI link".

## "Call me / text me / WhatsApp me when it's done"

Finish the job first. Then, once:

- **"call me"**: `call_me(reason, say, link)`. `say` is one short spoken sentence
  (under 140 characters, no links or numbers), e.g. "The test suite is green and the
  deploy finished." `reason` is texted to them with any `link`.
- **"text me"**: `text_me(message, via="sms")`.
- **"WhatsApp me"**: `text_me(message, via="whatsapp")`.
- Channel not named: `text_me(message)` uses the channel they use with CG.

Report what the tool result says and nothing more: "CG is ringing your phone now" is true
only when the result starts with "calling". A result starting with "not" means it did not
happen; say so and why.

## Having CG phone someone for them

`call_someone(who, kind, job, said, ...)` queues a call; `said` must be the person's own
words asking for it. CG rings people in **their contacts** and **listed businesses**. For a
number that is neither, the result asks who it is: a name saves them as a contact, a
business name lets CG find its listed number. Up to 10 calls a day to personal numbers;
businesses and their own phone are not counted. A new contact saved by you needs their
YES on their phone; the call itself is confirmed with a text that shows the number.
`call_status` tells you how it went; nothing has happened until it says so.

`start_errand` finds local businesses for a job; `approve_providers` (their words) lets CG
ring the ones they pick for prices and times.

## Never

- Never say a call, text or reminder happened unless the tool result says it did.
- Never read a tool result's link or number aloud into a commit, file or log.
- Never call `call_me` or `text_me` in a loop; once per finished job.
