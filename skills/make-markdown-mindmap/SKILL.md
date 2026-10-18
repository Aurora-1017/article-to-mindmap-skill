---
name: make-markdown-mindmap
description: Convert a pasted article or local Markdown/TXT file into a source-grounded 3–7 level Markdown mind map using nested bullets. Use for article summaries, study outlines, and structured notes; not for web scraping, PDFs, images, GUI output, or image export.
---

# Make Markdown Mindmap

Turn one article into a compact Markdown mind map made of nested unordered-list items. The result is a faithful structural extract, not a rewritten article or a generic summary.

## Input and scope

- Accept pasted text and local `.md`, `.markdown`, or `.txt` files.
- For a file path, read the file with the available file-reading tool. Do not infer content from its filename.
- Process one article per request. If several are supplied, ask whether to handle them separately; do not merge them silently.
- Do not browse, scrape, fetch URLs, extract PDFs or Office files, OCR images, or use outside knowledge as source material. If the input is a URL, PDF, image, or another unsupported format, say it is unsupported and ask for pasted text or a Markdown/TXT file.
- If the article is missing, empty, unreadable, or too truncated to understand, ask for the complete text. Do not guess.
- Treat only the supplied article as evidence. Never add facts, examples, definitions, comparisons, or conclusions that are not present in it.

## Analyze before writing

1. Identify the article's central subject and, when stated, its main claim or conclusion. Use that as the root.
2. Group the main supporting points around the article's own logic: dimensions, stages, causes and effects, problem and solution, claim and evidence, and so on. Keep one grouping logic among siblings.
3. Expand a branch only while the article supplies meaningful detail. A parent should name a category or claim; its children should give the reason, mechanism, evidence, condition, example, or consequence.
4. Merge repeated points at their most informative occurrence. Drop repeated background, promotional wording, navigation, credits, and anecdotes that support no point. Do not mirror the article sentence by sentence; keep a compact map rather than an exhaustive outline.
5. Check every node against the article. Keep numbers, names, dates, conditions, exceptions, and hedged wording intact.

## Hierarchy rules

Count the root as level 1. Target 3–7 levels.

- Level 1 is the central topic or claim; level 2 holds the main branches; level 3 adds supporting points; deeper levels hold evidence, conditions, or examples only when the article provides them.
- Do not create empty categories or padding just to reach three levels. If the source genuinely supports fewer than three meaningful levels, keep its natural depth and add one short note after the map: `注：原文信息不足以形成 3 层结构，已按实际内容保留 N 层。`
- Never exceed seven levels. If the source's natural structure is deeper, merge the deepest details into their level-7 parent as concise clauses instead of adding an eighth level.
- Prefer two to six children under a parent. Collapse a single-child chain unless the intermediate node represents a real category in the source.
- Make the root specific. Do not automatically use a clickbait title; if the article states no thesis, use a precise topic label rather than inventing a conclusion.
- Keep siblings distinct. If one sibling is a subtopic of another, nest it or merge them; if a child merely repeats its parent, merge or remove it.

## Fidelity

- Do not invent links between points. A sequence in the source does not prove a cause; keep the source's own relationship and strength.
- Preserve qualifiers such as “可能”, “部分”, “初步”, scope limits, and definitional wording such as “通常指”. Do not turn a possibility into a certainty.
- Treat branch labels as claims too. Apply the same hedging and attribution tests there, and do not drop an attribution such as “反对者指出” when a claim becomes a parent.
- Attribute opinions, predictions, and contested claims when the article does; do not present them as established fact.
- Keep a key term's brief explanation only when the article supplies it. Use `概念：简短解释` or `概念（简短解释）` inside the same node. Use the source's meaning; never add a dictionary definition from outside. If the article does not explain a term, leave it unglossed.
- Do not add a final “启示” or “行动建议” branch unless the article itself draws that conclusion.

## Output format

Return only the mind map, with no preamble, analysis, or closing commentary. The sole exception is the source-limited depth note described above.

Use this shape (illustrative only):

```markdown
- 核心主题
  - 一级分支
    - 支撑要点
      - 证据或条件
```

- Use exactly one top-level `- ` item as the root.
- Use two spaces per indent level. Use `- ` for every node; no tabs, headings, numbered lists, tables, or blank lines inside the map.
- Keep each node to a short phrase or clause with one idea. Do not end nodes with a period.
- Do not wrap the map in a code fence unless the user asks for copyable source text.
- Keep the article's main language, proper nouns, and technical terms. Translate only when the user asks.
- If the user asks to save the result, write only the map to the requested `.md` or `.txt` path and report the path. Do not add frontmatter or commentary to the file.
- Do not produce Mermaid, an image, or any other diagram format.

## Final check

Before returning, verify that:

- the map has one root and 3–7 levels, or a documented source-limited exception;
- every node is traceable to the supplied article;
- every child belongs under a broader parent and adds new detail;
- duplicate points have been removed;
- no node exists only to fill a level;
- key numbers, conditions, attributions, and hedges are preserved;
- the output contains the map only, plus the depth note if required.

Fix any failure before responding.
