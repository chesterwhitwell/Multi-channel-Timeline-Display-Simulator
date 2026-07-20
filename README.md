# Multi-channel Timeline Display Simulator

A browser-based simulator for building, previewing and presenting up to four independent meter signals across a shared timeline.

The tool combines configurable seven-segment displays, keyframe-based signal timelines, independent graph axes, signal noise, trip markers and direct graph editing – including custom cubic Bézier transition curves.

It is delivered as a single self-contained HTML file and requires no build process, server, framework or external dependency.

![Multi-channel Timeline Display Simulator with four active channels and editable Bézier transitions](docs/timeline-simulator.png)

## Features

- Select between **one and four active channels**.
- Configure an independent timeline for each channel.
- Retain hidden channel data when reducing the active channel count.
- Set channel labels, units, modes, decimal places, display digits and rounding steps.
- Add, remove and edit keyframes with precise time and value controls.
- Choose transition presets or create custom cubic Bézier curves.
- Allow vertical curve overshoot while keeping time progression forwards.
- Add constant, settling or occasional spike noise.
- Add trip markers to significant keyframes.
- Scrub or play all channels along one shared timeline.
- Play once or loop, with adjustable playback speed and update rate.
- Give every channel an independent vertical graph scale.
- Use automatic graph ranges or enter manual minimum and maximum values.
- Drag graph keyframes to change their time and value.
- Select graph segments and drag Bézier handles to reshape transitions.
- Use a focused edit-channel mode to make overlapping traces easier to select.
- Collapse individual channel timelines to reduce interface clutter.
- Switch to presentation mode to show only the readouts and graph.
- Automatically save settings in the browser.
- Import and export complete configurations as JSON.
- Run offline – no data is sent to a server.

## Quick start

1. Download the HTML file.
2. Open it in a modern web browser.
3. Select the number of channels required.
4. Edit the example timelines or choose **Restore example** to return to the default configuration.

No installation or local server is required.

For a GitHub repository, rename the application file to `index.html`:

```text
/
├── docs/
│   └── timeline-simulator.png
├── index.html
└── README.md
```

## Using the simulator

### 1. Choose the active channels

Use the **Channels** selector in the header to show one, two, three or four channels.

Reducing the channel count hides the additional channels without deleting their timelines or settings. Increasing the count restores them.

### 2. Preview the timeline

The **Preview timeline** panel provides:

- a shared timeline scrubber
- play and pause control
- stop control
- playback progress
- a live summary of the current channel values

The longest active channel determines the shared timeline duration.

### 3. Edit keyframes precisely

Each channel has its own collapsible timeline table.

A keyframe defines:

- time
- channel value
- transition to the next keyframe
- optional noise type and amount
- optional trip marker

Transition and noise settings belong to the segment that begins at that keyframe. They therefore do not apply to the final keyframe.

The first keyframe always begins at `0 s`.

### 4. Edit directly on the graph

Select **Edit graph**, then choose the channel to edit.

The active channel is brought visually to the front and other channels are subdued. This makes overlapping traces easier to distinguish and select.

While graph editing is active:

- drag a keyframe vertically to change its value
- drag a keyframe horizontally to change its time
- click a line segment to select its transition
- drag a numbered Bézier handle to reshape the selected transition

Keyframes cannot be dragged past neighbouring keyframes.

The graph uses enlarged invisible target areas around points, lines and Bézier handles, while keeping the visible controls compact.

### 5. Shape custom Bézier transitions

Continuous transitions can be edited as cubic Bézier curves.

Horizontal handle movement controls when acceleration and deceleration occur. The handles are constrained so time always progresses forwards.

Vertical movement can extend beyond the start and end values, allowing effects such as:

- overshoot
- undershoot
- delayed movement
- rapid rise or fall
- settling behaviour

Moving either handle changes the segment transition to **Custom curve**.

Preset transitions remain available in the timeline table, including linear, ease-in, ease-out, ease-in/out, overshoot, stepped and hold behaviours.

Stepped and held segments must be changed to a continuous transition before Bézier handles can be edited.

### 6. Set graph ranges

Each channel has its own vertical graph scale.

Open **Axis ranges** to enter a channel-specific minimum and maximum. Leave both fields blank to use automatic scaling.

Manual axis settings affect only the graph presentation – they do not change timeline values or seven-segment readings.

When graph editing is enabled, the visible range temporarily expands to provide additional room for keyframes and Bézier handles, including overshoot controls.

### 7. Configure the displays and playback

Open **Display and playback settings** to change:

- channel label
- unit
- mode text
- decimal places
- number of display digits
- display rounding step
- update rate
- noise settling
- spike probability
- playback time scale
- play-once or looping behaviour
- LCD contrast

## Graph behaviour

Each channel has an independent vertical axis. A trace must therefore be read against its own channel scale rather than the scale belonging to another trace.

The static graph is based on the defined timeline. During playback, sampled readings – including configured noise – can be recorded as a trace.

Use **Clear trace** to remove recorded playback samples and return to the static timeline view.

Trip markers are annotations. A trip marker does not automatically change a value – define a following keyframe when the signal should drop or move to another level.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| `Space` | Play, pause or resume the timeline |
| `P` | Enter or leave presentation mode |
| `Escape` | Leave presentation mode |

Keyboard shortcuts are ignored while typing in an input, select menu or editable field.

## Saving, importing and exporting

Settings are saved automatically using browser `localStorage`.

The saved configuration includes:

- all four channel timelines
- active channel count
- channel display settings
- transition and Bézier data
- noise and trip settings
- graph ranges
- playback settings
- collapsed timeline states

Use **Export** to download the configuration as JSON and **Import** to restore a previously exported file.

Use **Forget saved settings** to remove the saved browser copy without immediately changing the current view.

Browser storage is specific to the browser, device and page origin. Export important configurations before clearing browser data or moving the tool to a different web address.

## Presentation mode

Presentation mode removes editing controls and focuses on the active readouts and graph.

This is useful for:

- demonstrations
- screen recording
- facilitated learning activities
- embedding the simulator in a larger learning experience

## GitHub Pages deployment

The application can be hosted directly with GitHub Pages.

1. Add the simulator to a repository as `index.html`.
2. Add this `README.md` file.
3. Push the repository to GitHub.
4. Open **Settings → Pages**.
5. Under **Build and deployment**, choose **Deploy from a branch**.
6. Select the required branch and the repository root.
7. Save the settings and wait for GitHub to publish the site.

Because the tool has no external dependencies, no build action is required.

## Embedding

Once hosted, the simulator can be embedded in another page using an iframe:

```html
<iframe
  src="https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/"
  title="Multi-channel Timeline Display Simulator"
  width="100%"
  height="900"
  style="border: 0;"
  allowfullscreen>
</iframe>
```

Adjust the height to suit the host page and the number of controls that need to remain visible.

When embedding into a learning platform, confirm that the platform allows iframe content and browser storage for the hosted domain.

## Technical overview

The simulator uses:

- semantic HTML
- responsive CSS
- vanilla JavaScript
- the Canvas 2D API for graph rendering and hit-testing
- Pointer Events for graph interaction
- `localStorage` for automatic browser persistence
- the File API for JSON import and export

Everything is contained in one HTML file.

## Development

There is no compilation step. Edit the HTML file directly and refresh the browser.

A simple local server can be useful during development, although it is not required:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

When modifying the graph, take care to preserve the distinction between:

- timeline values
- rounded values shown by the seven-segment displays
- graph-axis ranges
- sampled playback values
- Bézier control-point values

Axis ranges should affect only visual mapping. They should never alter the underlying signal values.

## Privacy

The simulator does not make network requests and does not transmit timeline data.

Configurations remain in the browser unless the user explicitly exports a JSON file.

## Accessibility and responsive behaviour

The interface includes labelled controls, keyboard focus states, status announcements and reduced-motion support.

Timeline tables convert into labelled keyframe cards on narrower screens. Graph editing is supported through Pointer Events, but detailed curve shaping is generally easiest with a mouse, trackpad or stylus on a larger screen.
