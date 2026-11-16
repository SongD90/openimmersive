# 02: Frame-blocking header removal and Bilingual fallback

**What to build:** Enable **Split view** to load sites that normally block framing (`X-Frame-Options` or `Content-Security-Policy` `frame-ancestors`) by dynamically removing those headers only for the extension's **Split view** frames in the active tab, and cleaning up those rules as soon as **Split view** is turned off or the tab closes. If the **translated pane** still cannot load (network failure, frame-busting script, or readiness timeout), automatically exit **Split view**, fall back to **Bilingual** on the page, and display a short notice to the reader.

**Blocked by:** 01: Core Split view tracer bullet

**Status:** resolved

- [x] Starting **Split view** activates header-removal rules scoped strictly to subframes in the active tab so `X-Frame-Options` and `Content-Security-Policy` `frame-ancestors` do not block the **original pane** or **translated pane**.
- [x] Turning **Split view** off or closing the tab removes the header-removal rules for that tab so no other pages or tabs lose clickjacking protection.
- [x] If the **translated pane** reports a load error or fails to become ready within the timeout, the **Split view** container is removed, the page translates using **Bilingual**, and the reader sees a brief notice explaining the fallback.
- [x] Automated tests verify rule registration and cleanup in the background context and fallback-to-**Bilingual** behaviour in the content context.
