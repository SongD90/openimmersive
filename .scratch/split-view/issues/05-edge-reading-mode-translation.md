# 05: Direct Reading Mode side-by-side translation (`img/readModeTranslation.png`)

**What to build:** Support translating articles directly in a side-by-side Reading Mode view matching `img/readModeTranslation.png` (clean distraction-free Reader Mode on both the **original pane** and **translated pane** without falling back to the normal website layout or losing Edge Reading Mode fidelity).

**Blocked by:** 01: Core Split view tracer bullet

**Status:** pending

- [ ] Entering or triggering translation from Reading Mode renders both the **original pane** (left) and **translated pane** (right) in a faithful Reading Mode layout matching `img/readModeTranslation.png` rather than switching back to the normal website view.
- [ ] Preserves Reading Mode typography, paper background (`#f8f4ec`), lead image + caption, and article body structure across both panes.
- [ ] Keeps **scroll sync** and inline link preservation working across both Reading Mode panes.
