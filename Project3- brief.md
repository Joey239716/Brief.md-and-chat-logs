# Brief: The Adpharm contact form

Add a contact form to The Adpharm's existing website. The Adpharm is a healthcare and pharmaceutical marketing agency. The form works like Typeform: one question per screen, driven by the keyboard, and it should feel calm and quick. It lives on its own page within the site, and a prospective client should be able to finish it in under two minutes.

**This is a one-shot build.** The user gives one prompt and gets a finished, working, verified form back. Nobody will answer questions, so:

- Do not ask clarifying questions, ask for approval, or hand back half-done work.
- Where this brief is silent, make the call yourself and log it in `DECISIONS.md` (format at the end).
- When something breaks, fix it or work around it and log what happened.
- Your turn ends only when every item under **Done means** passes, or when something is genuinely impossible. If it's impossible, say exactly what and why in `DECISIONS.md`.

**The website already exists. Fit in with it.** Use the site's existing framework, language, styling approach, components, package manager and scripts. Do not add a new framework, replace the build setup or restructure the site. Do not use a Typeform embed, and do not add a new hosted form service (one the site already uses is fine; see section 9).

---

## 1. What this is for

The form is the first thing a prospective client (a pharma brand manager, a marketing lead or a medical affairs contact) sends the agency. It has two jobs:

1. **Qualify the lead.** By the end, the agency should know who this person is, what they need, how soon they need it and roughly what budget they have.
2. **Show the agency's craft.** This is a design agency's form. It should look considered and clearly part of the site, and it should not look like a template.

When choosing between options, pick whichever serves those two goals better.

---

## 2. Order of authority

When sources conflict, follow this order:

1. This brief
2. The site's existing design system and brand assets
3. The site's existing code conventions
4. Your own judgement, which you log in `DECISIONS.md`

---

## 3. Getting to know the site

Before you write anything, learn how the site works and write a short summary at the top of `DECISIONS.md`.

- **Stack:** framework, language, styling (CSS modules, Tailwind, Sass and so on), package manager, how routing works, and how the site is built and served.
- **Commands:** install, dev, build, lint, typecheck and test, whatever exists. Run them, and note anything that already fails *before* you change anything, so you don't get blamed for it or waste time on it.
- **Brand:** the logo files, fonts, colour variables or tokens, spacing and type scale, and the existing components (buttons, inputs, links). Also check any brand folder (such as `/brand`) or guidelines in the repo.
- **Contact today:** whether the site already has a contact page, form, `mailto:` link or a "Contact" call to action in the navigation or footer.
- **Back end:** whether the site has API routes, server functions or a server. If it has none, how it is hosted (static, serverless and so on), as far as the repo shows.
- **Form handling:** whether the site already sends forms anywhere (HubSpot, Netlify Forms, Formspree, a CRM script and so on).
- **Baseline screenshots:** before you change anything, take screenshots of the home page and one other key page at 1440×900 and 390×844, saved to `shots/baseline/`.

---

## 4. Brand

- Use only the colours, typefaces and components the site already uses. If you need a tint or shade that doesn't exist (a hover state, border or disabled state), derive it from an existing colour and log it.
- Reuse the site's existing button, input and link styles where they fit. If one doesn't fit the Typeform-style layout (for example, the question type needs to be much larger than any existing heading), extend the site's scale rather than inventing a new one, and log it.
- If something you need is missing, such as a dark version of the logo, make a careful fallback and log it. Do not stop.
- **Do not invent facts about the agency.** That means no addresses, phone numbers, client names, awards, staff names or statistics unless they already appear on the site. Where something would normally go, leave it out rather than making it up.

### Design skills: install and use them

Use these two Claude Code skills for design and UI decisions:

- **`frontend-design`** (Anthropic): helps with the aesthetic direction and typography and keeps the result from looking like a template.
- **`ui-ux-pro-max`**: covers UX guidelines, accessibility, interaction patterns and stack-specific UI advice.

**Check whether they're installed.** Look for `SKILL.md` in `~/.claude/skills/<name>/`, in `.claude/skills/<name>/` inside this project, or in `~/.claude/plugins/marketplaces/*/plugins/<name>/`. If one is missing, install it into this project:

```bash
mkdir -p .claude/skills

# frontend-design: Anthropic's official plugin marketplace
git clone --depth 1 https://github.com/anthropics/claude-plugins-official /tmp/cpo
cp -R /tmp/cpo/plugins/frontend-design/skills/frontend-design .claude/skills/

# ui-ux-pro-max
git clone --depth 1 https://github.com/nextlevelbuilder/ui-ux-pro-max-skill /tmp/uipro
cp -R /tmp/uipro/.claude/skills/ui-ux-pro-max .claude/skills/
```

If those paths have moved, find the skill folder in the cloned repo (the one containing `SKILL.md`) and copy that folder.

**How to use them**
- If a skill is available through the Skill tool, invoke it. A skill installed during this session may not show up there until the next session; in that case, **read its `SKILL.md` and follow it directly**, along with any reference files or scripts it points to. Not being able to invoke a skill is not a reason to skip it.
- Use `frontend-design` to shape the form's design **within the site's existing visual language**, then use `ui-ux-pro-max` to check the UX, accessibility and form patterns. Use both before writing styles, and again during each screenshot review round (section 10).
- **The site's design system and this brief override both skills.** If a skill suggests a colour, typeface or pattern that conflicts with the site or with section 9, follow the site and the brief.
- In `DECISIONS.md`, record that the skills were found or installed, and list the main recommendations you took from each.

---

## 5. Order of work

1. Read this whole brief.
2. Get to know the site (section 3), take the baseline screenshots, and start `DECISIONS.md` with your summary.
3. If the project is a git repository, create a branch called `contact-form`. Commit as you go if you like, but never push.
4. Find or install the design skills, and use them to settle how the form will look within the site.
5. Build the endpoint and the shared validation, then the form screens, then the branching, saved progress and failure handling.
6. Link the form into the site (section 6).
7. Write the end-to-end test and the screenshot script, using the test tooling the site already has if there is any, and run both.
8. Do the screenshot review rounds and the accessibility pass, fixing problems as you find them.
9. Update the README, finish the **Verification** section of `DECISIONS.md`, and run every **Done means** check one last time.

---

## 6. Where it goes in the site

- The form gets **its own page**, at `/contact` or wherever the site's routing conventions say it belongs. It is focused and free of distractions: you may hide or reduce the site's main navigation on this page, but keep the logo, linked to the home page, and a clear way out.
- Point the site's existing "Contact" links and calls to action at the new page.
- If the site already has a contact form, leave it working and put the new one at its own route. In `DECISIONS.md`, explain how to swap one for the other.
- **Keep your footprint small.** Only edit files outside the form when you need to link the form in or reuse something. List every existing file you changed in `DECISIONS.md`.

---

## 7. The questions

Each item below is one screen. The **wording is a starting point**: you may tighten it, and you should log any change of meaning. **What each screen asks, which screens are required and how the flow branches are fixed.**

| # | Screen | Input | Required | Notes |
|---|---|---|---|---|
| 0 | **Welcome**: "Let's talk about your next project." with one line of supporting copy and a Start button | none | — | Enter starts the form. Say roughly how long it takes ("About 2 minutes"). |
| 1 | "First, what's your name?" | Text, first and last name | First name required | Later screens may greet the person by first name. |
| 2 | "What's the best email to reach you, {first name}?" | Email | Yes | Validate the format. Hint that a work email is preferred, but don't block personal addresses. |
| 3 | "Which company are you with?" | Text | Yes | |
| 4 | "What's your role?" | Single choice | Yes | Brand / product manager · Marketing lead · Medical / regulatory · Procurement · Agency or partner · Other (choosing it reveals a text field) |
| 5 | "What can we help with?" | Multiple choice | At least one | Brand strategy & creative · HCP marketing · Patient education / DTC · Digital & web · Medical education · Media · Something else. If the site lists its services, use the site's names and log it. |
| 5b | *Branch: shown only if HCP marketing, Patient education / DTC or Medical education was picked on screen 5.* "Where is the product in its lifecycle?" | Single choice | Yes, when shown | Pre-launch · Launching within 12 months · In market · Not product-specific |
| 5c | *Same branch condition.* "What therapy area?" | Text | No | Example placeholder: "e.g. oncology, dermatology". |
| 6 | "When do you need to get started?" | Single choice | Yes | As soon as possible · 1–3 months · 3–6 months · 6+ months · Just exploring |
| 7 | "Do you have a budget range in mind?" | Single choice | No (show a visible Skip) | Under $50K · $50K–$150K · $150K–$500K · $500K+ · Not sure yet. Currency: CAD, shown once on the screen. |
| 8 | "Tell us a bit about the project." | Long text | Yes, at least 20 characters | Below the field, show this note: *"Please don't include patient information or adverse event reports. To report a side effect, contact the product's manufacturer."* |
| 9 | "How did you hear about us?" | Single choice | No | Search · Referral · LinkedIn · Event or conference · Other |
| 10 | **Consent** | Two checkboxes | First one only | (a) "I agree to be contacted about this inquiry." Required. (b) "Send me occasional insights from The Adpharm." Optional and **unticked by default**. Link to the site's privacy policy if it has one. Otherwise use a placeholder link and log it. |
| 11 | **Review** | none | — | Summarise every answer, with an Edit link on each that jumps to its screen and then returns here. The Submit button is on this screen. |
| 12 | **Thank you** | none | — | "Thanks, {first name}. We'll be in touch within two business days." Offer a link back to the home page. No further fields. |

**Branching rules**
- If the person goes back and unticks every option that triggers 5b and 5c, both screens are skipped and their answers are dropped from the payload.
- The progress indicator counts only the screens on the person's current path.

---

## 8. How it behaves

These are requirements, not suggestions.

**Navigation**
- Exactly one question is on screen at a time. Moving between screens takes a short transition of 300 ms or less. With `prefers-reduced-motion`, there is no movement, only an instant change or a fade.
- **Enter** advances when the current answer is valid. In long text, **Enter** adds a new line and **Cmd/Ctrl + Enter** advances. Show this hint near the field.
- **Choice screens:** each option has a letter key (A, B, C…) shown on the option. Pressing it selects the option, and on single-choice screens it also advances after a short beat (about 250 ms). The arrow keys move between options.
- There are visible Back and Next controls and a progress indicator. Back never loses an answer.
- On each new screen, focus moves to the input. On touch devices, take care that this doesn't make the page jump when the keyboard opens.

**Validation**
- Validate when the person tries to advance, not while they type. The error appears inline, is announced to screen readers and is cleared as soon as the answer is fixed.
- The same validation rules run on the server. Put them in one shared module that both the client and the server use.

**State**
- Answers are saved to `localStorage` as the person goes. Reloading the page resumes at the same screen with the same answers. A successful submission clears them.
- A submission must not be sent twice. Disable Submit while the request is in flight.

**Failure**
- If the endpoint is unreachable or returns an error, the person stays on Review with their answers intact, sees a plain-language message and gets a Retry button.
- If the server rejects specific fields, jump to the first bad screen and show the server's error message there.

**Responsive**
- Works from 320 px wide to large desktop. Touch targets are at least 44 px.

---

## 9. The endpoint

Build it the way the site builds server code: an API route, a server function or a handler on its existing server. If the site has no back end at all, add the smallest server-side endpoint that suits how the site is built and hosted, and log the choice. Use `/api/contact` unless the site's conventions say otherwise.

- It accepts a JSON `POST`.
- Validate on the server with the shared rules and check the branching (5b and 5c must not be present unless they are triggered). Return:
  - `201 { ok: true, id }` on success
  - `400 { ok: false, errors: { field: message } }` when validation fails
  - `429` when the rate limit is hit (a simple in-memory limit per IP is enough, for example 5 requests a minute)
  - `413` for bodies over 20 KB
- **Spam:** add a hidden honeypot field. If it is filled in, return a fake success and do not store the submission.
- **Storage:** keep it behind one small function so it is easy to replace. By default, append one JSON line per submission to `data/submissions.jsonl`, with an `id`, an ISO timestamp, the answers and the user agent. Create the folder if it is missing, and add `data/` to `.gitignore`. If the site's hosting means local files won't persist (serverless, for example), keep the JSON-lines store for local use, and note in `DECISIONS.md` what production would need.
- **If the site already uses a form service or CRM**, the endpoint forwards each valid submission to it the same way the site's existing forms do, mapping the answers onto its fields, and also writes to the local JSON-lines store. If forwarding needs credentials that aren't in the repo, read them from environment variables, skip forwarding when they're missing, and document them in the README. Do not hard-code keys. The tests must not send anything to the real service.
- Trim strings and cap their lengths (choose sensible limits and log them).
- Do not send email. Leave a clearly marked `TODO: notify` comment where an email or CRM integration would go.

---

## 10. Design guardrails

You have creative freedom within the site's design language. These rules are fixed.

**Do**
- Make it feel like part of the site: the same type, colours and button style.
- Set the question large; it is the hero of each screen.
- Use generous whitespace and a single column, with the question and its answer in a clear relationship.
- Make focus states visible and on-brand, not the browser default.
- Use motion only to explain (where you came from and where you're going), never as decoration.

**Do not** (these give away a generated template)
- Purple-to-blue gradients, glassmorphism or backdrop blur cards, or glowing borders
- Emoji in the UI
- Small all-caps "eyebrow" labels above questions, unless the site already uses them
- Fade-and-slide-up on every element, or staggered entrance animations
- Placeholder text used as the label
- Stock illustrations or 3D blobs
- Arrows (→) appended to every button label, unless the site already does this
- Centred body text longer than two lines

**Accessibility (WCAG 2.2 AA)**
- Text contrast is at least 4.5:1, or 3:1 for large text and UI boundaries. Check every colour pairing you use. If a brand colour fails for small text, keep it for large text and graphics only, and log that.
- Every screen is a real `<form>` region with a programmatic label: a `<label>`, or a `<fieldset>` with a `<legend>` for choices.
- Errors and step changes are announced through an `aria-live` region ("Question 3 of 9").
- Everything works with the keyboard alone and with VoiceOver.
- The page has its own `<title>` and meta description, set the way the site sets them.

---

## 11. Done means

Do not stop until all of these are true. Record the result of each in `DECISIONS.md` under **Verification**.

1. **Builds and runs.** The site's install and build commands succeed, and the production build serves the form, which submits successfully. The site's existing lint, typecheck and test commands fail no more than they did at baseline.
2. **Nothing else broke.** Re-shoot the baseline pages and compare them with `shots/baseline/`. The only differences should be the updated Contact links.
3. **End-to-end test**, an automated test that drives a real browser. Use the site's existing test tool if it has one; otherwise add one and log it. Add a script for it in the site's usual way. The test must:
   - complete the full path **with the keyboard only**, including the 5b/5c branch, and check that a matching submission is stored
   - complete a path without the branch and check that 5b and 5c are absent from the stored payload
   - check that an invalid email blocks advancing and shows an error
   - check that the endpoint returns 400 for an invalid payload posted directly
   - check that a filled honeypot returns success but stores nothing
   - check that reloading mid-form resumes on the same screen
   - use a temporary data file, so tests don't pollute real submissions
4. **Screenshot self-review.** A script saves a screenshot of every screen, plus an error state and the endpoint-failure state, at **1440×900** and **390×844**, in `shots/`. **Open every screenshot and look at it.** Fix what's wrong and shoot again. Do at least two rounds, and note in `DECISIONS.md` what each round changed.
5. **Keyboard and accessibility pass.** Tab order, focus visibility, letter keys, Enter and Cmd/Ctrl+Enter all work. Contrast is checked for every colour pair you use, with the ratios listed in `DECISIONS.md`. Reduced motion is respected.
6. **README.** Add a short "Contact form" section to the site's existing README. Do not rewrite the rest. It covers where the form lives, how to run its test and screenshot scripts, where submissions are stored and where the `TODO: notify` hook is.

---

## 12. Out of scope

Do not build any of these:

- Email sending, CRM integration or analytics
- Authentication or an admin view of submissions
- A privacy policy page
- Localisation (English only, though the copy should be easy to find in one file)
- File uploads
- Any redesign or refactor of the rest of the site

---

## 13. Structure

Put files where the site's conventions say they belong. These rules are fixed:

- The questions are **data-driven**. All question definitions and copy live in one file, so adding or rewording a question means editing that file only.
- The validation rules are in one module that both the client and the server use.
- Submissions go to `data/` (gitignored), and screenshots go to `shots/`.
- `DECISIONS.md` sits at the project root.

---

## 14. DECISIONS.md format

Start with the site summary from section 3, then log every call this brief didn't make for you. Newest entries go first.

```markdown
# Decisions

## Site summary
Stack, commands, brand sources, existing contact entry points, back end, baseline failures.

## Files changed outside the form
- path/to/Header.tsx: Contact link now points to /contact

## D-07  Question type size
- Gap: the site's largest heading (48px) is too small for a one-question-per-screen layout.
- Call: added a 64px step to the existing type scale, using the same font and weight.
- Why: the question needs to dominate the screen while staying within the site's type system.
- Revisit if: the design team defines a display size.
```

End the file with a **Verification** section that records the outcome of every **Done means** item: what passed, what failed and what was not checked.

---

## 15. Final message

When you finish, end with a short report to the user, no longer than one screen:

- **What was built:** the route, and where the form's files are.
- **How to run it:** the dev command, the test command and the screenshot command.
- **Where submissions go:** the local file, plus any forwarding and the environment variables it needs.
- **Status:** each **Done means** item, marked passed, failed or not checked, in one line each.
- **The five decisions that matter most:** the calls in `DECISIONS.md` the user is most likely to want to change, with their IDs.
- **Existing files changed:** a count, pointing to the list in `DECISIONS.md`.
