# Deterministic Reader Bugs

Reproducible reader bugs to aim tests at. Open issues are targets: write a failing test first.
Closed issues are regression guards: the test must stay green. Each issue was opened and read on
2026-10-04; state is as of that date.

The root problem is insufficient test coverage of reader code in jellyfin-web and the server.
Bugs from downstream clients (Android, iOS, Android TV, desktop players) count when their root
cause plausibly lives in that code. List them with the web reader issues they share a cause with.

## EPUB (`bookPlayer`)

- [Save progress on reading epubs](https://github.com/jellyfin/jellyfin-web/issues/3791) (open) - Progress saves only per chapter because every epub.js location in a chapter shares one number, and the CFI is dropped. Test: open mid-chapter, close, reopen, assert the same page and not just the chapter start.
- [Book Position Not Remembered](https://github.com/jellyfin/jellyfin-web/issues/2582) (open) - Reopened books start at the beginning because `startPositionTicks` never reaches the player. Test: core resume flow for the first Playwright PR.
- [Comic / Manga navigation broken for the ePub format](https://github.com/jellyfin/jellyfin-web/issues/3345) (open) - In EPUB 3 manga, the left arrow does nothing while the right arrow goes back. Test: right-to-left fixture, assert both arrow keys move one page in the expected direction.
- [EPUB reader waits for complete location indexing before allowing reading](https://github.com/jellyfin/jellyfin-web/issues/8422) (closed, not planned) - Large EPUBs stay behind the spinner until the full location map is generated, about 20 s for a 171-spine omnibus, and the work repeats on reopen. Test: performance benchmark, time to first readable page with a large fixture.
- [Book (epub) do not show sub-entries in the table of contents](https://github.com/jellyfin/jellyfin-web/issues/4486) (closed) - Nested TOC chapters were hidden; only top-level entries appeared. Test: regression guard, fixture with a nested TOC, assert every level renders and navigates.
- [EPUBs are not readable with new dark mode](https://github.com/jellyfin/jellyfin-web/issues/3930) (closed) - Dark theme rendered black text on a dark background. Test: visual regression and contrast check on the EPUB page in dark theme.
- [Swiping on the Book Player does not navigate the pages of the Book](https://github.com/jellyfin/jellyfin-web/issues/6160) (closed) - Swipe left and right did nothing on mobile. Test: touch-emulated swipe on a mobile viewport, assert the page changes.
- [Book Player top bar is missing navigation buttons](https://github.com/jellyfin/jellyfin-web/issues/6161) (closed) - The code expected previous and next buttons that the template never rendered. Test: regression guard, assert the buttons exist and turn pages.
- [Can't read a book if user doesn't have download privileges](https://github.com/jellyfin/jellyfin-web/issues/1394) (closed) - EPUB loaded forever for users without download permission. Test: open as a no-download user, assert the book renders.

## Comics (`comicsPlayer`)

- [.cbz files aren't loaded per page](https://github.com/jellyfin/jellyfin-web/issues/3447) (open) - The whole archive downloads before any page shows, so large CBZs take 1-2 minutes to open. Test: performance benchmark, time to first page with a large CBZ.
- [Book player doesn't warn, or provide info, when comics will not load due to user not allowed to download media](https://github.com/jellyfin/jellyfin-web/issues/3698) (open) - Users without download permission get a blank page with no message. Test: open a comic as a no-download user, assert an error message is shown.
- [Comic Player uses metadata and other non-page files for reading position calculation](https://github.com/jellyfin/jellyfin-web/issues/3352) (closed) - A `ComicInfo.xml` inside the archive shifted the start page. Test: regression guard, fixtures with and without `ComicInfo.xml`, assert the same start and resume page.
- [Page Order issues with CBZ files containing folders.](https://github.com/jellyfin/jellyfin-web/issues/6642) (closed, not planned) - Pages inside chapter folders come out of order. Test: fixture with nested folders, assert page order. Expected to fail today.
- [Comic / Manga do not scale properly on resize event](https://github.com/jellyfin/jellyfin-web/issues/4498) (closed, not planned) - Resizing the window breaks page scaling until the original size returns. Test: resize the viewport mid-read, assert the page fits. Likely still failing.

## Mobile comic fit (downstream: Android)

The Android app renders jellyfin-web in a WebView, so these likely share a root cause in
`comicsPlayer` fit and zoom handling. That is inferred, not confirmed in the threads. All
reproduce as a page cropped on a phone-sized viewport with no way to pan.

- [Comic book pages overlap when trying to read](https://github.com/jellyfin/jellyfin-web/issues/5245) (closed) - On Android, pages zoom to fill the screen and crop about a third, and panning turns the page instead. Test: mobile viewport, assert the whole page is visible and a pan does not change the page.
- [CBZ fit width issue](https://github.com/jellyfin/jellyfin-web/issues/3861) (closed) - CBZ pages open fit-to-height with overlap and no zoom out. Test: assert fit mode on open at portrait and landscape sizes.
- [Manga reader zoomed in, cant fit to screen](https://github.com/jellyfin/jellyfin-web/issues/4508) (closed, not planned) - Manga is fixed to page height, cropping the right side, and zooming out snaps back. Test: pinch-out on mobile, assert zoom holds. Likely still failing.
- [comic books get zoomed in too far when viewed on mobile](https://github.com/jellyfin/jellyfin/issues/11084) (closed, not planned) - Same crop-and-pan-turns-page behavior on CBZ, filed in the server repo. Test: covered by the 5245 test.
- [Comic / Manga reader doesn't fit screen correctly](https://github.com/jellyfin/jellyfin/issues/8274) (closed) - Reproduced in an Android browser too, worse on two-page spreads. Test: add a two-page spread fixture to the mobile fit test.
- [CBZ comic books opens completely zoomed in, no way to zoom out to read the page.](https://github.com/jellyfin/jellyfin/issues/4808) (closed) - CBZ opened zoomed to the top-left corner in desktop Firefox and Android, while PDFs were fine. Test: regression guard, desktop viewport fit check for CBZ.

## PDF (`pdfPlayer`)

- [[10.7.0 RC2] PDF Reader full screen & Next PDF](https://github.com/jellyfin/jellyfin-web/issues/2329) (open) - PDFs force fullscreen, and the reader does not advance to the next PDF in the folder. Test: assert fullscreen state on open, and next-item behavior with a multi-PDF folder.
- [Cannot read PDF with capital extension](https://github.com/jellyfin/jellyfin-web/issues/7126) (closed) - Files ending in `.PDF` would not open. Test: regression guard, fixture named `*.PDF`, assert the page renders.
- [PDF display is too large](https://github.com/jellyfin/jellyfin-web/issues/6288) (closed) - PDFs opened zoomed in, showing only part of the page. Test: visual check that the first page fits the viewport at desktop and mobile sizes.
- [PDF reader does not use dark theme](https://github.com/jellyfin/jellyfin-web/issues/3929) (closed) - The close button was invisible in the dark theme. Test: assert the reader controls are visible and focusable in dark theme.

## Cross-reader

- [[bug] Song track get skipped when a book is closed](https://github.com/jellyfin/jellyfin-web/issues/8321) (closed, not planned) - Closing a book skips the music track playing in the background. Test: play audio, open and close a book, assert the same track is still playing. Expected to fail today.

## Server (`jellyfin`)

- [Wrong MIME type returned for different comic book formats](https://github.com/jellyfin/jellyfin/issues/11009) (closed) - Every comic archive was served as `application/x-cbr`. Test: unit test in `MimeTypeTests.cs` for cbz, cbr, cb7, and cbt.
