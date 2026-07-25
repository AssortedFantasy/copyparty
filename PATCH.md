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

## Add a filename-free thumbnail mode

A new `no names` grid control sits beside the existing crop and 3x controls.
It is enabled by default and persists in browser storage. When enabled, both
the filename and its surrounding chin are removed from image tiles. Folders
and non-image files retain their labels. Turning the setting off restores the
normal image filename display and chop controls.

## Keep mobile thumbnail long-presses from opening the drop overlay

On touch devices, holding an image thumbnail to use the browser's link menu
could also begin a native image/link drag. Copyparty interpreted that as an
incoming upload and displayed its upload/search drop overlay behind the menu.
Grid thumbnail images, their links, and full-size gallery preview images are
now non-draggable on touch devices. Thumbnail links remain normal links so
actions such as opening them in a new tab continue to work. Desktop dragging
is unchanged.

## Restore gallery scroll positions reliably

Copyparty previously kept only one global saved scroll position, so navigating
through nested image folders overwrote the position of the parent folder.
Grid-folder navigation also bypassed that saving logic entirely. Returning
with browser Back could therefore leave the previous folder at an unrelated
position.

Scroll state is now stored independently for each folder and records the first
visible tile plus its viewport offset. Restoration waits until the destination
grid has been rebuilt, then keeps that tile anchored while slow, variable-size
thumbnails above it finish loading. A deliberate touch or wheel action cancels
the correction immediately. Native browser scroll restoration is disabled so
it does not compete with copyparty's single-page navigation.

## Reserve uncropped thumbnail dimensions before loading

When multimedia indexing provides an image's `res` metadata, the grid now uses
that aspect ratio to calculate the thumbnail's display height before requesting
the thumbnail itself. This prevents slow, uncropped thumbnails from changing
row heights as they load and does not depend on the thumbnail cache.

The indexed ratio is only a pre-load fallback. Once decoded, the thumbnail's
intrinsic aspect ratio takes precedence, avoiding distortion when metadata is
incorrect or does not account for image rotation. Images without valid
resolution metadata retain the existing placeholder behavior, and cropped
thumbnail mode is unchanged.
