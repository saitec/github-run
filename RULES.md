# 📏 rules

The GitHub Run follows these. Every day. No exceptions.

## 🔒 never goes public

Strip these from every insight, skill and log line.
When unsure, leave it out.

- Money: revenue, margins, burn, runway, balances, prices, deal terms
- Fundraising, valuations, investor talks, term sheets
- Acquisitions: targets, prices, diligence
- Salaries, offers, comp, equity, cap tables
- Hiring or firing of named people
- Feedback about named people
- Legal matters
- Client names, client data, client contracts
- Anything from ShopOS internal Slack, email or docs, word for word
- Personal life: health, family, relationships, travel plans
- Passwords, keys, tokens, account numbers

Insights talk about **ideas and patterns**, never people or companies by name.
"A brand asked for 40 ad variants overnight" is fine.
"Brand X asked Y for 40 variants" is not.

## 🔧 picking a fix

Good targets:

- Tools I actually use: Claude skills, Remotion, A-Frame, FFmpeg wrappers, Playwright, MCP servers, Shopify kits
- Issues labelled `bug`, `good first issue` or `help wanted`
- Open for 7+ days, nobody assigned, no open PR already on it
- Repo merged an outside PR in the last 90 days (so it's alive)

Skip:

- Typos, formatting, dependency bumps
- Anything that needs a design call from the maintainers
- Repos whose CONTRIBUTING says no AI-assisted PRs
- Anything I can't test

## ✅ a fix is ready when

- [ ] Repo's own tests pass, plus a new test for the bug
- [ ] Diff is small and only touches what the bug needs
- [ ] PR text: what broke, why, how it's fixed, how it was tested
- [ ] PR text says "Drafted with Claude, reviewed by me"
- [ ] It's all in `fixes/YYYY-MM-DD-repo/`

## 💡 an insight is ready when

- [ ] One idea. One page. Short sentences.
- [ ] It helps someone who isn't me
- [ ] Passes the never-goes-public list
- [ ] If it's a repeatable job, it's a skill file in `skills/`, not a note

## 🚀 shipping

Nothing leaves this repo without Sai saying yes.
The routine prepares. Sai ships.
