Brag Doc ✿ Cherry on Tech · slumber-squad
A running list of what I built, fixed, and learned, so I never have to answer "what did I do these last few weeks?" from memory.

Update: every Friday (or after every merged PR) · Started: October 2026

Role: Developer, slumber-squad (Cherry on Tech dev squad) Project: slumber-squad, a web app that encourages marginalized genders to use AI in the workplace and helps close the AI adoption gap Live site: slumber-squad.netlify.app


Goals for This Program
Ship features from Figma mockup to production that real users can navigate with a keyboard and screen reader
Get fluent with a professional Git workflow: branches, PRs, code review, deploy previews
Build processes that make the squad faster, not just my own code
Practice explaining technical work to non-technical teammates (QA, PM, design)
Leave with portfolio-ready STAR stories for interviews
add your own


Projects
1. Repo foundation & tooling · Oct 4
Situation: The squad needed a shared codebase that everyone could clone, run, and contribute to safely from day one.

Task: Set up the repo, tooling, and guardrails before feature work started.

Action:

Scaffolded the React app with Vite and added Tailwind CSS v4, ESLint, and Prettier
Connected Netlify for continuous deployment: every merge to main deploys to production, and every PR gets its own deploy preview
Wrote the README (setup steps, a scripts table explaining each npm run command, deployment docs, Netlify status badge)
Created the PR template (Story Card / Description / Testing Instructions / Gotchas)
Added CODEOWNERS so reviewers get requested automatically

Result: Every squadmate can go from git clone to a running app with three commands, and every PR comes with a consistent description and a live preview. (PRs #1, #2)


2. Homepage · Oct 5 · PR #3
Situation: Users landing on slumber-squad need a quick way to say what they're working on so the app can point them to relevant AI resources.

Task: Turn the Figma homepage mockup into a working, responsive page.

Action:

Built the page from reusable components: Header, Footer, TaskChip, HomePage
Defined brand design tokens in Tailwind's @theme (brand purple #7510a7, Inter font), so the squad uses bg-brand instead of hard-coded hex values
Rendered the task chips from a data array (one source of truth) instead of hard-coding the JSX
Added accessible foundations: a role="search" landmark, a labeled search input, and visible focus outlines
Removed Vite boilerplate and kept unrelated package-lock.json changes out of the PR
Noticed the pink/blue stripes in the mockup were Figma's layout grid, not part of the design

Result: The homepage is live and matches the mockup on desktop and mobile. It's the foundation for every page after it.


3. Production build fix: file name casing · Oct 5 · PR #4
Situation: Right after the homepage merged, the Netlify production build failed with "cannot resolve ../components/TaskChip.jsx", even though it worked on my machine.

Task: Find out why and get production green again.

Action:

Diagnosed a case-sensitivity mismatch: the file was committed as Taskchip.jsx, but the import said TaskChip.jsx. Windows ignores letter case and Netlify's Linux build doesn't.
Fixed it with git mv, which records a rename that only changes capitalization (a normal rename in File Explorer wouldn't register on Windows)
Fixed the follow-up VS Code error by restarting the TypeScript server
Wrote the gotcha into CONTRIBUTING.md so no one else loses time on it

Result: Production was green again the same morning, with a single rename and no code changes. I now check file casing first whenever an import "can't be resolved."


4. Questions page + client-side routing · Oct 5 · PR #5
Situation: Users who aren't sure what they need had nowhere to go. The "Not sure where to start?" link was a dead end.

Task: Build the "Answer these questions" page from Figma and connect it to the homepage.

Action:

Built role (optional) and AI-experience chip groups using <fieldset> + <legend> for accessible grouping
Added hash-based routing (#questions) with a hashchange listener and useEffect cleanup, so the browser back/forward buttons work. It needs no new dependency and no Netlify redirect rules.
Reused TaskChip, so I didn't have to change the component at all
Caught and fixed two copy typos from the mockup ("occassionally," "we'll help you")

Result: Users can now go from the homepage to a guided questions flow. Squad feedback request sent with live link.


5. Accessibility fix: chip groups → radio groups · Oct 5 · PR #9 · Closes #7, #8
Situation: QA testing with NVDA across Chrome, Edge, Firefox, and Norton found that arrow keys didn't work in the chip groups, and the screen reader didn't read the task list. Keyboard and screen reader users couldn't use the core feature.

Task: Make every chip group on both pages fully keyboard- and screen-reader-accessible without changing the visual design.

Action:

Identified the root cause: single-select options were built as toggle buttons, when the semantically correct element is a radio group
Rebuilt TaskChip as a visually hidden native <input type="radio"> (sr-only, still focusable) with the pill styled through Tailwind peer-checked: / peer-focus-visible: classes
Pulled out a shared ChipGroup component (fieldset, legend, aria-describedby for hints) used on both pages, with React useId() for unique radio group names
Kept "click again to clear" by handling onClick instead of onChange
Verified with browser automation (Tab order, arrow keys, wrapping, accessibility tree) and asked QA to retest with NVDA

Result: All three chip groups now follow the WAI-ARIA Radio Group pattern: one Tab stop per group, arrow-key navigation, and proper screen reader announcements ("radio button, not checked, 1 of 6"). A shared component replaced the duplicate chip-group code on the two pages. This supports the app's mission directly: it's built for people who've been left out, so it has to work for everyone.

Follow-up: fill in QA's NVDA retest results here.


Collaboration & Process
Bug tracking system · PR #6
Goal: QA feedback was arriving as chat messages, with no way to track, prioritize, or confirm fixes.
What I did:
Recommended GitHub Issues (free, already in our repo, auto-links to PRs) and deferred the final call to the PM
Built a bug report issue template with steps to reproduce, expected vs actual, a multi-browser table, assistive tech, severity levels, and WCAG references
Created an accessibility label
Logged the first two bugs (#7, #8) as model examples for QA to follow
Taught QA the NVDA Speech Viewer trick for copying exact screen reader output into reports
Effect: Every bug now has a number, a consistent format, and a closing PR linked to it. QA has a clear, repeatable way to report bugs.
Squad workflow documentation · PR #10
Wrote CONTRIBUTING.md from scratch: branch naming, step-by-step Git flow, PR standards, deploy previews, bug reporting, QA testing workflow, code style, and gotchas
Defined the QA-before-merge loop: dev opens PR → QA tests the Deploy Preview → approve or comment → dev pushes fixes to the same PR → merge
Added a "comment on the PR vs log a new issue" decision table to cut down on confusion
Effect: New squadmates can onboard without asking, and the workflow lives in the repo instead of getting lost in chat.
Cross-functional communication
Wrote a structured feedback request to the whole squad covering aesthetics, accessibility, responsiveness, performance, copy, and bugs, so people gave specific feedback instead of "looks good!"
Explained deploy previews vs. production to QA so she knew where to test
Flagged to the squad that CODEOWNERS still has placeholder usernames (reviewers aren't being auto-assigned yet)
Working with AI
Used Claude as an AI pair programmer throughout: Figma-to-code, debugging the Netlify build, accessibility research, PR descriptions, and documentation
Reviewed, tested, committed, and pushed every change myself, and owned the Git workflow from start to finish
Fitting, since the app is about helping marginalized genders adopt AI at work. I'm using the same tools I'm building for.


Design & Documentation
Doc
Why it exists
README.md
Setup, scripts, and deployment explained for every squadmate
PR template
Consistent PRs: story, description, testing steps, lessons learned
Bug report template
Consistent, complete bug reports from QA
CONTRIBUTING.md
One source of truth for how the squad works
PR descriptions (#3, #4, #5, #9)
Each one includes numbered testing steps and a "What I Learned" section



What I Learned
React

useState with a function initializer (useState(getPage)) runs only on the first render
useEffect cleanup prevents duplicate event listeners, especially under StrictMode's double-run in development
useId() for stable unique IDs across component instances
"UI is a function of state": data arrays → mapped components

Accessibility

Pick the right HTML element for the behavior first: native radios gave me arrow keys and screen reader support for free
sr-only vs display: none: hidden visually vs hidden from everyone
fieldset/legend, aria-describedby, aria-pressed, role="search"
WCAG 2.1.1 (Keyboard), 4.1.2 (Name, Role, Value), 1.3.1 (Info and Relationships), WAI-ARIA Radio Group pattern
Automated checks can't replace testing with a real screen reader

Tailwind CSS v4

@theme design tokens that generate utility classes
peer / peer-checked: / peer-focus-visible: for styling based on a sibling's state

Git & GitHub

Branch → commit → push → PR → review → merge, from start to finish
git mv for renames that only change capitalization
Closes #N to auto-close issues, issue templates with YAML front matter, labels, CODEOWNERS
GitHub only reads issue templates from the default branch

DevOps / Deployment

Netlify deploy previews vs production builds
Case-sensitive Linux builds vs case-insensitive Windows/macOS
Hash routing vs path routing (and why path routing needs redirect rules)

Process

Setting up a QA workflow, writing bug reports, sorting bugs by severity
Writing docs so teammates don't have to ask me


Buzzwords Bank 🐝
Keywords for resumes, LinkedIn, and the A in STAR. Add as you go!

React · Vite · Tailwind CSS v4 · design tokens · component architecture · reusable components · state management · React hooks (useState, useEffect, useId) · client-side routing · hash routing · responsive design · Figma-to-code · accessibility (a11y) · WCAG 2.1 · WAI-ARIA · semantic HTML · screen reader testing (NVDA) · keyboard navigation · cross-browser testing · Git · GitHub · feature branching · pull requests · code review · GitHub Issues · issue templates · CODEOWNERS · CI/CD · Netlify · deploy previews · production debugging · case-sensitive file systems · ESLint · Prettier · technical documentation · QA workflow · cross-functional collaboration · Agile sprints · AI-assisted development


Outside the Squad
Open source: contributed the "developer" definition to the Cherry on Tech Tech Dictionary (cherryontech/website #265)
Accessibility practice: fixed 6+ WCAG issues in the a11ylearn scavenger hunt (lang attribute, heading structure, list semantics, focus outlines, link text, alt text)
Iyashicoded: add brand / freelance highlights
add talks, posts, community work


Weekly Log
Quick notes, polished into the sections above later.
Week of Oct 5, 2026
Oct 4: Set up repo, tooling, Netlify, README, PR template, CODEOWNERS
Oct 5: Shipped homepage (#3), fixed production build (#4), shipped questions page (#5), set up bug tracking (#6, #7, #8), fixed chip accessibility (#9), wrote CONTRIBUTING.md (#10). Merged 6 PRs in one day.
Feedback received: paste any kind words from squad / QA / PM here ♡
Week of Oct 12, 2026

Reflection Prompts
Most proud of:
Patterns I'm noticing in my work: (accessibility + process + docs?)
What I want more of / less of:
What I'd do differently: e.g. wait for QA approval before merging bug fixes (#9 merged before the NVDA retest)
If I were convincing a friend to join this squad, I'd say:
