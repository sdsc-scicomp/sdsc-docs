# SDSC HPC User Documentation Modernization Plan

**Author:** Andrea Zonca **Status:** Draft for discussion **Scope:** Cross-system user documentation for the SDSC HPC systems Expanse, Expanse2, TSCC, Voyager, and Cosmos, informed by the NERSC documentation model.

## 1\. Purpose

Adopt a modern, git-based documentation operating model for the SDSC HPC user guides. The goal is not only to replace the publishing technology, but to make documentation part of normal system operations: subject matter experts should be able to update, review, and maintain the authoritative content directly through a version-controlled workflow. Today, nearly all changes to the user guides are made by a single person after an email request, which separates the people who know the systems from the mechanism required to correct the documentation. The NRP documentation is currently the main SDSC example of contributors updating documentation directly.

## 2\. Motivation (why now)

- **Accessibility.** UC San Diego is a public university covered by the ADA Title II web accessibility rule. The issue is not static HTML itself; rather, the legacy templates and centralized publishing workflow make systematic accessibility remediation, automated testing, and prevention of regressions difficult. A shared documentation platform should make accessibility an ongoing engineering and authoring requirement.  
- **Expanse2 and newer systems.** Expanse2 is coming, and newer systems (Voyager, Cosmos) need fresh documentation. A reusable platform adopted now would pay off for every future system.  
- **The current workflow is a barrier.** Because changes are requested by email and applied by one person, contributors hesitate to recommend improvements. The new model should support both low-friction issue reporting and direct pull-request contributions, with version-controlled examples, step-by-step how-tos, and answers to common questions.  
- **Timeline.** Expanse is expected to run at least through September 2027, and a no-cost extension may push that later. Documentation work therefore remains worthwhile.

## 3\. Current state of documentation

| System | Current documentation | Git/markdown version |
| :---- | :---- | :---- |
| Expanse | Static HTML user guide on `sdsc.edu` | Yes, MkDocs proof of concept (`sdsc-scicomp/expanse-docs`, GitHub Pages via GitHub Actions) |
| Expanse2 | Not yet (future system) | No |
| TSCC | Static HTML user guide on `sdsc.edu` | No |
| Voyager | Static HTML user guide on `sdsc.edu` (needs substantial updates; the system is now in production) | No |
| Cosmos | Static HTML user guide on `sdsc.edu` (reflects the early user phase) | No |
| NRP | Standalone community documentation (`docs.nationalresearchplatform.org`), updated directly by contributors | Yes |

**Reference model: NERSC** (`https://docs.nersc.gov/`). NERSC publishes its documentation on MkDocs (Material theme), hosted on GitLab, and welcomes community contributions. It has a clear information architecture: Getting Started, Getting Help, QOSes and Charges, curated example job scripts, the basics of running jobs, Jupyter, Globus, Unix file permissions, MFA, and per-system resource pages. The NERSC layout is an excellent model for task-oriented information architecture. SDSC should copy these information-architecture and contribution principles rather than assume that NERSC's implementation stack must also be copied. The platform decision below is separate from the layout because Material for MkDocs is in final maintenance and approaching its announced end of life.

## 4\. Goals

1. One authoritative documentation site and operating model that subject matter experts can edit and review easily, with a shared core and clearly labeled system-specific content.  
2. All content stored in git and reviewed via pull or merge requests, with example scripts maintained as first-class versioned files and associated with the correct system.  
3. A structure that scales across systems (Expanse, Expanse2, TSCC, Voyager, Cosmos) and to future systems.  
4. Content organized so that common questions, in-depth how-tos, application-specific guidance, and system-specific facts are easy to add, search, and find without ambiguity about which machine they apply to.  
5. Compliance with WCAG 2.1 Level AA under the ADA Title II rule, plus any additional accessibility requirements that apply through specific federal contracts, awards, procurement, or other obligations.

### Non-goals for the initial phase

* Rewriting all SDSC system documentation at once.  
* Choosing a platform because of feature count or migration convenience rather than the agreed requirements and prototype results.  
* Requiring every contributor to use Git; simple issue reporting remains a supported contribution path.  
* Using framework documentation versioning to represent Expanse, Expanse2, Voyager, Cosmos, and TSCC as versions of one product.

## 5\. Accessibility and compliance

* Applicable standard: WCAG 2.1 Level AA. The Department of Justice Title II rule requires state and local government web content and mobile apps, including public universities, to conform to WCAG 2.1 Level AA. For deadline purposes, a state university uses the population of the state of which it is part rather than its student or employee count. Section 508 directly governs federal agencies; its electronic support-documentation requirements may still be relevant where a specific federal contract, award, procurement, or other obligation makes them applicable.  
* Deadline. The DOJ's 2026 Interim Final Rule extended the compliance date for public entities with a population of 50,000 or more to April 26, 2027 (public entities with fewer than 50,000 people and special district governments by April 26, 2028). For a state university, the relevant population is the population of the state of which it is part; student enrollment and employee count do not determine the deadline. Because UC San Diego is part of California, the applicable deadline is the 50,000-or-more date above.  
* Platform and theme conformance must be validated on the final SDSC site. An automated score, marketing language, or third-party conformance statement can be useful evidence but is not sufficient by itself to establish WCAG 2.1 AA conformance.  
* Verification approach. Make accessibility testing a deployment gate: build, markdown/content lint, link check, automated accessibility scan (axe-core, Pa11y, and/or Lighthouse), deploy preview, then production. Automated checks should be paired with manual keyboard testing and periodic screen-reader review, especially for navigation, search, tables, code blocks, custom components, and theme changes.  
* Content authoring rules. Enforce semantic heading hierarchy; descriptive link text; meaningful alt text for informative images; semantic table headers; accessible names and keyboard operation for controls; no information conveyed by color alone; captions/transcripts where applicable; and reduced-motion behavior where relevant. Custom MDX/components require additional review. Put these rules in CONTRIBUTING.md and lint what can be linted automatically. Provide a visible accessibility-feedback path on the production site.

## 6\. Proposed operating model and platform evaluation

### Platform comparison: Docusaurus and Zensical

- **Two production prototypes are being built from the same Expanse content: one with Docusaurus and one with Zensical. Neither platform is preselected. The prototypes should be evaluated against the same acceptance criteria so the final choice is evidence-based.**  
- Docusaurus is mature and actively maintained, uses Markdown/MDX and a Node/React ecosystem, deploys cleanly to GitHub Pages, and offers official first-class integration with Algolia DocSearch; community-maintained local search options also exist. Its maturity and extension model reduce platform risk, but migration from the existing MkDocs proof of concept is a larger rewrite.  
- Zensical is the modern successor being developed by the Material for MkDocs team. It has a new architecture, built-in client-side search, substantial compatibility with existing MkDocs/Material projects, and can build the current mkdocs.yml in parallel with MkDocs for a gradual, reversible comparison. It remains alpha software and is still rapidly evolving, so stability, plugin coverage, accessibility, and operational maturity must be validated before production adoption.

#### Prototype evaluation criteria

The same Expanse content should be evaluated against the criteria below. Accessibility, contributor/maintainer experience, reliability, and long-term maintainability should carry more weight than migration convenience alone.

| Criterion | Docusaurus prototype | Zensical prototype |
| :---- | :---- | :---- |
| **Accessibility** | **Mature base; validate the final theme, search, custom components, keyboard behavior, and screen-reader behavior.** | **Alpha and rapidly evolving; validate the final rendered site, search, navigation, and components carefully.** |
| **Existing Expanse POC migration** | **Larger migration and reconfiguration from the existing MkDocs/Material proof of concept.** | **Strong advantage: designed for MkDocs/Material compatibility and can build the same mkdocs.yml side-by-side.** |
| **Search** | **Official first-class Algolia DocSearch; local/community or self-hosted alternatives require a separate choice.** | **Built-in client-side search enabled by default; no required third-party search service.** |
| **Contributor / customization stack** | **Markdown/MDX; Node/React for advanced customization.** | **Markdown with strong MkDocs/Python continuity; newer runtime and configuration model.** |
| **Maturity / stability** | **Mature Docusaurus 3.x line and established ecosystem.** | **Officially alpha 0.0.x and iterating rapidly; validate stability and upgrade churn.** |
| **Plugins / extensions** | **Mature React/plugin ecosystem.** | **Growing native replacements for major MkDocs plugins; verify every plugin SDSC needs.** |
| **Versioning / archive fit** | **Native versioning exists, but Docusaurus itself warns it often adds unnecessary complexity. Prefer separate SDSC systems plus archival snapshots.** | **Native versioning is still evolving; not required if SDSC treats machines as separate systems and archives retired ones.** |
| **Operational burden** | **Mature Node dependency/toolchain; search choice may add an external service.** | **Lower migration burden and built-in search, but alpha churn may increase maintenance risk.** |

**Why not MkDocs Material.** The existing proof of concept and the NERSC site both use Material, but Material for MkDocs is in final maintenance and scheduled for end of life in November 2026\. New feature development has moved to Zensical. Material remains useful as the baseline implementation and migration reference, but the production decision should compare Zensical and Docusaurus rather than launch a new long-lived site on a framework approaching EOL.

### Hosting: SDSC GitHub organization

- Content lives in a repository under the SDSC GitHub org and is deployed to GitHub Pages.  
- Confirm the current SDSC GitHub organization plan, policies, and administrator configuration. Standard GitHub organization repository roles (Read, Triage, Write, Maintain, Admin), teams, and outside-collaborator access may already satisfy the contributor model; an upgrade should be considered only if SDSC needs additional enterprise capabilities.  
- Alternative: the HPC GitLab instance, which could serve as secondary hosting or as a backup.

### Integration with sdsc.edu

- Keep the documentation site standalone, as NERSC and NRP do, and link to it from each system's page on sdsc.edu. Prefer one authoritative SDSC HPC documentation site containing shared material and clearly separated system sections, unless organizational or operational constraints favor separate deployments.  
- Coordinate with Ben Tolo (SDSC web content lead) on integration with the SDSC web presence and on accessibility.

### Workflow and contribution model

- Changes are made via pull requests with review, and all content is tracked in git. CODEOWNERS or equivalent ownership rules should route system-specific changes to the appropriate reviewers, and protected branches should require review before publishing.  
- Contributors can add how-tos, FAQs, application examples, and corrections directly. Every page should expose an "Edit this page" action, and users who do not want to use Git should have a simple "Report a documentation problem" issue path.  
- A small set of documentation maintainers owns platform and site quality, while system owners and SMEs own technical correctness for defined areas. The repository should include a short contributor guide, an accessibility checklist, and clear review expectations.  
* Operational changes that affect user-visible behavior should include a documentation-impact check and, when needed, the corresponding documentation update before the change is considered complete.  
* High-volatility pages should have an explicit owner and periodic review cadence. Use approved analytics, search behavior, support tickets, and user-reported issues to prioritize improvements; analytics are a feedback tool, not a launch blocker.  
* Where practical, example job scripts should be stored as actual repository files and linted, syntax-checked, or smoke-tested rather than duplicated independently in multiple pages.

### Structure: Diataxis

- Use Diataxis to guide the purpose of individual pages—tutorials, how-to guides, reference, and explanation—but organize the visible navigation around HPC user tasks and SDSC resources rather than around the Diataxis labels themselves.  
- Use a shared core for concepts and workflows SDSC intentionally standardizes (getting started, Slurm concepts, storage concepts, data movement, authentication, file permissions) and system-specific pages for facts that can differ (hostnames, partitions/QOS, hardware, limits, filesystems, charge factors, software environment). System identity must be visible in page titles, navigation, search results, and examples so users do not apply instructions to the wrong machine.  
* Do not model Expanse, Expanse2, Voyager, Cosmos, and TSCC as versions of one documentation product. Treat them as separate systems. If a system retires, keep its documentation at stable URLs in a clearly marked read-only/archive state and remove it from the primary getting-started path.

## 7\. Initial content

Initial subject-matter scope is the current Expanse guide. Before final platform selection, build parallel Docusaurus and Zensical versions from the same representative Expanse content using the same information architecture and acceptance tests. After selecting the platform, migrate the full existing Expanse guide without turning the MVP into a complete editorial rewrite. During migration, correct clearly obsolete instructions, broken links, inaccessible structures, duplicated content, and other defects discovered along the way. Preserve existing public URLs through redirects at the page/section level where practical, and maintain a redirect map as a launch artifact.

## 8\. Decision gates / next steps

1. Gate 1 — Requirements and ownership: hold the SDSC web/docs planning meeting, including Ben Tolo (via Mahidhar), current maintainers, and representative SMEs; confirm scope, owners, accessibility responsibility, URL strategy, and hosting constraints.  
2. Gate 2 — Parallel prototypes: build Docusaurus and Zensical versions from the same representative Expanse content, including navigation, search, code examples, tables, redirects, CI, and "Edit this page" / issue-reporting flows.  
3. Gate 3 — Platform decision: evaluate the prototypes against the agreed comparison criteria and choose the production platform and hosting model. Confirm whether the existing SDSC GitHub organization configuration is sufficient before considering any upgrade.  
4. Gate 4 — Expanse MVP: migrate the full in-scope Expanse guide, remediate obvious defects, create the redirect map, and establish the shared-core/per-system structure in the selected repository.  
5. Gate 5 — Launch readiness: require build, link, lint, accessibility, preview, ownership, and redirect checks to pass; complete the agreed manual accessibility review.  
6. Gate 6 — Production launch and operations: publish the authoritative site, route old URLs appropriately, enable user and accessibility feedback, onboard contributors, and document the maintenance and review process.  
7. Gate 7 — Expansion: use the selected architecture for Expanse2, then migrate Voyager, Cosmos, and TSCC in the agreed priority order; improve tutorials, how-tos, application examples, and analytics-driven gaps incrementally.

## 9\. Success criteria

The modernization is successful when:

* The authoritative Expanse documentation source is in git and the production site is built from it.  
* Subject matter experts can submit corrections through page-level edit and pull-request flows, while non-Git users have a simple issue-reporting path.  
* Ownership is defined for platform maintenance and system-specific technical content, with review routing and a periodic review process for high-volatility pages.  
* Every pull request passes the required build, content/markdown lint, link, and automated accessibility checks and produces a reviewable preview.  
* The migration includes a maintained redirect map so existing public URLs continue to work where practical.  
* Search results and page design make the applicable SDSC system unambiguous.  
* Example job scripts are maintained as versioned files and linted or smoke-tested where practical instead of being copied independently across pages.  
* The selected site passes the agreed manual keyboard and screen-reader review and provides a public accessibility-feedback path.  
* The final platform choice is documented from the side-by-side Docusaurus/Zensical prototype results rather than from framework preference alone.

## 10\. Open questions

- Is straightforward access to the current Expanse content available (owners, permissions)?  
- Who acts as the maintainer/reviewer for each system, and what review turnaround is realistic?  
- Confirm the hosting decision (SDSC GitHub org vs GitLab), GitHub Pages/domain strategy, and whether the current organization features are sufficient.  
- Confirm the priority order across systems (Expanse2 first, or TSCC/Voyager/Cosmos concurrently).  
- Confirm the accessibility verification process and who owns it (automated checks plus manual review).  
- What are the final acceptance thresholds for Docusaurus versus Zensical, and who signs off on the platform decision?  
* Should the production site be one authoritative SDSC HPC documentation site or separate per-system deployments, and what domain/URL scheme should be used?  
* For the Docusaurus prototype, which search option is acceptable (Algolia DocSearch, a community/local implementation, or a self-hosted alternative), and what privacy or operational constraints apply?

## References

- Expanse docs MkDocs proof of concept (deployed): `https://sdsc-scicomp.github.io/expanse-docs/`  
- Expanse docs source repository: `https://github.com/sdsc-scicomp/expanse-docs/`  
- NERSC documentation (reference model): `https://docs.nersc.gov/`  
- NRP documentation (self-service example): `https://docs.nationalresearchplatform.org/`  
- Diataxis documentation framework: `https://diataxis.fr/`  
- Docusaurus (platform candidate): `https://docusaurus.io/`  
- Zensical (platform candidate and roadmap): https://zensical.org/about/roadmap/  
- Zensical MkDocs migration: https://zensical.org/docs/compatibility/mkdocs/migration/  
- Zensical search: https://zensical.org/docs/setup/search/  
- Docusaurus search: https://docusaurus.io/docs/search  
- Docusaurus versioning guidance: https://docusaurus.io/docs/versioning  
- GitHub organization repository roles: https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles/repository-roles-for-an-organization  
- ADA Title II implementation guidance (state-university population example): https://www.ada.gov/resources/web-rule-first-steps/  
- Section 508 applicability: https://www.section508.gov/manage/laws-and-policies/  
- MkDocs Material (end of life, November 2026): `https://github.com/squidfunk/mkdocs-material/releases`  
- Accessibility standards:  
  - ADA Title II final rule (WCAG 2.1 Level AA): `https://www.federalregister.gov/documents/2024/04/24/2024-07758/nondiscrimination-on-the-basis-of-disability-accessibility-of-web-information-and-services-of-state`  
  - Deadlines extension (Interim Final Rule, April 2026): `https://www.federalregister.gov/documents/2026/04/20/2026-07663/extension-of-compliance-dates-for-nondiscrimination-on-the-basis-of-disability-accessibility-of-web`  
  - Section 508 (Revised Standards, WCAG 2.0 AA): `https://www.section508.gov/`  
- Current SDSC system user guides:  
  - Expanse: `https://www.sdsc.edu/systems/expanse/user_guide.html`  
  - TSCC: `https://www.sdsc.edu/systems/tscc/user_guide.html`  
  - Voyager: `https://www.sdsc.edu/systems/voyager/user_guide.html`  
  - Cosmos: `https://www.sdsc.edu/systems/cosmos/user_guide.html`  
- DESC HPC Training retreat planning doc: `https://docs.google.com/document/d/15c5jp7rDJ8T0_upOmQ1u9zZZhI5SpaHAuZn2MWalGNs/edit`

## Appendix: Lighthouse accessibility audit (September 2026\)

Lighthouse CI was run on both proof-of-concept sites by the repos' lighthouse.yml workflows (GitHub Actions artifacts). The full HTML reports are saved in a Drive folder linked at the end of this appendix. These results support the platform recommendation in section 5\.

### Results summary

**MkDocs site (**https://sdsc-scicomp.github.io/expanse-docs/**)** \- 15 pages audited; accessibility score 79 to 84 (fails WCAG 2.1 AA). The same accessibility failures appear on every page:

* Elements use prohibited ARIA attributes  
* Elements with a role are missing required ARIA attributes  
* Buttons do not have an accessible name  
* Heading elements are not in sequential order  
* Links do not have a discernible name  
* Table cells in large tables are not associated with headers

**Docusaurus site (**https://sdsc-scicomp.github.io/expanse-docusaurus/**)** \- 5 pages audited; accessibility score 100 on every page, with no accessibility failures. Performance was lower (57 to 70), driven by render-blocking resources, unused CSS/JS, and unsized images. Those are performance issues, not accessibility issues.

### Interpretation

The automated audit confirms the recommendation in section 5: the Docusaurus proof of concept meets WCAG 2.1 AA accessibility out of the box, while the MkDocs Material site fails multiple AA checks on all pages, consistent with that theme's known accessibility gaps and its end-of-life status.

### Report files

Full Lighthouse HTML reports (15 for the MkDocs site, 5 for the Docusaurus site) are in the Drive folder:

Expanse-docs Lighthouse reports: https://drive.google.com/drive/folders/1dYrJ1lin9lIDhMPNh3DtkUnE\_pVEVJTf  
