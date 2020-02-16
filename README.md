# Developer Handbook

A handbook for software development and deployment practices, naming conventions, design patterns, and project recipes.

This project uses [Hugo](https://gohugo.io/), an open-source static site generator.

## Discussing with the team

Present to the team to get a mutually agreed upon set of practices and conventions.
Update pages with feedback from the team and remove the ones that do not apply.
Add new pages as you see fit. 

## Launching the handbook

1. Install `hugo`
    ```
    brew install hugo
    ```

1. Run locally (default: `http://localhost:1313`)
   ```
   hugo -D server
   ```

## Adding new content

For new sections:
```
hugo new <section>/_index.md
```

Creates an index page for that section. You can have nested sections too.
The generated _index.md has a title field which can be used to customize the section title in the sidebar.
This way you can name a section 'dev', but title it as 'Development Practices' 
without having the spaces and uppercase characters in your directory structure.

For new pages within a section:

```
hugo new <section>/_index.md
```

### Writing content

The handbook content is typically written in [Markdown](https://en.wikipedia.org/wiki/Markdown),
a readable, easy-to-learn mark-up language - that this very README file uses.

However, Markdown can be limiting, so Hugo introduced [shortcodes](https://gohugo.io/content-management/shortcodes/) 
to help overcome these limitations without resorting to HTML.
The Hugo Learn Theme used in the project also introduces additional [shortcodes](https://learn.netlify.com/en/shortcodes/).

## Moving or deleting pages

If you moved or deleted pages, the locally running server might not pick it up properly until it is restarted.

## Deployment

TODO

## Resources

- [Hugo Documentation](https://gohugo.io/getting-started/quick-start/)
- [Hugo Learn Theme Documentation](https://learn.netlify.com/en/)
- [Markdown Syntax](https://learn.netlify.com/en/cont/markdown/)
- [Hugo default shortcodes](https://gohugo.io/content-management/shortcodes/)
- [Hugo Learn Theme shortcodes](https://learn.netlify.com/en/shortcodes/)