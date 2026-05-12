# discord-gfm-tables

BetterDiscord plugin that renders GitHub-Flavored Markdown pipe tables in Discord messages.

Discord already handles a useful subset of Markdown, but table syntax still shows up as raw pipe-delimited text. `discord-gfm-tables` watches rendered messages, detects valid GFM table blocks, and replaces only those blocks with native HTML tables.

## install

Copy `discord-gfm-tables.plugin.js` into your BetterDiscord plugins folder:

```text
%appdata%/BetterDiscord/plugins
```

or on Linux:

```text
~/.config/BetterDiscord/plugins
```

then enable **MarkdownTableRenderer** in BetterDiscord's plugin settings.

## example

```markdown
| name | status | notes |
| --- | :---: | ---: |
| tables | working | rendered |
| discord | missing | fixed locally |
```

renders as a real table in the message view. Wide tables keep their content and scroll horizontally instead of truncating columns.

## what it supports

| behavior | support |
| --- | --- |
| header row plus delimiter row | yes |
| left, center, and right alignment | yes |
| optional leading and trailing pipes | yes |
| escaped pipes like `\|` inside cells | yes |
| missing body cells | padded as empty cells |
| extra body cells | ignored, matching GFM |
| block content inside cells | no |

## why this shape

This is a DOM post-processor, not a Discord Markdown parser patch. Discord does not expose a stable public renderer extension point, so the plugin takes the conservative path:

1. watch message content nodes
2. read the visible message text
3. parse only valid GFM-style table blocks
4. leave non-table messages untouched

The table rules are documented in [docs/markdown-table-rendering-research.md](docs/markdown-table-rendering-research.md).

## notes

- The plugin targets rendered message IDs that currently start with `message-content-`.
- It restores original message text when the plugin is stopped.
- It has no external dependencies and does not make network requests.
- BetterDiscord and Discord DOM internals are unofficial surfaces, so client updates can require selector changes.
