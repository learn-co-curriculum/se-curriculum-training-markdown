# Working with Markdown

## Learning Goals

- Write well-formed lessons using Markdown
- Identify style issues in Markdown using markdownlint
- Fix style issues with Prettier

## Introduction

We author all of our lessons in Markdown. It's easy to learn, and (crucially for
us) it's quite portable and can be adapted to a number of different contexts
— in our case, it can be rendered in both GitHub and Canvas (with a little
processing).

If you're not familiar with Markdown, take some time with these resources:

- [Mastering Markdown](https://masteringmarkdown.com/)
- [Markdown Guide](https://www.markdownguide.org/getting-started/)

In the rest of this lesson, we'll see how to write well-formed Markdown that can
be displayed in GitHub and Canvas, and set up some tools to help ensure
stylistic consistency when working with Markdown.

## Markdown for Canvas

You've already learned about the github-to-canvas gem and have seen that it is
able to create lessons in Canvas from Markdown files. Under the hood, this
conversion is done via two Ruby gems:

- [`redcarpet`](https://github.com/vmg/redcarpet): converts Markdown to HTML
- [`rouge`](https://github.com/rouge-ruby/rouge): allows syntax highlighting for
  fenced code blocks

The [github-to-canvas source code][conversion code] shows the configuration we
use with these two gems. But the relevant info is that we can convert the
following Markdown syntax to HTML:

- All [basic syntax][basic syntax]
- [Tables][tables]
- Fenced code blocks with [syntax highlighting][syntax highlighting]

Feel free to take advantage of any of these features when you are authoring
lessons!

Fenced code blocks with syntax highlighting are a particularly important tool
for demonstrating code snippets. Here's a quick list of common languages we use
in the curriculum:

- `js`: JavaScript
- `jsx`: JavaScript with JSX (i.e. React components)
- `json`: JSON
- `rb`: Ruby
- `html`: HTML
- `css`: CSS
- `sql`: SQL
- `console`: console commands (prefixed with a `$`)
- `sh`: Shell scripts (you may see this more in old lessons where we should
  actually be using `console`)
- `txt`: plain text without syntax highlighting

The `rouge` gem supports [many more languages][languages] as well.

## Identifying and Fixing Markdown Style Issues

In order to ensure stylistic consistency between all of our Markdown files, we
use the [`markdownlint`][markdownlint] tool.

`markdownlint` can be used in a number of different ways, such as a CLI (which
makes it handy for automation), but the easiest way to use it is via a text
editor extension.

You're welcome to use an editor of your choice, but our team has found that VS
Code has many useful extensions that help increase our productivity, such as an
extension for `markdownlint`.

Install the [`markdownlint` extension][markdownlint-extension], then open your
VS Code settings by opening the Command Palette in VSCode (`Command+Shift+P`),
and typing `Preferences: Open Settings (JSON)`. Include the following
configuration in your `settings.json` file:

```json
{
  "markdownlint.config": {
    // line-length: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md#md013
    "MD013": {
      "code_blocks": false,
      "headings": false,
      "tables": false
    },
    // commands-show-output: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md#md014
    "MD014": false,
    // no-duplicate-headings: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md#md024
    "MD024": {
      "siblings_only": true
    },
    // no-trailing-punctuation: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md#md026
    "MD026": {
      "punctuation": ".,;:。，；："
    },
    // no-inline-html: https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md#md026
    "MD033": false
  }
}
```

We also have our own set of custom markdown custom linting rules which can be
used along with `markdownlint`:

- [markdownlint-rules-flatiron-lesson][]

See the Usage and VSCode Integration notes in the link above for details on
setting it up.

It's also recommended to use the [Prettier VS Code extension][prettier] to help
automatically format Markdown files (as well as a number of other languages).
Install it, then include the following configuration in your `settings.json`
file to set it as your editor's default formatter, and (optionally) enable
format on save:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  // Auto-wrap long lines at 80 characters. https://prettier.io/docs/en/options.html#prose-wrap
  "prettier.proseWrap": "always"
}
```

The [Code Spell Checker][code spell checker] plugin is also recommended to help
catch spelling errors in both code as well as Markdown files.

With this setup, you'll be able to easily spot and fix most common stylistic
issues in Markdown files. Let's try that out now!

## Exercise: Fixing a Markdown File

We're going to show you what a poorly formatted Markdown file might look like
and how to fix it using the tools you installed in the previous section.

In this lesson's repository is a file `fix-me.md` that has an early version of a
Flatiron lesson with lots of Markdown issues. Clone down the repo for this
lesson, and open up the file `fix-me.md` in VS Code.

If you set up `markdownlint` correctly, you should see some tan-colored squiggly
lines throughout the file. Hovering over these lines with your mouse will reveal
the issues identified by `markdownlint`.

![markdownlint example](https://curriculum-content.s3.amazonaws.com/curriculum-training/markdown/markdownlint.png)

If you also have Prettier configured, and have the `editor.formatOnSave` option
set to `true`, you can fix these issues simply by saving the file! The document
will be re-formatted and any violations that `markdownlint` or Prettier is able
to fix automatically will be fixed.

If you don't have the `formatOnSave` feature enabled, you can also auto-fix some
issues by placing your cursor anywhere in a place where there's an issue,
pressing the `command+.` keys, and selecting the "Fix all supported markdownlint
violations" option. Not all issues will be resolved. For instance, lines that
are longer than 80 characters can't be fixed automatically by `markdownlint`,
but they can be by Prettier.

You'll also need to resolve some issues manually, like adding alt text to the
images.

See if you can fix all the issues identified by `markdownlint` before moving on!

[conversion code]:
  https://github.com/learn-co-curriculum/github-to-canvas/blob/master/lib/github-to-canvas/repository_converter.rb#L30-L41
[basic syntax]: https://www.markdownguide.org/basic-syntax/
[tables]: https://www.markdownguide.org/extended-syntax/#tables
[syntax highlighting]:
  https://www.markdownguide.org/extended-syntax/#syntax-highlighting
[languages]: https://github.com/rouge-ruby/rouge/blob/master/docs/Languages.md
[markdownlint]: https://github.com/DavidAnson/markdownlint
[markdownlint-extension]:
  https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint
[markdownlint-rules-flatiron-lesson]:
  https://github.com/learn-co-curriculum/markdownlint-rules-flatiron-lesson
[prettier]:
  https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
[code spell checker]:
  https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
