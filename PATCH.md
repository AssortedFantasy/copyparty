# k8p patches

This branch contains the small UI changes used by the copyparty instance on
`k8p`. Keep each change in a cohesive commit so the patch set remains easy to
review and rebase onto upstream.

## Disable gallery transitions by default

The gallery previously defaulted to a sliding transition when the browser did
not request reduced motion. This movement is distracting, so the default is now
the gallery's `none` animation.

An animation explicitly selected and saved by the user is still respected.

## Make gallery navigation reliable

Tapping the empty area around an image used to close the gallery. This was easy
to trigger accidentally when moving between images with different aspect
ratios, because the same screen position could suddenly fall outside the next
image. Empty space now navigates backward or forward according to which half of
the screen was tapped. The gallery can still be dismissed by swiping upward or
using its close button.

The gallery was also built from only the file rows currently rendered in the
browser. Large directories initially render a limited number of rows, commonly
250, so the gallery silently ended there until scrolling caused more rows to be
loaded. Entering grid mode now expands the complete directory listing before
the gallery is built, ensuring its item count covers the entire directory.

## Tighten thumbnail-grid spacing

The upstream grid leaves generous margins, padding, and gaps around thumbnail
tiles. These values are reduced to roughly one quarter of their defaults so
images occupy more of the available screen while retaining upstream's
variable-aspect-ratio row layout.
