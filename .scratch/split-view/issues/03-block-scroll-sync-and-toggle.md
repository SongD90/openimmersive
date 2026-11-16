# 03: Block-by-block scroll sync and toggle

**What to build:** Keep the **original pane** and **translated pane** aligned at the same reading position by syncing scroll position **block** by **block** in both directions by default, with a persisted toggle to turn **scroll sync** off. Pair **blocks** across panes by their original text in order of appearance so ads or personalized elements present in only one pane are skipped without throwing off alignment.

**Blocked by:** 01: Core Split view tracer bullet

**Status:** resolved

- [x] **Blocks** in the **original pane** and **translated pane** are paired by matching their pre-translation text in order of appearance, and unmatched **blocks** are ignored by **scroll sync**.
- [x] Scrolling either pane while **scroll sync** is enabled scrolls the other pane so the corresponding paired **block** and relative offset within that **block** stay aligned, even when translated text is shorter or longer than the original.
- [x] **Scroll sync** guards against feedback loops so programmatic scrolling in the target pane does not bounce scroll events back to the source pane.
- [x] Readers can toggle **scroll sync** on and off, and the preference is saved and restored across sessions.
- [x] Turning **Split view** off restores the underlying page's scroll position to match where the reader was in the **original pane**.
- [x] Automated tests cover pure **block** pairing (identical pages, extra ads/blocks in one pane, repeated text, no matches), pure scroll position calculation, feedback-loop protection, and toggle persistence.
