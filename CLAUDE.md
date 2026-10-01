# Adversarial Machine Learning — Spring 2026 course website

Quarto website for CS 4727/5727 (University of Idaho), published via GitHub
Actions (`.github/workflows/publish.yml`) to GitHub Pages on every push to
`main`. It mirrors the layout and theme of the Fall 2026 Applied Data Science
with Python site (https://github.com/avakanski/Fall-2026-Applied-Data-Science-with-Python).

Unlike that site, the lectures here are **slide decks (PDF)**, not notebooks.
Each lecture page is a small `.qmd` that embeds the PDF in an `<iframe>` and
links to the PowerPoint version hosted on the instructor's website
(https://www.idahofallshighered.org/vakanski/Courses/Adversarial_Machine_Learning.html).
PPTX files are **not** stored in the repo (several are 15–40 MB).

## Layout

```
_quarto.yml                 site config + sidebar (one entry per lecture)
index.qmd                   course info + week-by-week schedule with readings
README.md                   GitHub landing page (duplicates index.qmd content)
Lectures/
  CS_4727_5727-Adversarial_Machine_Learning-Syllabus.pdf
  Lecture_1-Introduction_to_AML/
    Lecture_1-Introduction_to_AML.pdf
    Lecture_1-Introduction_to_AML.qmd
  Lecture_2-.../ ... Lecture_15-.../   (flat: no theme folders, see note below)
Codes/
  Code_Examples.qmd         landing page for notebooks (add notebooks next to it)
```

Lecture numbering on the site (1–15) follows the topic list on the
instructor's page, so it differs from the file names on that server, where
e.g. all three cybersecurity decks are "Lecture_10_…" and Privacy is
"Lecture_11_…". The mapping lives in the PPTX links inside each lecture `.qmd`.

## Updating a lecture PDF

Replace the PDF in its folder, keeping the file name (the `.qmd` next to it
references the name in both the link and the iframe). Then commit and push.

## Adding a lecture

1. Create `Lectures/Lecture_N-Short_Title/` with the PDF named
   like the folder, and copy an existing lecture `.qmd` (update title, PDF
   name, and PPTX link).
2. Register it in **three** places: the sidebar in `_quarto.yml`, the
   schedule row in `index.qmd`, and the lecture list in `README.md`.

## Adding a code notebook

1. Put the executed notebook (outputs saved — `execute: enabled: false` means
   the build never runs code) in `Codes/Code_N-Short_Title/Code_N-Short_Title.ipynb`,
   with any images in an `images/` subfolder.
2. Add a sidebar entry under the "Code Examples" section in `_quarto.yml`,
   link it from `Codes/Code_Examples.qmd`, and list it in `README.md`.
3. Optional header badges (first markdown cell), replacing `<PATH>` with the
   notebook's repo path (commas URL-encoded as `%2C`):

   ```markdown
   [![View notebook on Github](https://img.shields.io/static/v1.svg?logo=github&label=Repo&message=View%20On%20Github&color=lightgrey)](https://github.com/avakanski/Spring-2026-Adversarial-Machine-Learning/blob/main/<PATH>.ipynb)
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/avakanski/Spring-2026-Adversarial-Machine-Learning/blob/main/<PATH>.ipynb)
   [![View on Website](https://img.shields.io/static/v1.svg?logo=quarto&label=Web&message=View%20on%20Website&color=blue)](https://avakanski.github.io/Spring-2026-Adversarial-Machine-Learning/<PATH>.html)
   ```

   Notebook heading/anchor conventions (numbered `## N.1` sections, `<a id>`
   anchors on their own line above headings, no self-closing `<a/>`) are the
   same as in the Fall 2026 site's CLAUDE.md.

## Site-wide conventions (don't break these)

- Theme: `cosmo` + `theme.scss` / `theme-dark.scss`, Atkinson Hyperlegible
  font, teal `#D9E3E4` sidebar/footer, no top navbar.
- `styles.css`: bold blue top-level sidebar entries, 16px main content,
  schedule-table styling (`.schedule`, `.theme` labels, italic subtopics).
- `toc-arrows.html` (included on every page): expand/collapse arrows in the
  right-hand TOC, and it collapses the sidebar section literally named
  "Course Information" on load — if that section is renamed, update the
  string there too.
- `index.qmd` schedule: `[Theme Name]{.theme}` labels, `**bold**` lecture
  titles linked to the lecture `.qmd`, italic lines for subtopics and
  readings. Reading links come from the syllabus.

## Publishing

```bash
git pull --ff-only
git add -A
git commit -m "Short message"
git push            # GitHub Actions rebuilds the site (~2 min)
```

First-time setup after creating the GitHub repo: in the repo's Settings →
Pages, set the source to "Deploy from a branch" and the branch to `gh-pages`
(the Actions workflow creates that branch on its first run). Browsers cache
pages for 10 min — if a page looks stale after a deploy, Ctrl+F5.

Local render (optional): `quarto render` with Quarto on PATH.

## Windows long paths (why there are no theme folders on disk)

The OneDrive prefix of this clone is ~125 characters. With a `Theme_X-…/`
level under `Lectures/`, several lecture PDFs exceeded the Windows 260-char
`MAX_PATH` limit and became invisible to Python and Quarto on this machine.
Lectures therefore live directly under `Lectures/`; themes exist only as
sidebar sections in `_quarto.yml`. Keep lecture folder names under ~50
characters. `core.longpaths` is set in this clone's git config as well.
