# School Zone Law Check

A six-question interactive quiz that tells Denton drivers what the Texas school bus and school zone laws actually say — and cites the statute behind every answer. Built for the Denton Fire Department, Community Risk Reduction Division, and shipped with the August 2026 *Frontline Report*.

Most drivers miss at least one. The center turn lane question is the one almost everyone gets wrong.

**Live tool:** https://dentonfire.github.io/School-Zone/

---

## Why this exists

Denton ISD went back on August 13, 2026. Texas recorded **877 crashes in school zones in 2025, a 12 percent increase** over the prior year, with distracted driving and speeding leading the causes (TxDOT). Most of those are not caused by reckless people — they are caused by drivers who genuinely do not know a rule, or who believe a rule that isn't real.

A tip sheet tells people what to do. A quiz shows them what they got wrong. That difference is the whole point of this tool.

---

## The six questions

| # | Scenario | Correct answer | Authority |
|---|---|---|---|
| 1 | Bus stops across a five-lane road with a shared center turn lane | **You must stop** | TX Transp. Code §545.066(a), (f)(1), (f)(2) |
| 2 | Handheld phone call in an active school crossing zone | **Not legal** | TX Transp. Code §545.425; §541.302(13) |
| 3 | Size of the school bus danger zone | **10 feet / five giant steps** | NHTSA School Bus Safety |
| 4 | First-offense fine for passing a stopped bus | **$500 to $1,250** | TX Transp. Code §545.066(c), (d); Penal Code §§12.21, 12.35 |
| 5 | Texting while driving outside a school zone | **Banned statewide** | TX Transp. Code §545.4251 |
| 6 | Child drops something under the bus | **Leave it, tell the driver** | NHTSA School Bus Safety |

---

## How it works

One self-contained `index.html`. No build step, no backend, no dependencies to install.

- **No APIs.** Everything is client-side. Nothing to break, nothing to rate-limit.
- **No data collection.** Answers and scores never leave the device. Nothing is stored or transmitted.
- **One external script:** `qrcodejs` from cdnjs, used only for the share QR. If it fails to load, the tool falls back to an `api.qrserver.com` image automatically.
- **Progressive flow:** one question at a time, immediate right/wrong feedback with the statute, then a score screen that replays anything missed.
- **Share:** uses the native share sheet when available (`navigator.share`), otherwise opens the QR modal.

### Design system
Matches the DFD tool family (Fire Code Lookup, After the Fire, Home Safety Checklist): navy `#152a40`, red `#bf2726`, gold `#f9c031`, Inter, cube-texture header, gold QR share FAB, DFD patch favicon and Open Graph image.

---

## Deploying

1. Create a repo named exactly **`School-Zone`** under the `dentonfire` GitHub organization.
2. Add `index.html` (and this `README.md`) to the repo root.
3. **Settings → Pages**, source: `main` branch, root folder.
4. Live at `https://dentonfire.github.io/School-Zone/`.

The QR code encodes `window.location.href`, so it self-corrects to wherever the tool is actually hosted. The `DEFAULT_URL` constant is only the fallback for local `file://` preview. If you publish under a different slug, also update `og:url` in the `<head>` so link previews resolve correctly.

---

## Editing the questions

Everything lives in the `QUESTIONS` array near the top of the `<script>` block. Each entry:

```js
{
  scene:   "The setup — a concrete situation, not an abstraction.",
  q:       "The question itself.",
  opts:    ["Option A", "Option B", "Option C"],   // 2 or 3 options
  correct: 0,                                       // zero-based index
  why:     "The explanation. HTML allowed. Lead with the verdict in <strong>.",
  cite:    "The statute or source. HTML allowed."
}
```

Add or remove entries freely — the progress bar, scoring, and results screen all read from array length. The scoring thresholds in `renderResult()` are hardcoded to six questions; adjust if you change the count.

**To reuse this shell for another topic**, swap the question bank and the header copy. The same structure works for a fireworks quiz in December or a burn ban quiz in summer.

---

## Legal verification

Every legal claim in this tool was verified against **primary sources** — Texas Transportation Code text and enrolled bill texts on capitol.texas.gov — not law-firm blogs or news summaries. An independent adversarial review was run specifically to *refute* each answer. Five corrections came out of it, two material:

1. **§545.066(f)(2) is dispositive on the turn lane question.** The statute says outright that a highway is *not* considered to have separate roadways if they are separated only by a left turn lane. An earlier draft argued this by inference from (f)(1); the direct citation is stronger and is what ships.
2. **"Stopped in traffic is not stopped" is NOT state law.** §545.4251 never defines "stopped," and nothing in the statute or bill analyses says a red light fails the exception. That rule comes from **Denton's own citywide handheld ordinance**, and the tool attributes it there.
3. **The missing-sign defense does not apply in Denton.** §545.425(d)(2) makes an unposted sign an affirmative defense, but §545.425(d-1) removes it in jurisdictions with a compliant citywide ban — which Denton has.
4. **"School crossing zone" is narrow** (§541.302(13)): the designated reduced-speed zone, only during the hours the reduced limit is in effect. Not "anywhere near a school."
5. **10 feet is a minimum standoff, not a safe clearance.** NHTSA's rule behind the bus is categorical — never. The tool says so explicitly, because the rear blind spot is where that number could get a child hurt.

### Before this goes public
- [ ] **City Attorney review.** This tool states penalties and tells residents what is legal. It should be read by legal before it is promoted.
- [ ] **Confirm the Denton ordinance section number** for the citywide handheld ban, and add it to the citations in questions 2 and 5.
- [ ] Confirm current statute text — verification was done via enrolled bill texts and a rendered snapshot, because the official statutes site now blocks automated retrieval.

### Maintenance
The Texas Legislature meets in **odd-numbered years**. Re-verify §§545.066, 545.425, and 545.4251 after each regular session (next: 2027) and update fine amounts and offense levels if they change. No 2025 session bill amended these sections.

---

## Accessibility

- Full keyboard operation; visible focus states.
- Focus moves to the modal Close button when the QR dialog opens; Escape closes it.
- Touch targets sized for mobile; 16px inputs prevent iOS zoom.
- Respects `prefers-reduced-motion`.
- Correct/incorrect state is conveyed by icon and text, not color alone.

---

## Known limitations

- **This is a plain-language guide, not legal advice**, and the tool says so on every screen.
- **Denton-specific.** The city ordinance references do not apply outside Denton city limits. A resident of Corinth or Argyle gets accurate *state* law but not their own city's rules.
- **Fine ranges exclude court costs**, which are additional.
- The license suspension in question 4 is **discretionary** ("the court may order") and only on a second or subsequent offense. The copy hedges this deliberately — keep the hedge if you edit it.

---

## Credits

Built for the **Denton Fire Department — Community Risk Reduction Division**.
Legal sources: Texas Transportation Code, Texas Penal Code, Texas Legislature Online.
Safety guidance: National Highway Traffic Safety Administration.
Crash data: Texas Department of Transportation.
Contact: (940) 349-8840 · https://dentonfire.com
