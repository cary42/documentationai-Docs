# Agent instructions for this repository

This repo is the source for the Campium documentation site at
[docs.campium.com](https://docs.campium.com). documentation.ai publishes from
`main`. A push to `main` republishes the whole site, so the site is only ever as
correct as the last commit here.

The product source lives in a separate repo, `cary42/campium-all`. Nothing in
this repo is generated from it automatically. A scheduled agent reads that repo
and writes the updates here.

## Cadence

The scheduled agent runs nightly.

| Run | What it may change |
| --- | --- |
| Any night | Page copy (`*.mdx` other than `changelog.mdx`) and `documentation.json` |
| Monday only | `changelog.mdx`, plus the page copy above |

**Do not touch `changelog.mdx` on a non-Monday run.** Page corrections ship the
night the change ships, so the docs never describe a screen that no longer
exists. Release notes are a weekly digest, not a running log of every commit.

## Scope a run by the last commit, never by a date window

Work out what to document by diffing `campium-all` from the commit the last docs
change already covered, up to now. Do not scope to "commits in the last 24
hours" or "commits since yesterday".

Runs get missed. In August 2026 the agent skipped two nights and a fixed 24 hour
window would have dropped those two days of changes permanently, with nothing to
show that anything was missing. Scoping from the last covered commit means a
missed run is repaired by the next one automatically.

The same rule applies to the weekly changelog entry: cover everything not yet
described by an existing `<Update>` block, which is usually but not always the
last seven days.

## Changelog entries

One `<Update>` block per week, added above the existing blocks.

```
<Update label="2026-08-24" description="Short summary of the week" tags={["feature","improvement","bugfix"]}>
  ## New

  - **Thing** - What a camp can now do, and where to find it. See [Page](/page#anchor).

  ## Updates

  - **Thing** - What changed about something that already existed.

  ## Fixes

  - **Thing** - What was broken, described from the outside.
</Update>
```

- `label` is the date the entry publishes, meaning the Monday of the run. The
  entry covers the week before it.
- Include only sections that have content. An entry with no fixes has no
  **Fixes** heading.
- Only `tags` that apply. Valid values: `feature`, `improvement`, `bugfix`.
- Skip anything a camp cannot see: tests, dependency bumps, caching, logging,
  refactors, ops tooling.
- Link to the page section that explains the change wherever one exists.

## Verify against the code, not the commit message

Commit subjects describe intent. They routinely overstate what shipped, omit
that a feature is gated, or name a variable whose sense is inverted. Read the
template and the handler before writing the sentence a camp will read.

In particular, check whether a change is gated before describing it as general:
by payment processor, by module toggle, by role, by a purchase the family made,
or by a flag limiting it to one camp for now.

## Adding a page

A new `.mdx` file is invisible until `documentation.json` lists it in
`navigation`. A `path` in `documentation.json` with no matching `.mdx` file is
worse, because it breaks the build. Change both in the same commit.

## Do not break the build

An MDX syntax error does not fail loudly. The site keeps serving the last good
version and silently stops picking up every later commit, so the repo looks
healthy while the site quietly goes stale. This happened in August 2026 and went
unnoticed for five days.

Before committing, confirm that every `<Callout>`, `<Steps>`, `<Step>`,
`<Tabs>`, `<Tab>`, and `<Update>` you touched is closed, and that any `{` in
prose is inside backticks. MDX reads a bare `{` as the start of a JSX
expression.

## Writing for camps

- Address the reader as **you**. Write "You can offer a pay-in-full discount",
  not "Agencies can offer a pay-in-full discount".
- Never use the words "agency" or "agencies" in page copy. The reader runs a
  camp. Use "your camp" or "your organization".
- No em dashes anywhere. Use a hyphen or restructure the sentence.
- No time estimates. Do not write "takes about two minutes".
- Name buttons, tabs, and menu paths exactly as they appear on screen, in bold.

## Menu paths come from the sidebar, never from the URL

Directory names in `campium-all` do not match the sidebar. Forms live at
`/communication/forms.php` but **Forms** is its own top-level menu item. The
medication pages live under `/report/` but sit in the **Medical** menu.
`canteen/payments.php` is labelled **Deposits**. A path read off the URL looks
right and sends a camp to the wrong screen.

Take every menu path from `www/prod/template/header.php` in `campium-all`:

- Top-level items are `$arr[] = array('item'=>...)`. Their children are the
  `$subarr[]` entries built just above them.
- Some entries use a different variable or spacing, such as
  `'item' => 'Bunk Assignments'` and `$ci_entry = array('item' => 'Business Info & Logo', ...)`.
  Search for `'item'` with optional spaces around `=>`, not one exact pattern.
- Anything inside a `/* ... */` block is dead. The old
  **Admin → Site / Products / Portal / Usage / System** sub-nav is commented out.
  Sessions are reached from **Seasons** by clicking the season name.
- **Settings** (Passkeys, Two-Factor Auth, Email Notifications) is not in the
  sidebar. It is in the menu under your name, top right. Say so, or a camp will
  find **Medical → Settings** or **Portal → Settings** instead.
- The **Site Settings** tabs are exactly **Modules**, **Basic**,
  **Payment Info**, **Mobile App**, and **Admin**. **Profile Fields** is not one
  of them; it is its own item, **Admin → Profile Fields**.
- Write the full path from the top-level menu: **Admin → Site Settings → Modules**,
  not **Site Settings → Modules**, and **Reports → System Reports**, not
  **Reports → System**.
