# Split view removes frame-blocking headers for its own frame

Many sites send headers (`X-Frame-Options`, or `frame-ancestors` in `Content-Security-Policy`) that stop the browser from showing them inside a frame, which would leave the translated pane blank. Split view removes these headers using `declarativeNetRequest` rules, but only for the frame the extension creates for the translated pane, in that one tab. This deliberately switches off the site's clickjacking protection inside that frame, and adds one install-time permission, so that split view works on the sites people most often want to read.

## Considered Options

- **Fall back to Bilingual on these sites**: rejected as the main approach because it would rule out split view on most news and social sites. It is kept as the backup: if the frame still fails to load after the headers are removed, the page switches to Bilingual and shows a notice.
- **Fall back to copying the page's content on these sites**: rejected for the same reasons as in ADR 0001.
