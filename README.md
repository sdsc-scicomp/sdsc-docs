# sdsc-docs

Coordination repository for modernizing the SDSC HPC user documentation.

## Contents

- `PLAN.md` - copy of the planning Google Doc (markdown), the master plan for the effort.
- `docs/` - static site published on GitHub Pages.
- `docs/lighthouse/<date>/` - Lighthouse accessibility report snapshots, one folder per capture date. Each dates folder contains the per-page report HTML for the two proof-of-concept sites:
  - `mkdocs/` - the MkDocs proof of concept (`sdsc-scicomp/expanse-docs`)
  - `docusaurus/` - the Docusaurus proof of concept (`sdsc-scicomp/expanse-docusaurus`)

## Published site

The dashboard is published on GitHub Pages:

`https://sdsc-scicomp.github.io/sdsc-docs/`

Open `docs/index.html` to browse the Lighthouse accessibility summary for each snapshot.
