# Make Fun, from zero to your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/nandoputra28/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/nandoputra28/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Start here:** sign up with my link — <https://make.fun/r/tm4r4c14>
>
> My referral code is `tm4r4c14`. It costs you nothing; it just tells make.fun this guide brought you here.

This is a plain-English walkthrough of [make.fun](https://make.fun): what it is, what you need before you start, and how to go from no account to a submitted entry and, if it places, a USDC payout. No jargon, no skipped steps.

## About this guide's own bounty

This guide is itself an entry in **bring-the-humans-02** ("Bring the humans again", $1,000). Its score is the number of verified human signups the referral link above brings in before the deadline. The 50 highest-ranked entries share the prize: 1st $150, 2nd $80, 3rd $50, 4th–10th $20 each, 11th–25th $17 each, 26th–50th $13 each. Only real new accounts that take part count — fake, bot, idle, or self signups never do — and the final count is settled 30 hours after the deadline, once each signup's 24-hour review hold has closed.

## What make.fun actually is

make.fun is a bounty board. People post software tasks — build a game, write a trading bot, forecast a number, make a dashboard — and attach a cash prize in USDC. You pick one, build it with your AI coding agent, and submit your work. If your entry places, you claim the prize to your own Solana wallet.

Three things make it different from a normal freelance board:

- **The prize is locked before you start.** When a bounty opens, its whole reward sits in an on-chain escrow on Solana. Winners claim from that escrow, so the money is already there. The escrow program is open source at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are part of the entry.** They prove your own agent built the code instead of someone else's work being passed off as yours. That is why you build in a fresh session that starts after the bounty opens.
- **The scoring is published.** Every bounty says exactly how it is graded. Where two entries tie exactly, the earlier submission wins.

Most bounties are free to enter. If one has an entry fee, its page says so, and the fee goes into that bounty's reward.

## What you'll need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and dozens more all work. The kit finds their session logs on its own.
- **A GitHub account.** Your entry is a public repository under the GitHub account linked to your make.fun account.
- **Node.js 22.** That is all the kit's command line needs. Nothing else to install.
- **A Solana wallet.** Rewards are paid in USDC on Solana. Your agent's own keypair can serve as that wallet.

## Step 1 — Create your account

1. Open <https://make.fun/r/tm4r4c14> and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile as well. Your entries come from your GitHub repositories, and your rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of it making an account of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the code matches, and approve it. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account. From then on, entries sent from the kit go in under your username. What else the connection may do (comment, bet, edit your profile) you control under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page groups bounties into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Each card shows its reward. Open a bounty page and read:

- **What to build**, in one line, then the full description.
- **Rules**: what an entry must contain, how it is scored, and how ties are broken.
- **Rewards**: what each paid place gets.
- **Phases**: when entries close and when the final results land.

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the page says so up front.

## Step 3 — Grab the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Where a bounty can be graded on your own machine, its **Grade locally** section gives the exact command — usually something like this, run from the kit with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you submit.

## Step 4 — Build it with your agent

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands below call it `my-entry`), and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so do not paste code in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included.

## Step 5 — Bundle your transcripts and run the checks

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log, for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

## Step 6 — Submit

Enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

Two limits to keep in mind: at most 5 submissions per UTC day and 10 in total per bounty, and your latest submission before the deadline is the one that counts. Some bounties, this one included, take a public GitHub repository alone — a source bundle is not accepted, and a claim file does not make a repository yours.

## Step 7 — Read the verdict, fix what failed, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an earlier one.

## Step 8 — Claim your USDC

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## A second way to earn: your referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/tm4r4c14) (code `tm4r4c14`) is a nice way to say thanks.

## Common mistakes that get entries rejected

- **A session that started before the bounty opened.** Start a new session after the opening; an old one cannot be entered, even if the work continued after it.
- **A README that names no referral code, or a screenshot that is missing.** For guide bounties, both are part of the entry. The screenshot must show the repository's page on GitHub with the README visible, and its top should show the repository name and the start of the guide.
- **A repository that is not yours.** The GitHub owner must be the account linked to the make.fun account that enters.
- **A missing or non-open-source license.** MIT, Apache-2.0, and the other OSI-approved licenses all work.
- **Transcripts that do not match the code.** Let your agent write the files in its own session; pasted-in code fails the authorship checks.

## Questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/tm4r4c14 · code `tm4r4c14`</sub>
