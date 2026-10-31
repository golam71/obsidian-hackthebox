# How it works

Obsidian uses [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) to style the themes.
The way to make the theme is to look up the CSS variable from the [documentation](https://docs.obsidian.md/Reference/CSS+variables/CSS+variables) and then edit the default values of those variable. The file stays in `vault/.obsidian/themes/Hackthebox`. Inside which there is a [theme.css](theme.css) file that is where all the code is and the other [Mainfest.json](manifest.json) holds information about the theme.

- Foundations: Abstracted variables for colors, spacing, typography and more

- Components : All native items.

- Editor : The editors syntax highlight and other markdown specific stuff

- Plugins : Mainly the canvas, graph, search etc.

- Window : Outer border, frame, scrollbar etc.

I have skipped obsidian publish(this is when you make your notes and publish them as a website)

# Actual code explaination

so every single code block begins with a very long title

```
/* =========================================================== */
/* =========================================================== */
/* =======================FOUNDATIONS========================= */
/* =========================================================== */
/* =========================================================== */
```

here the title name changes and inside every header like this is sub topics of those like Foundations will have Borders, Colors etc.

The code always begins with a comment saying what subtopic it belongs to then it opens a body tag and modifies the default value.

```css
/* border  */
body {
  --border-width: 2px;
}

/* skipping most of colors because its not needed */
body {
  --accent-h: 80;
  --accent-s: 100%;
  --accent-l: 47%;
  --accent-a: 1;
  /* this thing resolved to the htb lime color  */
  /* --caret-color: var(--htb-lime); */
  --text-on-accent: var(--background-primary);
}
```

now notice here i begin with FOUNDATION then i say border and then the styles for border and the next item was colors which is why another comment another body tag.

I also try to be as vivid as possible. In some places we need to implement an ugly fix when the obsidian css variable simply does not have a class name for it. I will document each ugly fix so we can work on it and make it better. The code is pretty messy but will be polished along the way.

I lost the ugly fixes docs file :( (git commit fail)

first we begin with the initial variables that we will use all over the file.

# Guidelines

There are some plans for future and they are the following:

- Use only the CSS variable.

- Avoid using `!important`.

- Use less specific selectors.

Since Obsidian used `--code-comment` to color some elements, a new variable `--htb-border` was made to prevent these elements from inheriting the new color we set for `--code-comment`.

```css
.theme-dark {
  color: #a4b1cd;

  /* color of text normal */
  --htb-text-color: #a4b1cd;

  /* theme bg and secondery bg  */
  --background-primary: #141d2b;
  --background-secondary: #1a2331;
  /* main color to grab attention  */
  --htb-lime: #9fef00;

  /* heading */
  --htb-heading: #cad2e2;
  /* drivider */
  --htb-divider-color: #1a2332;
  /* border */
  --htb-border: #4a5466;
}

/* same color copy paste from dark because htb has no light theme  */
.theme-light {
  color: #a4b1cd;

  --htb-text-color: #a4b1cd;
  /* theme bg and secondery bg  */
  --background-primary: #1a2331;
  --background-secondary: #141d2b;
  --htb-lime: #9fef00;
  --htb-heading: #cad2e2;
  --htb-divider-color: #1a2332;
  --htb-border: #4a5466;
}
```

here all the color are taken directly from academy. for light and dark the secondery and primary background are swapped.

# Components

Dropdowns have some readable issues. Here i remove the border, shadow . Add radius and color.

```css
.dropdown {
  border-color: transparent !important;
  box-shadow: none !important;
  border-radius: 4px;
  color: var(--htb-heading);
  /* color: white; */
}
```

# Editor

There are callouts. Because we follow a simple roundness we add it since there is no class for it.

```css
.callout-content {
  border-radius: 7px;
}
```

code color theme is copied from academy and where i could not find academy colors i took from [silofy/hackthebox](https://github.com/silofy/hackthebox)
Two additional colors were added: `--code-attribute` and `--code-func-name`.

```css
body {
  /* htb vscode theme  */
  --code-background: var(--background-secondary);
  /* now look we can't give it the same bg as vscode has because then it wont stand out  */
  /* so i am using seconder one  */
  /* if you have better ideas do tell me  */
  --code-normal: #a4b1cd;
  --code-comment: #c5d1eb;
  --code-function: #ffcc5c;
  --code-important: #ffaf00;
  --code-keyword: #cf8dfb;
  --code-operator: #ff8484;
  --code-property: #a4b1cd;
  --code-punctuation: #a4b1cd;
  --code-string: #c5f467;
  --code-tag: #ffaf00;
  --code-value: #5cb2ff;
  --code-attribute: #ff3e3e;
  --code-func-name: #2e6cff; /* for lack of a better name */
  --code-white-space: pre;
  --code-size: var(--code-size);
}
```

The syntax highlighting for Reading Mode was copied from Academy's prism.css. We then tried to match Editing Mode's syntax highlighting to prism.css as closely as possible. They can't be exactly the same since Obsidian uses different renderers for Editing and Reading Mode.

```css
.token.comment,
.token.block-comment,
.token.prolog,
.token.doctype,
.token.cdata {
  color: var(--code-comment);
}

.token.punctuation {
  color: var(--code-punctuation);
}

.token.tag,
.token.attr-name,
.token.namespace,
.token.deleted {
  color: var(--code-attribute);
}

.token.function-name {
  color: var(--code-func-name);
}

.token.boolean,
.token.number,
.token.function {
  color: var(--code-important);
}

.token.property,
.token.class-name,
.token.constant,
.token.symbol {
  color: var(--code-function);
}

.token.selector,
.token.important,
.token.atrule,
.token.keyword,
.token.builtin {
  color: var(--code-operator);
}

.token.string,
.token.char,
.token.attr-value,
.token.regex,
.token.variable {
  color: var(--code-string);
}

.token.operator,
.token.entity,
.token.url {
  color: var(--code-value);
}

.token.important,
.token.bold {
  font-weight: 700;
}

.token.inserted {
  color: var(--htb-lime);
}

.cm-comment {
  color: var(--code-comment);
  /* font-style: italic; */
}

.cm-punctuation,
.cm-bracket,
.cm-hr {
  color: var(--code-punctuation) !important;
}

.cm-link {
  color: var(--code-property);
}

.cm-qualifier,
.cm-string,
.cm-variable-2,
.cm-def {
  color: var(--code-string);
}

.cm-tag,
.cm-attribute {
  color: var(--code-attribute);
}

.cm-number,
.cm-atom {
  color: var(--code-important);
}

.cm-keyword,
.cm-builtin,
.cm-operator {
  color: var(--code-operator);
}
.cm-property,
.cm-type {
  color: var(--code-function);
}

.cm-inline-code,
.cm-math,
.cm-variable,
.cm-variable-3,
.cm-meta {
  color: var(--code-normal);
}

.cm-operator,
.cm-string-2 {
  color: var(--code-value);
}
```

Code blocks closely resemble Academy's code blocks.

- The font size for text in all code blocks is 87.5% of the global font size.
- The color of the inline code block text is HTB green.
- Inline code blocks have no padding or background color.

```css
/* inline code has color lime and non inline code has normal code color for reading mode */
code {
  color: var(--htb-lime) !important;
  background-color: transparent !important;
  font-size: calc(var(--font-text-size) * 0.875) !important;
  padding: 0 0 !important;
}

code[data-line="0"] {
  color: var(--code-normal) !important;
  font-size: calc(var(--font-text-size) * 0.875) !important;
}

/* the inline `code` goes away and a span tag replaces that in editing mode */
span.cm-inline-code {
  color: var(--htb-lime) !important;
  background-color: transparent !important;
  font-size: calc(var(--font-text-size) * 0.875) !important;
  padding: 0 0 !important;
}

.HyperMD-codeblock {
  font-size: calc(var(--font-text-size) * 0.875) !important;
}
```

This is for lists and so that bullet points look same as number points as in unodered list and ordered list. Now the following only mkes bullet points to be a little more indented.

```css
.cm-formatting-list-ul {
  margin: 2px;
}

.cm-formatting.cm-formatting-list.cm-formatting-list-ul.cm-list-2 {
  margin-left: 10px;
}
```

The icons in metadata looks lime.

```css
.metadata-property-icon {
  color: var(--htb-lime);
  /* icons on metadata*/
}
```

This is add property to be lime.

```css
.metadata-add-button.text-icon-button {
  color: var(--htb-lime);
  /* this is add property button  */
}
```

This is the table and its rounded.

```css
/* Fix the borders and add a radius variable */
:root table {
  --table-border-radius: 8px;
  border-collapse: separate;
  border-spacing: 0;
}

/* Add the radius */
th:first-child {
  border-top-left-radius: var(--table-border-radius);
}

th:last-child {
  border-top-right-radius: var(--table-border-radius);
}

tr:last-child td:first-child {
  border-bottom-left-radius: var(--table-border-radius);
}

tr:last-child td:last-child {
  border-bottom-right-radius: var(--table-border-radius);
}

/* Redo the borders */
:root :is(td, th) {
  border-width: 0 var(--table-border-width) var(--table-border-width) 0;
}
```

# Foundation

when selecting something from teh search the color flashes as in highlights on the text in editor when that happens bg changes so we make the text black (on lime) for better readability.

```css
.is-flashing {
  color: black;
}
```

Highlighted or slected text font color meaning inside the highlighted part

```css
::selection {
  color: var(--text-on-accent);
}
```

coming to results when the search term macthes the text inside the highlight ,it needs to be black (on lime) to be readable.

```css
span.search-result-file-matched-text {
  --text-normal: black;
}
```

then when you hover upon the results the whole bg becomes lime so in this case the whole text inside the result should be black (on lime).

```css
.search-result-file-match:hover {
  --text-normal: black;
}
```

the up and down svg icon on hover needs to be black as well

```css
div.search-result-hover-button > svg {
  fill: black;
}
```

now with the icons the icon selected on the top left navbar is lime

```css
.mod-left-split .mod-top-left-space .workspace-tab-header.tappable.is-active {
  background-color: var(--htb-lime);
}
```

inside the selected icon the svg needs to be black to be visible

```css
.mod-top-left-space .workspace-tab-header.tappable.is-active svg {
  font-weight: 700;
  stroke: black;
  stroke-width: 2;
  /* color:black; */
}
```

# Extra fixes that dont fall into catagories

There are some externel fixes which removes border in some places because I go for a borderless shadow specific designa .

```css
body {
  --color-base-25: var(--background-secondary) !important;
  --color-base-40: transparent !important;

  --color-base-35: transparent !important;
  --color-base-50: var(--htb-heading) !important;
}

.vertical-tab-header-group-title {
  color: var(--htb-lime);
}
```

# Making a new release

To push the theme to a new version you must push it with a tag and this repo has a workflow build that automatically makes a new release. Release versions follow [npm semantic versioning guide.](https://docs.npmjs.com/about-semantic-versioning)

The tag must match the `version` in `manifest.json` exactly, **without a `v` prefix** (for example `2.0.9`, not `v2.0.9`). Obsidian's community directory matches the release tag to the manifest version, so a `v`-prefixed tag will not be recognised.

So in simple terms here is what you need to do:

- Make changes to the `theme.css` file
- Adjust the versioning number in `manifest.json`
- After push the code with the tag

```bash
vim manifest.json # increment the version MAJOR.MINOR.PATCH
git add .
git commit -m ":zap: Commit message" # follow gitmoji convention
git push
git tag -a 2.0.9 -m "2.0.9" # must exactly match manifest.json version, no "v" prefix
git push origin 2.0.9 # same as above
```

The workflow publishes the release with `manifest.json` and `theme.css` attached as assets. It also fails if the pushed tag does not match the manifest version.

# TODO

- Follow the guidelines

- Document the code even better.

- Listen to community feedback from discord.

Also if you have come this far please help me make this better. Join the [hacthebox discord](https://discord.com/invite/hackthebox) server we can discuss there.
