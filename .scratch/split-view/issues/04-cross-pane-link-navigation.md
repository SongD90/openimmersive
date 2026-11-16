# 04: Cross-pane link navigation

**What to build:** Keep both panes on the same article when the reader clicks links inside **Split view**. Clicking a standard page link in either the **original pane** or the **translated pane** navigates the whole tab to the destination URL and reopens **Split view** there automatically, while same-page anchor links scroll in place and links opened in a new tab behave normally without disturbing the current **Split view**.

**Blocked by:** 01: Core Split view tracer bullet

**Status:** resolved

- [x] Clicking a normal link in either the **original pane** or the **translated pane** navigates the top-level tab to the new URL and automatically opens **Split view** on the new page.
- [x] Clicking a same-page anchor link (`#...`) inside either pane scrolls that pane to the anchor without reloading the tab, and **scroll sync** aligns the other pane when enabled.
- [x] Links opened in a new tab or window (via `target="_blank"` or modifier keys such as Ctrl/Cmd+click or middle-click) open normally in the new tab without navigating or closing the current **Split view**.
- [x] Only navigation messages originating from the extension's own panes in the active tab are acted upon.
- [x] Automated tests verify link interception and classification (full navigation vs. same-page anchor vs. new-tab/modifier click) and top-level tab navigation reopening **Split view**.
