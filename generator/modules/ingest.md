# Phase 1: Ingest

Fetch and normalize the migration guide into a single working file.

## Input

One or more URLs or local file paths provided by the user.

## Steps

### 1. Fetch sources

For each source provided:
- **URL:** Fetch the page content. Convert HTML to markdown — strip navigation, sidebars, headers, footers. Keep only the main content area.
- **Local file:** Read the file directly. If HTML, convert to markdown. If already markdown, use as-is.

### 2. Detect multi-page guides

After fetching each page, check if it is a **stub page** — a short page (under ~100 lines of content) that primarily consists of links to sub-pages. Common patterns:
- Wiki pages with a table of contents linking to child pages
- Index pages with section links
- Pages where >50% of the content is hyperlinks

If a stub page is detected:
- Follow each content link (skip external links, API docs, and issue trackers)
- Fetch and convert each linked page
- Preserve the link order as the content order

### 3. Concatenate

Combine all fetched content into a single working file. Preserve the heading hierarchy:
- If content comes from multiple pages, ensure no duplicate headings
- If a sub-page title repeats the parent page title, remove the duplicate
- Add a horizontal rule (`---`) between content from different source pages

Write the result to a temporary working file (this will be copied to the output directory during the compose phase).

### 4. Detect metadata

Read the normalized guide and identify:

- **Language:** Look for:
  - Java: Maven coordinates (`groupId:artifactId`), `import` statements, `.java` file references, `pom.xml`/`build.gradle` mentions
  - Go: Go module paths (`github.com/...`), `go.mod` references, `import` blocks
  - Python: pip/PyPI package names, `import` statements, `pyproject.toml`/`setup.py` references
  - .NET: NuGet package IDs, `using` statements, `.csproj` references

- **Source framework and version:** The framework being migrated FROM (e.g., Spring Boot 3, Django 4)
- **Target framework and version:** The framework being migrated TO (e.g., Spring Boot 4, Django 5)

If language or framework can't be confidently detected, ask the user.

## Output

Pass to the next phase:
1. The normalized guide content (working file)
2. Detected metadata: `language`, `source_framework`, `source_version`, `target_framework`, `target_version`
