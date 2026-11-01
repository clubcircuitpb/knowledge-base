# ClubCircuit help center — instructions for agents and editors

This repo is the public help center for **ClubCircuit**, an interclub pickleball league platform.
Mintlify publishes it from `main`. It is **public**: anyone on GitHub can read it.

## Who it's for

| Tab | Folder | Reader |
|---|---|---|
| Players | `players/` | Anyone who plays in a ClubCircuit season |
| Captains | `captains/` | Team captains |
| Running a club | `clubs/` | Club admins and club owners. Pages only owners can act on carry `tag: "Owners only"` |
| League rules | `rules/` | Everyone. One rulebook per league style in `rules/<style>/` (the first is `rules/rally-21/`), plus the overview at `rules/index` |

League (market) admin and platform admin documentation does **not** belong here. It lives outside
Mintlify.

## Where the truth is

The app is the only source. It lives in the private repo `clubcircuitpb/leagues-app`.

- **Which article covers which screen:** `src/lib/help-center/kb-map.ts` in leagues-app. Every
  article here has an entry there. When you publish an article, set its `status` to `published`
  in that map, in a leagues-app branch.
- **UI labels:** copy them verbatim from the app's code (`src/app/**`, `src/components/**`).
- **Behaviour:** the screen specs in `docs/04-screens.md`, the rules in
  `docs/02-rules-and-eligibility.md` and `docs/03-match-format-and-lineups.md`, and the decisions
  in `docs/adr/`. When code and docs disagree, the code wins for labels and the newest ADR wins for
  behaviour. Flag every conflict to Larry.
- Pages that were in this repo before 1 November 2026 were not written from the app. Never reuse
  them as a source.

## Writing rules

Use the **kb-article-writer** skill for every article: answer first, task-based title, 5–7 steps,
a **Why isn't this working?** section, and a source for every step. On top of that:

1. **Put a hidden source note on every step**: `{/* src: src/app/match/[matchId]/lineup/page.tsx */}`.
   End each article with `{/* Last verified: <date> against leagues-app <short sha> */}`.
2. **Never quote a rule's value in a how-to article.** Deadlines, rating caps, roster limits and
   fees come from each season's `Ruleset` and differ by season. Explain how the rule works and link
   to the league's rulebook page. No "24 hours", no "4.0 cap".
   **The exception is the rulebook pages in `rules/<style>/`.** They *are* the published rules, so
   they state the values. Their source is Larry's rulebook, a Claude Docs document, not the code.
   Copy its wording and numbers faithfully; don't reword a rule or "fix" a number. Every value must
   also match what the app enforces for that season, so a change to the season's rules in the app
   and a change to its rulebook page ship together. Where the app and the rulebook disagree, flag
   it to Larry; don't pick one yourself.
3. **Rounds and courts, never game numbers.** "Round 3, Court 2", "You're playing rounds 1, 3 and 6".
4. **Write for one-handed phone use.** Players read this at a court. Short steps, no tables wider
   than a phone.
5. **No member data and nothing private.** No real names, emails or screenshots of real accounts,
   and nothing from leagues-app's `docs/14-owner-checklist.md`. Use the sandbox's fictional people
   if you need an example.
6. **Brand:** "ClubCircuit", one word. Support: `support@clubcircuitpb.com`. App:
   `https://app.clubcircuitpb.com`.
7. Mintlify conventions: sentence-case headings, root-relative links without `.mdx`, alt text on
   every image, and add every new page to `docs.json` (never list a page that doesn't exist yet).

## Branches and publishing

- Work on a branch named after the leagues-app branch that caused the change, or `kb-sync/<date>`
  for the weekly sync. Run `mint validate` and `mint broken-links` before you push.
- **Never merge to `main` yourself.** Production for the app changes only when Larry promotes a
  release; he merges help-center pull requests at the same time, so the help center never
  describes a feature members can't use yet.
- When you move or delete a published page, add a redirect in `docs.json`. The paths of the
  pre-November starter pages already redirect to `/`; don't reuse those exact paths.

## The weekly sync

`.kb-sync/last-checked` holds the leagues-app `main` commit the help center was last checked
against. The weekly sync runs `pnpm kb:impact <last-checked>..origin/main` in leagues-app,
re-verifies or writes what it flags, bumps this file, and opens one pull request here. It opens
one even when nothing needs changing, so Larry can see the check happened.
