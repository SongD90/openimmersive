# Split view loads the page again in two frames

Turning on split view covers the window with a full-window container holding two frames side by side, each loading the same URL: the original pane on the left and the translated pane on the right, where Replace mode runs. Using a real half-width frame for each side means sites lay themselves out for half the window and keep fixed headers within their own half, and both panes behave the same way for scroll sync. It reuses the existing Replace-mode translation code and keeps the site's own styles, fonts and scripts working. The cost is that the page loads two more times, and anything typed or expanded on the page is reset in the panes.

## Considered Options

- **Squeeze the current page into the left half and frame only the translated pane**: rejected because the site still lays out for the full window width, so its layout is cramped and fixed headers can stretch underneath the translated pane.
- **Copy the page's content into a container and translate the copy**: rejected because the site's scripts don't run in the copy, so menus, lazily loaded images and script-driven layouts break.
- **Pair each block with its translation in two columns on one page**: rejected because it gives one scrollbar, and navigation, bylines and boxes don't line up as two independent pages.

## Consequences

- The content script must also run inside frames, not only the top-level page.
- Sites that refuse to be shown inside a frame need separate handling (see ADR 0002).
- Each pane is a separate page load and can show different content from the other (ads, personalized boxes), so blocks are paired across panes by their original text, not by position.
- The original page stays underneath the container, so turning split view off returns to it unchanged, scrolled to where the original pane was.
