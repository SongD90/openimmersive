# 01: Core Split view tracer bullet

**What to build:** Allow readers to choose **Split view** as their **display mode** in Options and the toolbar popup, and toggle it on and off using existing controls (floating `T` button, `Alt+T`, toolbar popup, context menu, and auto-translate). Turning **Split view** on covers the window with a 50/50 two-frame container showing the **original pane** on the left and the **translated pane** on the right (translated using **Replace** mode with a progress indicator). Duplicate extension UI (`T` button, selection popup, and hover translation) is hidden inside the panes so a single top-level `T` button controls the whole **Split view**. Turning **Split view** off removes the container and restores the underlying page unchanged.

**Blocked by:** None (can start immediately)

**Status:** ready-for-agent

- [ ] Readers can select **Split view** alongside **Bilingual** and **Replace** in Options and the toolbar popup, and the choice persists.
- [ ] Existing **Bilingual** and **Replace** behaviour and defaults remain unchanged.
- [ ] Toggling translation on when **Split view** is selected opens a full-window 50/50 container with a thin vertical divider, the **original pane** on the left, and the **translated pane** on the right, regardless of window width.
- [ ] Sites and languages configured to auto-translate open directly into **Split view** when **Split view** is the active **display mode**.
- [ ] The **original pane** displays the page as published without translating and without showing extension UI.
- [ ] The **translated pane** translates the page in **Replace** mode using the reader's configured target language and engine, showing translation progress while hiding the `T` button, selection popup, and hover translation.
- [ ] Toggling **Split view** off removes the two-frame container and returns the reader to the underlying page unchanged.
- [ ] Automated tests verify mode selection persistence, container creation/teardown, and pane role behaviour at the existing test seams.
