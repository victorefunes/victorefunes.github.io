# victorefunes.github.io

Personal academic and job-market website of **Victor Funes-Leal**, served via
GitHub Pages at **https://victorefunes.github.io**.

> **This repository holds *generated* output, not source.**
> The files here are produced by Hugo from a separate source project. Editing
> them directly is pointless — the next build overwrites everything. Change the
> source, rebuild, then push.

## Stack

- **[Hugo](https://gohugo.io)** static site generator (extended build)
- The **Academic** (`hugo-academic`) theme, via Alex Hollingsworth's
  [job-market website template](https://github.com/hollina/template-job-market-website)
- Built and previewed from R with **[blogdown](https://bookdown.org/yihui/blogdown/)**

## What's in here

This repo is the `public/` output folder of the Hugo project — the rendered
HTML/CSS/XML that GitHub Pages serves. The **source** (content, config, theme)
lives separately in the `JM-website` project; only the compiled site is
committed here.

## Build & deploy

From the **source** project (`JM-website`) in RStudio:

```r
library(blogdown)
blogdown::serve_site()   # live local preview at http://127.0.0.1:4321
blogdown::hugo_build()   # regenerate the public/ output committed to this repo
```

Then, from inside `public/` (this repo):

```bash
git add -A
git commit -m "Rebuild site"
git push
```

Pushing to `master` updates the live site automatically.

### Adding a publication

In the source project, create `content/publication/<slug>/index.md`. Key
front-matter fields:

| Field                | Purpose                                                        |
| -------------------- | -------------------------------------------------------------- |
| `publication_types`  | `["3"]` = working paper, `["2"]` = published — drives which homepage section it appears in |
| `date`               | Sort order (newest first). Use a real `YYYY-MM-DDT00:00:00Z` — a leftover template date buries the paper at the bottom |
| `url_pdf`            | Link to the PDF (`files/paper.pdf`) or an external URL         |

Then rebuild and push as above.

## Notes & gotchas

- **Hugo version is pinned.** The `hugo-academic` theme predates current Hugo
  and breaks on recent releases (removed accessors such as
  `site.GoogleAnalytics`, and the `security.allowContent` policy added in
  0.162). Keep the pin in the source project's `.Rprofile`:

  ```r
  options(blogdown.hugo.version = "0.119.0")  # or whichever version you settled on
  ```

- **The repo lives inside a Box-synced folder.** Box and git both touch the
  files in `public/`, which can produce phantom merge conflicts in generated
  output. Let Box finish syncing before running git. If a pull tangles:

  ```bash
  git merge --abort
  git fetch origin
  git reset --hard origin/master
  # then rebuild from source and push
  ```

- **This README lives in the output folder.** Hugo leaves it alone on a normal
  build; it would only be removed if you build with `--cleanDestinationDir`.
