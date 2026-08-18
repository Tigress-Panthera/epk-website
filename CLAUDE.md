# Tigress Panthera — site maintenance guide (for Claude Code)

This repo **is** the live website for **tigresspanthera.com** — a static, single-page
**artist EPK / press kit** for the DJ **Tigress Panthera**, hosted free on GitHub
Pages, migrated off Bandzoogle. The owner, **Milly** (Emily Anderson), is
non-technical and makes periodic updates by describing them in plain English. Your
job: make the change faithfully, **preview it, and deploy only after she confirms.**

Sister site (same owner, same hosting pattern, separate org):
`Vital-Kneads/vital-kneads-site` (vitalkneadsmt.com). Full cross-site reference lives
in Notion: "Web Architecture — Vital Kneads & Tigress Panthera."

## Working with Milly (the loop)
1. She describes a change in everyday language ("tighten the bio", "add a show",
   "swap a photo", "make the orange a little deeper").
2. You make the edit(s).
3. **Preview** — run the local server and screenshot it so she can see it before it's
   public (or describe exactly what changed).
4. **Deploy only on her OK.** Deploy = commit + push to `main`; GitHub Pages rebuilds
   and it's live at https://tigresspanthera.com within ~1 minute.
5. Every change is a git commit, so **anything can be undone** — if she dislikes a
   change, revert it (`git revert` or restore the file and redeploy).

## Preview locally
```
python3 -m http.server 8080
```
Then open http://localhost:8080 . Images are relative, so previewing from the repo
root shows everything. (Slideshow photos are `loading="lazy"` — they only load once
scrolled into view; that's not a bug.)

## Deploy
```
git add -A && git commit -m "describe the change" && git push
```
Push auth is in the macOS keychain via `gh` (account `vitalkneadsco`). Live in ~1 min.
Confirm with `curl -sI https://tigresspanthera.com/` if needed.

> Note: this must be run from a real local clone of `Tigress-Panthera/epk-website`.
> A cloud-only assistant with no shell / no GitHub network access can't push — in that
> case, hand Milly the changed files to commit via the GitHub web uploader instead.

## Structure
- **One page, no build step:** everything is in `index.html` (inline CSS + JS).
- **Sections:** sticky nav → hero → `#bio` → `#shows` (Highlights grid + a
  collapsible `<details>` full archive) → `#music` (YouTube embeds + SoundCloud
  player) → `#photos` (two slideshows: Promo, Press) → `#logos` → `#booking`.
- `images/` — all 33 images, referenced with **relative** paths `images/<name>`.
  Self-hosted (localized off Bandzoogle's old CDN) — no external image dependencies.
  **Keep `index.html` and `images/` together.**
- `CNAME` — holds `tigresspanthera.com`. **Never delete.**
- The two slideshows are built in JS from the `promoPhotos` / `pressPhotos` arrays at
  the bottom of `index.html`.
- Colors, fonts, and copy voice are this brand's own — see the site's `:root` block
  and Milly's Notion Brand Identity page. (Distinct from Vital Kneads; don't cross them.)

## Conventions & gotchas
- **Images:** prefer web-friendly sizes; always include `alt`; `loading="lazy"` for
  the slideshows. To add a photo: drop the file in `images/` and add its path (e.g.
  `'images/newpic.jpg'`) to the relevant `promoPhotos` / `pressPhotos` array.
- **Booking** → `mailto:vitalkneadsco@gmail.com`. SoundCloud + Instagram links in the
  hero and footer are external; keep as-is.
- **Do NOT touch:** the `CNAME` file, DNS, or anything outside this repo. Domain is at
  **Porkbun (~$11/yr)**; DNS = 4 apex A records to GitHub Pages + `www` CNAME; deploys
  from `main` / root with "Enforce HTTPS" on. DNSSEC is **off** — ignore DNSSEC prompts.
- Don't merge this repo with the Vital Kneads repo — separate orgs on purpose.

## Common tasks (cookbook)
- **Edit the bio** → find the text in the `#bio` section of `index.html`.
- **Add a show** → add a `.show` card in the Highlights grid, or an `<li>` in the
  `<details>` archive. Keep the strongest/most-recognizable names in Highlights.
- **Add/replace a photo** → put the file in `images/`, add its path to the
  `promoPhotos` or `pressPhotos` array.
- **Tweak a color / font** → edit the token or rule in the `:root` / `<style>` block.
- **Update the booking address** → edit the `mailto:` in `#booking`.
