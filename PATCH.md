# k8p patches

This branch contains the small UI changes used by the copyparty instance on
`k8p`. Keep each change in a cohesive commit so the patch set remains easy to
review and rebase onto upstream.

## Disable gallery transitions by default

The gallery previously defaulted to a sliding transition when the browser did
not request reduced motion. This movement is distracting, so the default is now
the gallery's `none` animation.

An animation explicitly selected and saved by the user is still respected.
