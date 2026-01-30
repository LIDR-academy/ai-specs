## Markdown Formatting Guidelines

### Headings

- **Always surround headings with blank lines** (one line above and below)
- Use ATX-style headings (`#`, `##`, `###`) instead of setext-style
- Don't skip heading levels (don't go from `#` directly to `###`)

### Code Blocks

- **Always specify language** for fenced code blocks (```bash, ```go, ```yaml, etc.)
- Use `text` for generic content that doesn't have a specific language
- **Surround fenced code blocks with blank lines**

### Lists

- **Surround lists with blank lines** (one line above and below the entire list)
- Use consistent list markers (`-` for unordered, `1.` for ordered)
- Add blank lines between list items only when items contain multiple paragraphs

### Links

- Use angle brackets for bare URLs: `<http://example.com>` instead of `http://example.com`
- Prefer reference-style links for readability when the same URL is used multiple times

### Emphasis and Strong Text

- Use `**bold**` for strong emphasis, `*italic*` for emphasis
- Don't use emphasis as a substitute for proper headings

### Line Length

- Keep lines under 120 characters when possible
- Break long lines at natural points (after punctuation, before conjunctions)

### Example Structure

```markdown
## Main Heading

Brief introduction paragraph.

### Subheading

- List item 1
- List item 2
- List item 3

Code example:

```go
func example() {
    // code here
}
```

Another paragraph with a [link](https://example.com).
```

### Common Violations to Avoid

- Missing blank lines around headings (MD022)
- Missing blank lines around lists (MD032)  
- Missing blank lines around code blocks (MD031)
- Bare URLs without angle brackets (MD034)
- Missing language specification in code blocks (MD040)
- Using emphasis instead of proper headings (MD036)
