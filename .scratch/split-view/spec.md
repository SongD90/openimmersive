Status: ready-for-agent

# Split view display mode

## Problem Statement

A reader who wants to compare an article with its translation has only two choices today. **Bilingual** puts each translation under its original block, so the page gets long and the reader loses the shape of the original. **Replace** removes the original altogether. Neither lets the reader see the original page and the full translated page next to each other, each laid out the way the site intended, as in `img/expectedFormat.png`.

## Solution

Add a third **display mode**, **Split view**. When it is on, the window is divided into two halves. The **original pane** on the left shows the page as published. The **translated pane** on the right shows the same page translated. Each pane scrolls on its own and keeps the site's own styles, fonts and layout. By default the panes scroll together (**scroll sync**), so matching blocks stay level. Users turn Split view on and off with the same controls as the other modes. **Bilingual** and **Replace** are unchanged. See `GLOSSARY.md` for the terms, and ADRs 0001 and 0002 for the decisions below.

## User Stories

1. As a reader, I want to choose Split view as my display mode in Options, so that I can read original and translation side by side.
2. As a reader, I want to choose Split view from the toolbar popup as well, so that I don't have to open Options each time.
3. As a reader, I want Bilingual and Replace to work exactly as before, so that nothing I rely on changes.
4. As a reader, I want the existing toggle (the `T` button, `Alt+T`, the popup and the right-click menu) to turn Split view on and off, so that I don't have to learn new controls.
5. As a reader, I want the original page on the left and the translation on the right, so that the layout is always the same.
6. As a reader, I want each pane to be exactly half the window, so that neither side is crowded.
7. As a reader, I want each pane to scroll on its own, so that I can look ahead in one without losing my place in the other.
8. As a reader, I want each pane to lay itself out for half a window, so that top bars and fixed headers sit in their own half.
9. As a reader, I want the translated pane to keep the site's styles, fonts, images and links, so that it reads like the real page.
10. As a reader, I want the panes to scroll together by default, so that I stay at the same place in both.
11. As a reader, I want scroll sync to line up matching blocks, not pixel or percentage positions, so that the panes don't drift apart when the translation is shorter or longer.
12. As a reader, I want scroll sync to work from either pane, so that I can scroll whichever one I'm looking at.
13. As a reader, I want a toggle to turn scroll sync off, so that I can scroll the panes independently when I need to.
14. As a reader, I want my scroll sync choice to be remembered, so that I don't have to set it every time.
15. As a reader, I want blocks to be matched by their original text, so that an ad or personalized box that only one pane has doesn't throw off everything after it.
16. As a reader, I want repeated text such as "Share" to be matched in the order it appears, so that scroll sync still behaves sensibly.
17. As a reader, I want blocks that appear in only one pane to be skipped by scroll sync, so that they never pull the other pane to the wrong place.
18. As a reader, I want the translated pane to show only one `T` button's worth of controls (a single `T` button for the whole Split view), so that I'm not confused by two sets of controls.
19. As a reader, I want selection translation and hover translation turned off inside the translated pane, so that I don't get a translation of a translation.
20. As a reader, I want Split view to work on sites that normally refuse to be shown in a frame, so that it works on news sites and social media.
21. As a security-conscious user, I want the frame-blocking headers removed only for the extension's own frames in that one tab, so that no other page or tab loses its clickjacking protection.
22. As a security-conscious user, I want those header rules removed when Split view is turned off, so that they don't stay behind.
23. As a reader, I want the page to fall back to Bilingual with a short notice if the translated pane still can't load, so that I always get a translation.
24. As a reader, I want clicking a link in either pane to take the whole tab to the new page and reopen Split view there, so that both panes always show the same article.
25. As a reader, I want a link that jumps within the same page, such as a table of contents, to just scroll, and the other pane to follow, so that in-page navigation feels natural.
26. As a reader, I want links opened in a new tab to behave normally, so that they don't disturb my Split view.
27. As a reader, I want turning Split view off to return me to the original page as it was, so that I lose nothing.
28. As a reader, I want Split view to keep splitting however narrow the window is, so that the mode never changes behind my back.
29. As a reader, I want sites set to "always translate" to open straight into Split view when it is the chosen display mode, so that auto-translation matches my setting.
30. As a reader, I want the translated pane to use my chosen target language and translation engine, so that Split view translates the way the other modes do.
31. As a reader, I want the translated pane to show a progress indication while it is translating, so that I know it is working.
32. As a maintainer, I want the new mode's behaviour covered by tests at the existing seams, so that future changes don't break it.
33. As a maintainer, I want the extension's terms (Split view, original pane, translated pane, block, scroll sync) used consistently in code, labels and docs, so that the mode is easy to discuss and navigate.

## Implementation Decisions

- **New display mode value.** The display mode setting gains a third value for Split view, alongside Bilingual and Replace. It is selectable in Options and in the toolbar popup. Existing stored values and defaults are unchanged.
- **Container.** Turning Split view on covers the window with a full-window container holding two frames side by side, 50/50, with a thin divider. Both frames load the current page's URL. The original page stays underneath the container and is not modified, so turning Split view off just removes the container (ADR 0001).
- **Pane roles.** The content script now also runs inside frames, not only in the top-level page. Each instance works out its role: top-level page, original pane, or translated pane. The container tells each frame its role.
- **Translated pane behaviour.** The translated pane runs the existing Replace mode on its own copy of the page, using the user's target language and engine. The extension's floating `T` button, selection popup and hover translation are not shown in it. A single `T` button, on the top-level page, controls the whole Split view.
- **Original pane behaviour.** The original pane does not translate and shows no extension UI.
- **Frame-blocking headers.** When Split view starts, the background script adds `declarativeNetRequest` rules that remove `X-Frame-Options` and the `frame-ancestors` part of `Content-Security-Policy` for the extension's own frames in that tab only. The rules are removed when Split view ends or the tab closes. This needs one new install-time permission (ADR 0002).
- **Fallback.** If the translated pane fails to load, the page switches to Bilingual and shows a short notice (ADR 0002). Detecting "failed to load" covers a failed network request, a refusal to be framed that the header removal didn't fix, and a pane that never reports that it is ready within a time limit.
- **Block pairing.** Each pane reports the original text of its blocks, in order, along with their positions. A pairing step matches a translated-pane block with an original-pane block that has the same original text. Repeated texts are matched in order of appearance. Blocks with no counterpart are left unpaired and ignored by scroll sync. This is a pure function so it can be tested without a real page.
- **Scroll sync.** When one pane scrolls, the reader's position is expressed as "the paired block nearest the top of the pane, plus how far into it". The other pane is scrolled to put the matching block at the same place. A toggle turns syncing off, and the setting is stored and remembered. This position calculation is also a pure function: given both panes' block positions and one pane's scroll position, it returns the other pane's scroll position. Guard against feedback loops, where scrolling one pane causes the other to scroll and report back.
- **Messages between frames.** The container and the two frames communicate by messages: ready, translated blocks and positions, scrolled, link clicked, load failed. Each message is checked to come from the extension's own frames in the same tab.
- **Link handling.** A link clicked in either pane takes the whole tab to the new URL, and Split view reopens there. Same-page anchors scroll the pane and scroll sync moves the other. Links that open in a new tab behave as normal.
- **Auto-translate.** Sites and languages set to auto-translate open into Split view when it is the chosen display mode.
- **Narrow windows.** There is no width threshold. Split view always splits, and each pane shows the site's own layout for its width.
- **Not changing.** Bilingual and Replace behaviour, translation engines and routing, sensitive-data masking, and the file translation feature.

## Testing Decisions

- **What makes a good test:** it checks what a user or the rest of the extension can observe (what is on the page, which messages are sent, which settings are saved, which rules are added), not how the code does it.
- **`content.js`** is the main seam, tested the way it is today: loaded into jsdom and exercised through `__yllInternals`. Cases:
  - Turning Split view on builds the container with two frames pointing at the current URL, and turning it off removes it and leaves the page unchanged.
  - Role detection: top-level, original pane and translated pane behave differently, and the translated pane runs Replace mode and hides the extension's UI.
  - Block pairing as a pure function: identical pages, an extra block in one pane, repeated texts, and no matches.
  - Scroll sync position calculation as a pure function: translations shorter and longer than the original, unpaired blocks, top and bottom of the page, and feedback guarding.
  - Messages: load failure leads to Bilingual with a notice, a scrolled message leads to the other pane following, a link click leads to the tab going to the new URL, and messages from other senders are ignored.
- **`background.js`** is tested in its isolated vm sandbox with the mocked browser API: the header-removal rule is added for the right tab and frames when Split view starts, and removed when it ends or the tab closes.
- **`options.js` and `popup.js`** are tested the way they are today: Split view can be selected as a display mode, and the scroll sync toggle is saved and restored.
- **Prior art:** `tests/content.test.js` and `tests/content-dynamic.test.js` for content script behaviour, `tests/background.test.js` for background behaviour, `tests/options.test.js` and `tests/popup.test.js` for settings.
- **Manual checks** (jsdom can't cover real framing, real headers or real scrolling): a news site such as the one in `img/expectedFormat.png`, Wikipedia, a site that blocks framing, a site that redirects out of frames, and a narrow window.

## Out of Scope

- A draggable divider or any split other than 50/50.
- Swapping the sides (translation on the left).
- Showing different target languages in different panes.
- Split view for local file translation.
- Persisting the reader's place across a reload.
- Browsers or contexts where `declarativeNetRequest` is unavailable. How Firefox is handled needs a decision during implementation (see Further Notes).

## Further Notes

- **Firefox.** The extension supports Firefox (event page, `browser` API). Firefox uses a different mechanism for changing response headers, so the header removal in ADR 0002 may need a separate implementation there. This was not discussed; check it before building.
- **Page loads three times.** The top-level page plus two frames means the site is loaded three times. This is accepted in ADR 0001, but worth noting for pages with heavy scripts, logins or one-time content.
- **Assumptions made without a question** (recorded in the grilling session): repeated identical text is paired in order of appearance, same-page links just scroll, links opened in a new tab behave normally, and auto-translate sites open into Split view.
- **Possible stale label file.** `docs/agents/triage-labels.md` is referenced by the issue tracker doc but does not exist. This spec uses the default `ready-for-agent` label.
- **ADR file name.** `docs/adr/0001-split-view-loads-page-twice-in-a-frame.md` still has its original name, although it now describes two frames. Renaming was blocked by a permissions error.
