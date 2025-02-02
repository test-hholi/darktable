We're proud to announce the new feature release of darktable, 4.4.0!

The github release is here: [https://github.com/darktable-org/darktable/releases/tag/release-4.4.0](https://github.com/darktable-org/darktable/releases/tag/release-4.4.0).

As always, please don't use the autogenerated tarball provided by
github, but only our tar.xz file. The checksums are:

```
$ sha256sum darktable-4.4.0.tar.xz
??? darktable-4.4.0.tar.xz
$ sha256sum darktable-4.4.0.dmg
??? darktable-4.4.0.dmg
$ sha256sum darktable-4.4.0.exe
??? darktable-4.4.0.exe
```

When updating from the stable 4.2.x series, please bear in
mind that your edits will be preserved during this process, but the new
library and configuration will no longer be usable with 4.2.x.

You are strongly advised to take a backup first.

#### Important note: to make sure that darktable can keep on supporting the raw file format for your camera, *please* read [this post](https://discuss.pixls.us/t/raw-samples-wanted/5420?u=lebedevri) on how/what raw samples you can contribute to ensure that we have the *full* raw sample set for your camera under CC0 license!

Since darktable 4.2:

- ??? commits to darktable+rawspeed
- ??? pull requests handled
- ?? issues closed


_Please note that the darktable documentation is not currently complete for release 4.4
and contributions are greatly appreciated. Please see the
[project documentation](https://github.com/darktable-org/dtdocs#contributing)
for more information on how to contribute._

## The Big Ones

The following is a summary of the main features added to darktable
4.4. Please see the user manual for more details of the individual
changes (where available).

- It is now possible to define multiple automatically-applied presets
  against a single processing module. Each preset after the first will
  be added to a new module instance immediately after the first
  instance in the pipeline.

  To better visualise which module instance corresponds to which
  preset, the module label will be automatically set to the name of
  any preset that matches the current module's parameters. If you
  subsequently alter those parameters, the label will be cleared,
  unless its parameters match to another preset, in which case it will
  be changed to the name of the matched preset. If the module label
  has been hand edited it will never be updated automatically.

  A new option (preferences > darkroom > automatically update module
  name) has been introduced to allow this functionality to be
  disabled.

- The default parameters of some processing modules have been reworked
  to allow them to be more easily used in copy/paste, presets and
  styles.

- Many modules have default parameters based on image metadata or
  current workflow, for example:

  - exposure: in a scene-referred workflow, the exposure is adjusted
    using the Exif exposure compensation value,
  - denoise (profiled): set based on camera and ISO,
  - lens correction: set based on camera, lens, focal length, focus distance,
  - base curve: set based on the camera maker,
  - white balance: set based on Exif metadata,
  - orientation: set based on Exif metadata,
  - color calibration: set based on Exif metadata

  For all of these modules it is now possible to paste settings while
  ensuring that the proper image metadata is used to set the module
  parameters.  This is achieved by selecting the "Reset" column in the
  preset and style dialogs, which makes the module behave as if its
  reset button has been clicked.

  When creating or editing presets, a new option, "reset all module
  parameters to their default values", has been added. Choose this
  option to auto-add modules to matching images while retaining their
  default parameters (including any set based on image metadata).

  This functionality has allowed us to clean up a number of
  module-specific hacks that previously achieved similar functionality
  (albeit in a more limited way).

- The default workflow configuration option (preferences > processing
  > auto-apply pixel workflow defaults) has now been adjusted to
  incorporate the previous chromatic adaptation workflow setting and to
  include the new sigmoid module.  Options are now as follows:

  - scene-referred (filmic) -- default
  - scene-referred (sigmoid)
  - display-referred (legacy)
  - None

  With these new options, the color calibration module and
  scene-referred module order will be automatically applied for all
  workflows except "display-referred (legacy)", which will continue to
  use only the white balance module for chromatic adaptation. The
  value in brackets represents the default module used for tone
  mapping (filmic/sigmoid). In the legacy mode, base curve will be
  used for tone mapping and in "none" mode, no tone mapping module
  will be applied.

- The scopes module now has a new color harmony overlay option in RYB
  vectorscope mode. You can choose from 9 different color harmonies:

  - Monochromatic
  - Analogous
  - Analogous complementary
  - Complementary
  - Split complementary
  - Dyad
  - Triad
  - Tetrad
  - Square

  These will show as overlays on top of the vectorscope to tell you
  where the "harmonic" colors are. You can rotate the overlays by
  scrolling with your mouse.

  Color harmony guides can be used along with the global color picker
  module to guide you in shifting your image's colors to fit a given
  color harmony -- think of them like crop composition guides, but for
  colors.

- A global <kbd>right-click-and-drag</kbd> operation has been added to
  allow image rotation to be corrected without first opening the
  rotate and perspective module. This operation can be used as long as
  the currently-focused module does not already use that shortcut for
  another purpose.

- Drawn mask functionality has been completely overhauled with a large
  number of changes and fixes:

  - The "brush smoothing" and "pen pressure" options have been moved
    from the global preferences dialog to a new collapsible
    "properties" section in the mask manager, so that they can be
    changed while drawing and can be assigned shortcuts.

  - The drawn mask shape size/feather/hardness sliders in the mask
    manager now use a logarithmic scale and scrolling over them makes
    relative adjustments, just like <kbd>Shift+scroll</kbd> over the
    shape itself. As with other sliders, <kdb>Ctrl</kbd> or
    <kdb>Shift</kbd> can be used to make fine or coarse adjustments
    (similarly with shortcut fallbacks enabled). Shortcuts assigned to
    the sliders can be used to adjust brush size/hardness while
    drawing.

  - A fifth set operator has been added to the mask manager to allow
    drawn shapes to be combined in "sum" mode. This allows repeated
    brush strokes with low opacity to be layered on top of each other
    to increase the strength of the mask. This mode is now the default
    for brush shapes.

  - It is now possible to change the set operator (mode) for all
    shapes in a group from the right-click menu in the mask manager.

  - Some actions in the mask manager menu previously could be
    activated even though they would have no impact on the image in
    some contexts. The move up/down actions are therefore now disabled
    for the first and last element in a group respectively, and it is
    no longer possible to choose a set operator (mode) for the first
    element in a group.

  - The sort order of the shapes in mask manager groups has been
    reversed so that the lowest ranking shape is at the bottom of the
    group. The sort order of shapes outside of a group has also been
    changed for consistency.

  - It is now possible for all mask types to be added continuously in
    the mask manager.

  - Fixed an issue where the brush was not properly displayed after
    being created from the mask manager, and a crash when creating
    gradients from the mask manager has also been fixed.

  - For all shapes the editable state is now properly set after
    creation making it possible to move and resize the different parts
    of the shapes.

  - Fixed a display issue when editing a shape name in the mask manager.

  - Fixed the state of the set operators in the mask manager -- when
    moving a mask up/down we now ensure that the first mask has no
    operator and that the second one always has an operator assigned.
    If no operator has been set yet the default (union) operator is
    used.

  - When using a shortcut to add shapes to a drawn (blending) mask the
    blending mode will automatically switch to "drawn mask" or "drawn &
    parametric mask", depending on what it was before, so that any newly
    created shape will actually affect the image.

  - In the drawn mask blending mode there used to be an "invert mask"
    option that had the same functionality as the "toggle polarity" option.
    Since both were doing the same thing, "invert mask" is now removed.

  - The drawing of masks has been reworked to ensure that all types of
    masks are drawn in the same way -- the central area, border, and
    highlighted segments are now displayed consistently. The highlighted
    segment is now more visible, especially for the brush mask, where
    the highlighted segment was barely distinguishable due to a bug.

  - In the style and copy/paste dialogs a new column has been added to
    show whether a module uses a mask (drawn/parametric/raster) using
    the standard mask icon.

  - Editing of drawn masks has been improved -- it is now easier to
    select masks' control points and path segments (in some cases it was
    easy to accidentally select the whole path mask rather than a single
    segment). On-canvas mask rendering has also been improved for better
    consistency between shape types.

  - The brush path is now slightly more transparent in order to better
    see the underlying image.


## Performance Improvements

One of the major highlights of this release is the number of performance
improvements and enhancements:

- The following processing modules have had code cleanup and performance
  improvements. All SSE2 code paths have been removed (the optimized
  parallel code generated by the compiler is faster) or code optimized,
  leading to speed gains of 5-40%:

  - dithering (now called dither or posterize -- see below)
  - graduated density
  - input color profile
  - color look up table
  - framing
  - surface blur
  - vignetting
  - retouch
  - denoise (profiled)
  - invert (deprecated)
  - local contrast (local Laplacian mode)
  - lowpass
  - rgb levels
  - input color profile
  - lowlight vision
  - velvia
  - split-toning
  - negadoctor
  - color calibration (CIECAM16, XYZ, and linear/nonlinear Bradford).
  - filmic (legacy -- deprecated)
  - filmic rgb (including highlight reconstruction)
  - color balance (legacy)
  - color balance rgb
  - levels (legacy -- deprecated)
  - fill light (deprecated)
  - liquify
  - color mapping
  - highpass filter
  - shadows and highlights
  - lens correction
  - grain
  - monochrome
  - highlight reconstruction (inpaint opposed mode)

- Improvements to a number of core routines have also led to further
  speed improvements:

  - The interpolation algorithms (Bicubic, Bilinear, Lanczos2,
    Lanczos3) -- used by "rotate and perspective", "crop and rotate
    (deprecated)" and for resizing the output.

  - The gaussian generator -- used by censorize, denoise (profiled),
    lowpass, diffuse or sharpen, defringe, RAW chromatic
    aberrations, base curve, perspective correction, filmic RGB,
    retouch, tone equalizer and zone System (deprecated)

  - The box blur filter -- used by focus peaking, the guided filter
    for blending, the new highlight recovery algorithms, and the
    bloom, highpass, haze removal, and soften modules.

  - The Edge-Avoiding a-trous Wavelet -- used by contrast
    equalizer and denoise (profiled).

  - Some parts of the bilateral filter -- used in monochrome,
    lowpass Filter, shadows and highlights, censorize, retouch, color
    mapping, rotate and perspective, and local contrast.

  - All the blending modes in Lab & RGB for the display-referred and
    scene-referred workflows.

  - The luminance mask calculation for the tone equalizer.

  - The loader for the JPEG2000 file format.

  - The color adaptation matrices have been transposed to allow for
    vectorization.

  - The "acquire clusters" operation in the color mapping module has
    been sped up by a factor of 30 to 200, making the results
    perceptually instantaneous on clicking the button.

- Pixelpipe code and caching strategies have been rewritten with
  significant performance gains when developing in the darkroom.

- OpenCL support has been added to the sigmoid module.

- OpenMP support has been added to the XCF export and RGBE loader.

- The performance of the highlight reconstruction inpaint opposed
  algorithm has been improved by providing an OpenCL implementation
  and using internal caching in the darkroom.

- The guided Laplacian highlight reconstruction mode is now less
  memory hungry (with a saving of around 40%) and its performance is
  significantly improved, allowing for more iterations of recovery to
  be applied by default.

- Successive changes to sliders and other widgets (for example by
  dragging, scrolling or using shortcuts) have been made more
  responsive by creating fewer undo records. This also makes using
  undo/redo more effective because you are no longer forced to step
  through every micro-change.

## Other Changes

- The filter section of the preset dialog has been reworked to better
  show the relationship between raw/non-raw and HDR/monochrome/color.
  This should avoid the accidental creation of presets that can never
  apply to any image.

- ISO range selection has now been improved within the auto-apply
  presets dialog.

- Shortcuts assigned to presets or styles will now be shown when
  hovering over them in the relevant menu.

- Long-left-clicking a preset will now keep the preset menu open so
  you can quickly switch between several presets to visualise the
  effect. You can also scroll over the preset button to switch to
  previous/next presets (like you already could using shortcuts).

- Two new sharpness presets have been added to the diffuse or sharpen
  module -- standard sharpness and one with a stronger effect.

- The color picker code has been completely overhauled resulting in
  speed improvements due to code paths not being run unnecessarily.
  Code is now tuned for contemporary processors, using OpenMP features
  for more succinct code. A time-consuming denoise pass is now only
  executed when used from the filmic module (in which case removing
  noise makes the automatic tuning more robust). A monochrome image
  warning has been removed, along with additional code cleanup,
  de-duplication and optimization.

- All remaining color-picker buttons are now accessible via shortcuts
  and Lua scripts.

- Histogram calculation code has been modernized, removing SSE code
  paths.  While this doesn't itself provide any speed improvements, it
  is used as a model for optimized code using recent OpenMP features.
  Various pieces of unused code have been removed to provide a
  consistent internal API. In certain cases this code will produce
  marginally more accurate results, and in some cases will use
  substantially less memory.

- The histogram gui has been reworked. Control buttons have been split
  into two groups: On the left side, a series of buttons to switch
  between histogram modes (histogram, waveform, rbg parade,
  vectorscope); On the right side, the buttons that control the
  parameters of each mode (RGB Channels, orientation,
  vectorscope). For the RYB vectorscope, a series of buttons have also
  been added to visualize guide lines for the most common color
  harmonies.

- The zoom widget in the navigation window has been converted to
  a standard drop-down, better fitting the darktable style.

- The scroll zoom logic in the darkroom has been reworked in order
  to make the zoom steps more perceptually-uniform for all image sizes.

- A new option (preferences > darkroom > middle mouse button zooms to
  200%) has been added to control how the middle-mouse-click zoom
  toggle behaves in the darkroom. Select this option to toggle between
  fit, 100%, and 200%; disable the option to only toggle between fit
  and 100%.  In the latter case, you can access 200% zoom with
  <kbd>Ctrl+middle-click</kbd>.

- Snapshots are no longer invalidated when the history is compressed
  or reset. All snapshot are now stored with their full history and
  can always be correctly reconstructed.

- The snapshot list view been redesigned, bringing its display in line
  with that of the history module. At the same time the module's label
  is now shown in the list and is editable with <kbd>Ctrl+click</kbd>.

- The following modules have been deprecated and will be removed for
  selection in a future version (though they will continue to be
  available for old edits):

  - levels (use rgb levels instead)
  - contrast brightness saturation (use color balance RGB instead)

- The ISO 12646 border size was too small in the previous version and
  this has been fixed.

- The "default" module group has now been removed and you are now
  advised to use one of the scene-referred module groups instead.

- Support has been added for loading QOI and FITS images, and for
  writing metadata to XCF format (see notes below).

- The ability to read Exif metadata from AVIF, HEIC and JPEG XL images
  using native libraries (if not supported by Exiv2) has been added.

- Exif data is now written to the Exif PNG chunk when using an Exiv2
  version 0.28.0 and newer -- this is the new standard way to store
  Exif data in PNG files.

- Masks are now exported as extra channels in EXR files.

- Loading of BigTIFF images is now re-enabled, and attempts to use
  the native LibTIFF-based reader first.

- Export and thumbnail generation has been redesigned to remove some
  hacks that had accumulated and should result in a better export size.

- Various improvements have been made to the debug interface when
  running darktable from the command-line:

  - `-d common` outputs most valuable information and should now be
     used for bug reports instead of `-d all`.

  - `--bench-module <modulea,moduleb>` performs runtime benchmarking
      of the specified modules.

  - `--dump-pipe <modulea,moduleb>` writes input and output data of
    the specified modules as pfm files for inspection.

- Support for lens correction using embedded metadata has been improved:

  - Support for dng files has been added,
  - Scale and chromatic aberration correction can now be fine-tuned,
  - An auto-scale button has been added,
  - Overall module performance has been improved by approximately 8%.
  - An improved algorithm for embedded metadata lens correction has
    been added, providing better distortion and vignetting
    corrections for supported FujiFilm and Sony images.
  - Added support for Fujifilm X-Trans I/II/III raw files.

- Section headers have been added to the "sort by" drop-down in the
  top panel (files, times, etc).

- When the crop module receives focus and switches to an uncropped
  view of the image, the crop handles around the edges of the image
  now briefly light up to indicate that they can be
  clicked-and-dragged.

  As long as the crop covers the full image (and moving it by dragging
  is therefore impossible) the sensitive zone for dragging corners and
  edges is enlarged.

- While the crop module shows the whole image to facilitate making
  adjustments, it will no longer trigger a pipe recalculation until the
  module loses focus (for example by switching to another module or by
  collapsing the crop module) at which point the new crop will be used
  to resize the image. If shortcuts are used to make changes to the
  crop without focusing the module, these will still be implemented
  immediately.

- Removed the commit button from the crop module as it was no longer
  used.

- The height of resizeable module areas can now be changed by clicking
  and dragging the bottom of the resizeable area.  The previous method
  to achieve this, by scrolling while holding the control key, has
  been changed to <kbd>Shift+Alt+scroll</kbd> (and a note added to all
  tooltips). This frees up <kbd>Ctrl+scroll</kbd> for fine-tuning
  changes in RGB Levels or the histogram (to change exposure or black
  level). In the navigator preview <kbd>Ctrl+scroll</kbd> now adjusts
  zoom level without bounds, as it already does over the central image
  area.

- The module instance name in the darkroom has been altered so it is
  more clearly separated from the module name (using a "bullet"
  separator).  The label name in the history module has also been
  updated accordingly.

- The display of the range rating widget in the collection filters
  module and top panel has been improved to give it better
  contrast/icons, which should make it more easily readable.

- The search filter in the collection filters module and top panel
  has been improved to also allow searching for camera brand
  and model.

- In the lighttable view, the "hold" and "sticky" preview shortcuts
  are now linked to the same action -- previously there were two
  "toggle sticky preview mode" actions, one with and one without focus
  detection. Focus detection can now be selected (in shortcuts) via an
  element, and hold/toggle via an effect. All mapped shortcuts are
  shown in the tooltip of the preview layout button.

- A full copy-and-paste is now always performed in overwrite mode
  (replacing the history of the target image), since append mode is
  often the less appropriate choice -- for example, for some modules
  it often makes no sense to add extra instances.

- The style tooltip now immediately shows module details while waiting
  for the preview image to be calculated.

- The tooltips in the liquify module's shape tools are now consistent
  with the blending drawn mask tools.

- The "demosaicing for zoomed out darkroom mode" preference has been
  removed.  This option is no longer necessary due to improvements in
  the pixelpipe cache and previously could have led to slight
  differences in darkroom processing.

- The Exif focus distance field is now read for images taken with
  Nikon Z bodies.

- When reading creator metadata from IPTC Information Interchange
  Model, darktable now prefers By-line over Writer/Editor. Date/time
  and description metadata are read from commonly-used properties.

- The rendering of the arrow that joins source and target shapes in
  the retouch module has been reworked to be more consistent between
  shapes.  All source/target shapes are now linked with an arrow
  rather than a simple line and are rendered in such a way as to avoid
  crossing over the shapes themselves (by connecting the closest
  source/target borders).

- The full-frame-equivalent focal length and crop factor is now shown
  alongside the actual focal length in the image information module.

- New options have been added to the watermark module for more
  fine-grained control over scaling. In conjunction with the new
  'fixed-size-text' template it is now possible to insert text with
  constant font size.

- Encoder ring and button lights of the Behringer X-Touch Compact are
  now supported via midi. Unmapped encoder presses fall back to reset
  the encoder.

- Midi buttons mapped to the reset effect of a slider or combo (either
  directly or via fallback, like the row below the faders of the
  X-Touch Compact) now light up if the current value is not the default.

- Resetting a combobox that has sub-headers (by double clicking or via
  a shortcut) will now choose the first selectable item.

- Requests to switch images in the darkroom (via
  space/backspace/filmstrip) used to be quietly ignored if a
  recalculation was currently ongoing.  Now, they will be processed as
  soon as the pipe is ready.  Any changes that were made to the
  previous image while waiting will be discarded.

- Manual per-color black point adjustment is now possible for non-CFA
  (a.k.a. linear) raw images. Note that file-embedded levels might
  still not be set automatically on import.

- The dithering module now also includes posterization modes and
  masking, and is therefore renamed to "dither or posterize" to make
  the new functionality more discoverable.

- Help buttons have been added to several dialogs and preference tabs
  to allow direct access to the relevant online manual entries.

- A new version of fimic color science -- v7 (2023) -- has been added
  and is now the default. This version replaces the color preservation
  drop-down with a slider to control the saturation of the highlights.
  This slider is used to control a mixture between the previous "max
  RGB" and "no preservation" modes.

- It is now possible to import the following additional metadata keys:
  - Iptc.Application2.Byline
  - Iptc.Application2.DateCreated
  - Iptc.Application2.TimeCreated
  - Exif.Image.ImageDescription

- The shadows and highlights module now uses the bilateral filter by
  default as this avoids the halos common to the previous (Gaussian)
  default.

- Some new aspect ratios have been added to the framing module:
  - CinemaScope
  - US Letter
  - US Legal
  - Standard print sizes (5x7, 8x10, 11x14)

- A new tooltip has been added to the edges of sliders with soft
  limits describing how to set values outside those boundaries.

- When deletion of a physical file (or movement of that file to trash)
  fails, the clarity and usability of the "further action"
  confirmation dialog is now improved.

- It is now possible to see the current image's embedded ICC profile
  as a tooltip in the input profile module.

- A warning message is no longer shown when multiple color calibration
  module instances are used with masks. This approach is commonly used
  to handle multiple light sources and is considered to be correct
  usage of the module.

- It is now possible to calibrate colors using the Spyder Checkr Photo
  color checker in the color calibration module.

## Bug Fixes

- Fixed an issue where the sort order in the top panel was reset to
  'filename' on every collection change.

- Fixed an issue whereby modules were not always reset to their
  initial state when pressing the reset button. This fix is related to
  the rework of the auto-application of default parameters described
  above.

- XMP regions (e.g. from camera face recognition) are now properly
  transformed from metadata to ensure they match the image.

- Fixed some rounding issues in the calculation of the borders in the
  framing module. Borders on opposite sides are now created with the
  same size.

- Fixed entering a custom aspect ratio in the framing module.

- Code maintenance and bugfixes have been made for writing dng files
  in the "Create HDR" functionality

- Fixed some pixelpipe cache issues related to mask visualization and
  internal module histograms (e.g. within RGB curve). This
  ensures a better hit in the cache leading to better performance, and
  also avoids some refresh issues.

- Fixed calling darktable with `--threads n` so that it does
  not permit using more OMP threads than are available on the host.

- The raw chromatic aberrations module has been amended to always use
  the entire image, and so now works correctly at all darkroom zoom
  levels.

- An issue with setting the active item in a combox having varying
  content using a shortcut (or lua's `dt.gui.action`) has been
  fixed. It is also now possible to directly set the values of the
  comboboxes for the focused module's blending mode etc. by setting
  the shortcut's effect.

- The module presets menu has been fixed to allow the currently-used
  preset to be updated in more situations. For example after entering
  the darkroom and modifying some module parameters the "update
  preset" entry was not previously selectable and the user had to
  first select the preset and then update the parameters again.

- The color picker sample area calculation has been fixed to ensure
  that at least one pixel is always selected. At large zoom levels and
  with a very small area some rounding errors previously could result
  in an empty area selection and therefore an incorrect color sample.

- The "ignore EXIF rating" import option has been fixed for images
  containing the XMP.xmp.Rating tag. This does not change the rating
  if an XMP file is found with some specific rating already entered.

- Minor memory leaks have been fixed in some modules.

- Fixed a possible crash when selecting the original module history
  state and compressing the history stack.

- Fixed a possible crash in gradient mask creation due to an issue in
  the implemented parallelism.

- Fixed an issue with the "look for updated XMP files on startup"
  option that caused mismatches to be incorrectly reported every time
  darktable was started up. The fix ensures that the XMP and database
  timestamps are correctly aligned.

- The placement of the brush correction tool has been fixed in the
  retouch module. This issue was more visible on images that had been
  distorted by other modules earlier in the pipe.

- Some differences between CPU and OpenCL output have been fixed in
  the PPG and VNG/VNG4 demosaicers, as well as the match greens and
  color smoothing options.

- The (hidden) final scale module now properly uses the same
  user-defined scaling mode for image and masks.

- Fixed import of Nikon camera make and model Exif so that opening the
  image in the darkroom is no longer required, and import now also
  works for non-raw files.

- Fixed an issue where Canon CR3 metadata crop was not being ignored.
  The full visible sensor area (as determined by LibRaw) is now always
  used on new imports.

- Fixed to give priority to XWayland when using Wayland, since use of
  native Wayland is the cause of many issues in darktable.

- When using the spot exposure mapping mode, properly reset the mode
  to "correction" when changing image.

- Fixed a bug where the highlight reconstruction module could be
  enabled on images for which it cannot be used, such as JPEG files.

- Fixed a border issue in the inpaint opposed highlight reconstruction
  algorithm, whereby some pixels on the border of the image were not
  correctly handled by the algorithm. This may lead to a small
  difference on the border of the image and will avoid some possible
  reddish borders.

- Fixed an issue in the segmentation highlight reconstruction
  algorithm, whereby the mask display could be broken due to the
  module accessing some uninitialized data.

- Fixed to avoid writing to an XMP file if it was not requested and
  the image was not altered. This rule is now also properly applied
  when importing RAW + JPEG.

- Fixed to make sure the database timestamp is always set when an XMP
  sidecar file might be written.

- A workaround was implemented for the mouse hover effect over sliders
  and dropdowns, which used to cause the whole side panel (including
  the histogram) to be redrawn on each mouse movement between
  widgets. This has been fixed and should result in lower CPU
  consumption.

- In the rotate and perspective module, if the current rotation is
  close to ±180 degrees, adjusting it by drawing a horizon line with
  <kbd>right-click+drag</kbd> could lead to it being clipped at the
  end of the slider. This has been fixed so that the rotation angle
  correctly wraps around when it reaches ±180. A manually-entered
  value outside the range (like 182) will be similarly wrapped (to
  -178).

- Fixed loading of monochrome images.

- Fixed the display of tiny circular masks to ensure that the mask is
  always visible.

- Fixed the loading of the OpenCL library when the required symbols
  are not fully implemented.

- The imported EXR image size is now set to the extent of the valid
  data window only.

- Collection sort names are now properly translated in the recent
  collection sort history pop-up.

- Fixed dual demosaicing options for 4-color Bayer sensor cameras
  where only VNG4 and PassThrough are supported.

- Fixed to no longer truncate focal length on thumbnails to avoid loss
  of display precision.

- Fixed a possible crash caused by use of detail masks when switching
  to the darkroom.

- Fixed feathering masks in lens correction, retouch, liquify,
  and spot removal modules.

- Fixed some rare cases where masks were not displayed when trying to
  edit them after just starting darktable or changing module group.

- Fixed slideshow issues on HiDPI displays.

- Fixed crashes when using raster masks after reordering the pixelpipe.

- Fixed use of details mask in blown-out parts of the image.

- Allow adding color patch on 7x7 grid of the color checker module.

- Feathering input fixed when using distorting modules like retouch
  or lens correction.

- Fixed a long-standing potential memory bug in the interpolation
  code, though one which never has occurred due to how that code is
  used in darktable.

- Reworked the metadata editor to prevent possible data loss.

- Fixed import of auto-applied presets where the upper bound of ISO,
  aperture and exposure could be incorrectly set as the lower bound.

- Fixed the pin icon update in the collection filters module, which
  could crash darktable when using some specific filter combinations.

- Fixed the rating toast message not being shown when rating a
  collapsed group of images using a keyboard shortcut.

- Fixed a possible crash in the astrophoto denoise module when used on
  CPU (not an issue for OpenCL).

- Fixed some minor issues with the Spyder Checkr 48 (v2 - after 2018)
  reference values.

- Fixed a possible crash in the Edge-Avoiding a-trous Wavelet when
  handling very small image regions.

- Properly ignore empty GPX latitude/longitude which would otherwise
  create bogus location coordinates.

- Fixed saving of position in the collections module's history and
  recent collections module. This fix ensures that activating a
  history entry correctly restores the thumbnail offset to the point
  at which the entry was saved.

- Fixed to avoid possible unwanted flipping of the selected image when
  entering the print view due to filmstrip thumbnails being updated.

- Fixed an issue in the crop module, where Shift+dragging on an edge
  only allowed the crop area to be made larger (while maintaining the
  aspect ratio) and could not make it smaller. Previously this feature
  only worked correctly when dragging a corner.

## Lua

### API Version

- API version is now 9.1.0

### Add action support for Lua

- The lua call to `dt.gui.action` is now more flexible, with most
  parameters now being optional. For example you can read the focused
  status of a module with just `dt.gui.action("iop/filmicrgb",
  "focus")`.

- Tooltips now show the compact lua commands in mapping mode (only
  adding the last parameter -- instance -- if the module supports
  multi-instance) and have been added to presets and styles menus as
  well.

- Lua commands can be copied to the clipboard, using <kbd>Ctrl+v</kbd>
  in the shortcuts dialog, both from a selected action/shortcut, or
  long right click when in mapping mode (over a widget) or in the
  presets/styles menus.

- The shown/copied lua command for a slider or combobox will set the
  value it currently has.

- A shortcut can now be directed to a lua script that mimics a
  standard slider, dropdown or button, but dynamically selects the
  real widget(s) that receive it based on, for example, which module
  is focused or enabled. The advantage is that all fallbacks work as
  normal, so you can assign a midi knob to it and turning it (holding
  shift/ctrl to speed up/down) or pressing it to reset works
  regardless of which widget receives it.

  Basically this is a much more flexible alternative to the fake
  widgets under `processing modules/<focused>`.  This allows owners
  of, for example, an x-touch mini to use their scarce rotors in
  different, fully configurable, ways while working in different
  modules (which can also be focused using midi buttons which will
  then light up).

  Such configurations could be shared using
  https://github.com/darktable-org/lua-scripts.  examples/x-touch.lua
  is a working demonstration that can be easily amended.

- Added support for shortcuts to sliders/combos created in lua, either
  via visual mapping mode or in the shortcuts dialog under the lua
  category. Elements and effects are not supported.

### Other Lua changes

- Added an `aspect_ratio` field to `dt_lua_image_t` for image
  orientation retrieval support.

- `dt_lua_image_t` now repects the "show time in milliseconds" setting
  in lighttable preferences and will return `exif_datetime_taken` with
  milliseconds when enabled.

- Added `final_height`, `final_width`, `p_height`, and `p_width`
  fields to `dt_lua_image_t`.

- Two new properties have been added to get the flags (category,
  private) and the synonyms from a tag.

- Moved the `pixelpipe-processing-complete` event from the end of the
  image pixelpipe to the end of the preview pixelpipe to catch
  completion of events that only update the preview, such as spot
  exposure measurement in the exposure module.

## Notes

- When exporting to AVIF, EXR, JPEG XL, or XCF, selecting specific
  metadata (e.g. geo tag or creator) is not currently possible. For
  AVIF, EXR, JPEG XL, and XCF formats, darktable will not include any
  metadata fields unless the user selects all of the checkboxes in the
  export preference options.

- In order to support the correct display of numbers in darktable, the
  minimum supported Gtk version has had to be increased to
  3.24.15. For people who need to build darktable with an older
  version, this can be achieved by removing line 241 of the
  `darktable.css` file on your system. See
  https://github.com/darktable-org/darktable/issues/13166.

- Starting with this release a new support policy regarding macOS
  versions will be in place -- darktable releases will now only
  support those macOS versions that are also supported by Apple.
  Release 4.4 therefore drops support for macOS versions older than
  11.3.

## Changed Dependencies

### Mandatory

- ???

### Optional

- ???

## RawSpeed changes


## Camera support, compared to 4.2

### Base Support


### Missing Compression Mode Support

- Apple ProRAW DNGs
- CinemaDNG lossless (Blackmagic, DJI, etc.)
- Fujifilm lossy RAFs
- Nikon high efficiency NEFs
- Samsung Expert RAW DNGs

### New White Balance Presets

- Canon EOS R7
- Canon EOS R10

### Noise Profiles

   No data for this release.

### Suspended Support

Support for the following cameras is suspended because no samples
are available on raw.pixls.us:

- Creo/Leaf Aptus 22(LF3779)/Hasselblad H1
- Fujifilm FinePix S9600fd
- Fujifilm IS-1
- GoPro FUSION
- Kodak EasyShare Z980
- Leaf Aptus-II 5(LI300059)/Mamiya 645 AFD
- Leaf Credo 60
- Leaf Credo 80
- Minolta DiMAGE 5
- Olympus SP320
- Panasonic DMC-FX150
- Pentax Q10
- Phase One IQ250
- Samsung GX10
- Samsung GX20
- Samsung EK-GN120
- Samsung SM-G920F
- Samsung SM-G935F
- Sinar Hy6/ Sinarback eXact
- ST Micro STV680

## Translations

- New English translation with capital letters
- ???
