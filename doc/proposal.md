# Main Proposal

**Work In Progress**

This contains a proposal for `_toc.yml`. Given `LaTeX` is the more restrictive
medium -- this approach is based on support LaTeX in the _toc.yml structure and
relating it back to HTML in as flexible a way as possible.

## Introduce Categories

Use a list approach that would
enable multiple files to be classified as `frontmatter`.
This could be useful when building books with `preface` and
secondary `frontmatter` sections.

````{panels}
Current
^^^^^^^
```yaml
- file: myintro

- part: My first part
  chapters:
  - file: part1_chapter1
  - file: part1_chapter2
- part: My second part
  chapters:
  - file: part2_chapter1
```
---
Proposed
^^^^^^^^
```yaml
- front:
  - file: intro.md

- part: My first part
  chapters:
  - file: part1_chapter1
  - file: part1_chapter2
- part: My second part
  chapters:
  - file: part2_chapter1
```
````

Alternatively, we could introduce **optional** named sections in the `yml` such as `frontmatter`, `abstract`, `backmatter`). This
could extend the current `toctree` directive to include wrappers
for different content types for neater alignment with `LaTeX` and
`html` themes built to support `book` style qualities.

```yaml
# [optional] Front Matter
frontmatter:
  - file: preface
  - file: intro
# [required] Main Matter (Current Implementation
# but main file is a generated index file and first file
# is a standard chapter / document equivalent to other files)
- file: chapter1
- part: A Group of Chapters
  chapter:
    - file: chapter2
# [optional] Abstracts
abstract: Abstract Group
  - file: abstract1
# [optional] Back Matter
backmatter:
  - file: bibliography
```

**Implementation Ideas:**

This could be organised by an updated
sphinx `toctree` that wraps documents inside the
appropriate nodes in the AST.

```rst
.. toctree::
   :frontmatter:

   preface
   intro
```

with the standard chapters being contained in a separate toc

```rst
.. toctree::
   :numbered:

   chapter1
   part1
```

and part1 document would contain section `toc`:

```rst
.. toctree::

   chapter2
```

If we were able to add `frontmatter`, `abstract`, `backmatter` nodes into the `AST` this would faciliate more flexibility in structuring
numbering behaviour based on context.

For `html` perhaps we should write an inliner for `toctree` to get the numbering correct. A single `toc` may aslo be a useful way to generate  `sitemap.xml`?
