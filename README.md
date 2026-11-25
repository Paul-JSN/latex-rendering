# latex-rendering

A Claude skill that keeps LaTeX math from breaking in chat.

Most chat interfaces run a Markdown parser before the math renderer, so valid LaTeX can still show up as raw code. This skill teaches Claude to avoid the patterns that trigger that failure:

- `$$` blocks inside blockquotes, lists, or tables
- the `|` character inside math (array column specs, absolute values)
- multi-line content stacked over arrows (`\xrightarrow{\substack{...}}`)
- very long formulas packed into one block
- inline `$...$` in interfaces that do not render it

It also tells Claude how to respond when you say "it's still not rendering": find the rule that was broken and fix just that, rather than resending the same output.

## Install

Download `latex-rendering.skill` from this repo and open it in Claude (Save skill), or copy the `latex-rendering/` folder into your skills directory.

## Contributing

Found a pattern that breaks rendering in your interface? Open an issue with the LaTeX that failed and what the chat showed.

## License

MIT. See [LICENSE](LICENSE).
