# talk-template

Reusable reveal.js talk template for Pixu / Dev Dojo presentations.

Live deck: [Talk Template](https://pixu1980.github.io/talk-template/)

## What this template gives you

- A Parcel based reveal.js presentation.
- Shared Pixu talk fonts, theme, and core setup through `@pixu-talks/*` packages.
- Modular PostHTML slide includes, so each slide or topic can live in a small file.
- Standard intro, summary, topic, and outro sections.
- Speaker profile, links, QR code slides, and the TAF outro slide.
- Boilerplate topic slides for common talk patterns.

## Requirements

- Node.js compatible with the workspace packages.
- pnpm.
- The sibling `talks-libraries` workspace available at `../talks-libraries/packages/*`, as defined in `pnpm-workspace.yaml`.

Install dependencies from this folder:

```bash
pnpm install
```

If pnpm asks to verify dependencies before running scripts and dependencies are already installed, use:

```bash
pnpm --config.verify-deps-before-run=false run <script-name>
```

## Commands

```bash
pnpm run dev
```

Starts Parcel on port `6001` after clearing cache and `dist`.

```bash
pnpm run build
```

Builds the static deck into `dist/` with relative asset URLs.

```bash
pnpm run clear
```

Removes `dist/` and `.parcel-cache/`.

```bash
pnpm run format
```

Runs Prettier for HTML, CSS, Markdown, JSON, YAML, then Biome for JavaScript and TypeScript.

```bash
pnpm run lint
```

Runs Biome lint.

```bash
pnpm run rel:major
pnpm run rel:minor
pnpm run rel:patch
```

Builds, bumps the package version, commits the release tag message, then pushes commits and tags.

## Project structure

```text
src/
  index.html              Main HTML shell
  brand/brand.html        Persistent Dev Dojo brand link
  scripts/index.js        reveal.js setup through @pixu-talks/core
  styles/index.css        shared font and theme imports
  assets/                 images, logos, QR codes, audio
  slides/
    slides.html           root slide composition file
    intro/                intro stack
    summary/              agenda and dive in stack
    topics/               main content slides
    outro/                wrap up, links, TAF, thanks
```

`src/index.html` loads `src/slides/slides.html` inside Reveal's `.slides` container and includes `src/brand/brand.html` after the slide stack.

## Slide composition

Slides use PostHTML includes. Root composition lives in `src/slides/slides.html`:

```html
<include src="./slides/intro/intro.html"></include>
<include src="./slides/summary/summary.html"></include>
<include src="./slides/topics/topics.html"></include>
<include src="./slides/outro/outro.html"></include>
```

Each section file can include smaller partials:

```html
<include src="./slides/topics/my-topic/_01-section-opener.html"></include>
<include src="./slides/topics/my-topic/_02-concept.html"></include>
<include src="./slides/topics/my-topic/_03-demo.html"></include>
```

Use `_NN-name.html` for slide partials when order matters. Use `index.html` or `<topic-name>.html` as the container for a topic group.

## Existing slide sections

### Intro

Files in `src/slides/intro/`:

- `_doodle.html`: visual opener or talk mood slide.
- `_pixu.html`: speaker profile and social links.
- `_main-topic.html`: talk title and framing.
- `intro.html`: includes the intro partials.

Typical intro flow:

```html
<include src="./slides/intro/_doodle.html"></include>
<include src="./slides/intro/_pixu.html"></include>
<include src="./slides/intro/_main-topic.html"></include>
```

### Summary

Files in `src/slides/summary/`:

- `_topics.html`: agenda slide with fragments.
- `_dive-in.html`: animated transition slide.
- `summary.html`: includes the summary partials.

Use summary when the talk needs an agenda. You can comment the summary include in `src/slides/slides.html` for shorter talks.

### Topics

Main content starts at `src/slides/topics/topics.html`.

Current template includes:

```html
<include src="./slides/topics/boilerplate/boilerplate.html"></include>
```

Replace that include with real topics when building a talk, or keep the boilerplate section as an internal starter while drafting.

### Outro

Files in `src/slides/outro/`:

- `_wrap-up.html`: pros, cons, conclusions, or recap.
- `_links.html`: QR code and personal/community links.
- `_taf.html`: TAF animated closing slide with audio.
- `_thanks.html`: final thanks slide.
- `outro.html`: includes the outro partials.

## Boilerplate slides

Reusable partials live in `src/slides/topics/boilerplate/`.

They model patterns found across the talk repositories:

- `src/slides/slides.html` composes intro, summary or agenda, topics, and outro with PostHTML includes.
- `intro/intro.html` usually includes `_doodle.html`, `_pixu.html`, and `_main-topic.html`, sometimes with an extra event specific cover slide.
- `summary/summary.html` or `agenda/agenda.html` usually includes `_topics.html` and `_dive-in.html`.
- `topics/topics.html` either includes flat numbered partials or section containers with their own `index.html` file.
- Technical topics repeat the same flow: section opener, concept explanation, code walkthrough, code plus output, live demo, support note, and takeaways.
- Code slides use `<pre><code class="hljs language-*" data-trim data-line-numbers>`, often with line ranges for progressive reveal.
- Demo slides use `data-topic`, `data-menu-title`, `balance`, `fragment`, `<comparison>`, `<left>`, `<right>`, and `aria-live` when state changes.
- Outro sections usually include `_wrap-up.html`, `_links.html`, `_taf.html`, and `_thanks.html`.

Boilerplate files:

- `_01-section-opener.html`: title plus one sentence frame.
- `_02-concept.html`: problem, shift, payoff.
- `_03-code-walkthrough.html`: highlighted code with line ranges.
- `_04-code-and-output.html`: code beside rendered result.
- `_05-live-demo.html`: control plus live output with `aria-live`.
- `_06-support-note.html`: support, fallback, adoption decision.
- `_07-takeaways.html`: section summary and bridge.
- `boilerplate.html`: includes all boilerplate partials.

To start a new topic from the boilerplate:

1. Copy `src/slides/topics/boilerplate/` to `src/slides/topics/<topic-name>/`.
2. Rename `boilerplate.html` to `index.html` or `<topic-name>.html`.
3. Replace `data-topic="boilerplate"` with the topic slug.
4. Replace `data-menu-title` values with slide specific labels.
5. Update headings, body copy, code samples, notes, and demos.
6. Include the topic from `src/slides/topics/topics.html`.

Example:

```html
<include src="./slides/topics/css-layers/index.html"></include>
```

## Adding a single slide

Create a partial in the right section folder:

```text
src/slides/topics/_01-my-slide.html
```

Add it to `src/slides/topics/topics.html`:

```html
<include src="./slides/topics/_01-my-slide.html"></include>
```

Use this structure for a simple slide:

```html
<section data-topic="my-topic" data-menu-title="Short menu label">
  <h4>Slide <mark>title</mark></h4>
  <small balance>One focused explanation.</small>
</section>
```

## Adding a vertical topic stack

Use a wrapper `<section>` when a topic has nested slides:

```html
<section>
  <section data-topic="my-topic" data-menu-title="Intro">
    <h3><mark>My</mark> topic</h3>
  </section>
  <section data-topic="my-topic" data-menu-title="Example">
    <h4>Example</h4>
  </section>
</section>
```

Or split the stack into partials:

```html
<section>
  <include src="./slides/topics/my-topic/_01-intro.html"></include>
  <include src="./slides/topics/my-topic/_02-example.html"></include>
  <include src="./slides/topics/my-topic/_03-takeaways.html"></include>
</section>
```

## Code slides

Use Highlight.js classes and Reveal line number attributes:

```html
<pre>
  <code class="hljs language-css" data-trim data-line-numbers="1-3|5-8">
.card {
  display: grid;
  gap: 1rem;
}

.card[data-state="active"] {
  outline: 2px solid currentColor;
}
  </code>
</pre>
```

Guidelines:

- Keep examples short enough to read from the back of the room.
- Use line ranges to reveal one idea at a time.
- Prefer real syntax over pseudo code when teaching an API.
- Escape HTML examples inside `<code>` with `&lt;` and `&gt;`.

## Demo slides

Use native controls, accessible labels, and attribute driven state where possible:

```html
<section data-topic="demo" data-menu-title="Live demo">
  <h4><mark>Live</mark> demo</h4>
  <comparison>
    <left>
      <button type="button" aria-pressed="false" data-demo-toggle>Toggle state</button>
    </left>
    <right>
      <output aria-live="polite" data-demo-output>data-state="idle"</output>
    </right>
  </comparison>
</section>
```

Demo rules:

- Keep controls close to the visual result.
- Use `button`, `input`, `select`, `output`, and other native elements first.
- Add `aria-live` when text changes after user interaction.
- Keep JavaScript minimal and limited to setting attributes or syncing demo state.
- Document any demo specific state contract in the slide notes or README.

## Speaker notes

Use Reveal notes with `<aside class="notes">`:

```html
<aside class="notes">Explain why this example matters, what to point at, and what question to ask the room.</aside>
```

Notes are not visible in the main deck but are useful during presenter mode.

## Common slide helpers

The shared Pixu talk theme supports custom elements and attributes used across talks:

- `<comparison>`: two column layout.
- `<left>` and `<right>`: named columns inside `<comparison>`.
- `<example code>`: framed code example block.
- `fragment`: Reveal incremental reveal class.
- `balance`: text balancing helper.
- `speaker`: speaker slide styling.
- `links`: link/QR focused layout.
- `sr-only`: screen reader only text.
- `data-menu-title`: shorter label for Reveal menu/navigation.
- `data-topic`: groups slides by topic in markup and styling.

## Assets

Assets live under `src/assets/`.

Common folders:

- `src/assets/logo/`: social and platform icons.
- `src/assets/qrcodes/`: QR code images for slides and links.
- `src/assets/audio/`: audio used by outro slides.
- `src/assets/`: speaker photo, brand imagery, and talk specific media.

Use relative paths from compiled HTML:

```html
<img src="./assets/qrcodes/slides.png" alt="Slides QR code" />
```

For images with WebP conversion, use Parcel query syntax:

```html
<picture>
  <source srcset="./assets/pixu.jpg?as=webp" type="image/webp" />
  <img src="./assets/pixu.jpg" alt="Pixu" />
</picture>
```

Asset rules:

- Always include useful `alt` text for meaningful images.
- Use empty `alt=""` only for decorative images.
- Keep QR code captions visible so users know what they open.
- Optimize large images before committing.

## Styles

Main stylesheet: `src/styles/index.css`.

Default imports:

```css
@import 'npm:@pixu-talks/fonts';
@import 'npm:@pixu-talks/theme';
```

Add talk specific styles below the imports or split into imported files if the deck grows.

Style rules:

- Prefer theme helpers and existing slide patterns before custom layout.
- Keep per slide styles scoped by `data-topic`, class, or custom attribute.
- Respect reduced motion for heavy animations.
- Avoid inline styles except for tiny one off demos.

## Scripts

Main script: `src/scripts/index.js`.

Default content:

```js
import '@pixu-talks/core';
```

Add demo JavaScript only when CSS or native HTML cannot express the behavior.

Script rules:

- Use vanilla JavaScript modules.
- Prefer event delegation for slide demos.
- Mutate `data-*` attributes for visual state.
- Keep demo scripts small and isolated.

## Updating speaker and links

Speaker profile lives in `src/slides/intro/_pixu.html`.

Outro links live in `src/slides/outro/_links.html`.

Update these when cloning the template:

- Speaker name and bio.
- Speaker image in `src/assets/pixu.jpg`.
- Social links and icons.
- QR code images in `src/assets/qrcodes/`.
- Slide deck URL and repository URL if enabled.

## Build output and deployment

`pnpm run build` writes static output to `dist/`.

GitHub Pages workflow lives in `.github/workflows/static.yml` and deploys `dist/` when a version tag matching `v*.*.*` is pushed.

Release scripts create version tags:

```bash
pnpm run rel:patch
```

Before release:

1. Run `pnpm run format`.
2. Run `pnpm run lint`.
3. Run `pnpm run build`.
4. Check `dist/` output in browser if visual changes are significant.
5. Run appropriate release command.

## Clone checklist for a new talk

1. Rename package in `package.json`.
2. Update `<title>` in `src/index.html`.
3. Replace intro content in `src/slides/intro/`.
4. Replace agenda in `src/slides/summary/_topics.html` or remove summary include.
5. Replace boilerplate topic include with real topic includes.
6. Update outro wrap up, QR codes, and links.
7. Add talk specific assets under `src/assets/`.
8. Add talk specific styles in `src/styles/index.css`.
9. Add small demo scripts in `src/scripts/index.js` only when needed.
10. Run format, lint, build.

## Quality checklist

- One main idea per slide.
- Every technical topic has explanation, code, and demo or concrete output.
- Code samples are readable at presentation size.
- Interactive demos work with keyboard.
- Dynamic text uses `aria-live` when needed.
- Images have useful alt text.
- Speaker notes explain timing, context, and gotchas.
- Experimental APIs mention support and fallback.
- Build passes before release.
