---
name: mermaid-preview
description: Show a Mermaid diagram in the chat as an editable widget with Code and Preview buttons, so the user can tweak the diagram text and see it re-rendered on the spot. Use this whenever the user asks for a Mermaid diagram, flowchart, sequence diagram, architecture or process sketch, or wants to edit, adjust or iterate on a diagram interactively, even if they do not say "widget", "preview" or "editor". Also use it when the user pastes Mermaid code and asks what it looks like.
---

# Mermaid preview and edit widget

Solution Architects iterate on diagrams constantly. A static render means every small change costs a round trip to the chat. This skill puts the diagram text and its rendered result side by side in one widget, so the user can change a label, switch Code and Preview, and only come back to the chat when they want something bigger.

## When to use it

- The user asks for a flowchart, sequence diagram, architecture sketch or any other Mermaid diagram.
- The user pastes Mermaid code and wants to see or adjust it.
- The user says they want to edit a diagram themselves.

Skip the widget when the user only wants Mermaid text to paste into a file and has no need to see it first. Then give the code as described under "Giving the code as plain text".

## How to build it

1. Write the Mermaid code first. Keep it valid for Mermaid 10 (see "Writing diagrams that render reliably").
2. Load the visualizer guidance with `mcp__visualize__read_me` (module `interactive`) before the first widget call. Do this silently, without mentioning it to the user.
3. Read `assets/widget-template.html` from this skill folder. Replace the placeholder `__MERMAID_CODE__` with the diagram code. The code goes inside a `<textarea>`, so replace `&` with `&amp;` and `<` with `&lt;` in the code. Nothing else needs escaping.
4. Pass the result to `mcp__visualize__show_widget` as `widget_code`. Use a specific snake_case `title`, such as `solution_design_flow`.
5. Write one short sentence before the widget, and put explanations, assumptions and open questions in normal chat text after it. Keep prose out of the widget itself.

Do not change the template's structure unless the user asks. The Code and Preview buttons, the error line and the "Send to chat" button are the whole point. Do not add comments inside the widget code, because they break streaming.

## What the widget does

- **Preview** renders the diagram with Mermaid 10.9.1, loaded from cdnjs, in the team's flow style (see "Diagram style"). Syntax errors appear as a red line under the diagram instead of a broken picture.
- **Code** shows the editable text. Edits take effect when the user switches back to Preview.
- **Enlarge** switches to a taller layout: the code box grows, and the diagram is drawn at its natural size with horizontal scrolling when it is wider than the widget. The button then reads Shrink. This is not true full screen. A chat widget cannot go full screen and keeps a fixed width, so for a very large diagram suggest opening the code in mermaid.live.
- **Send to chat** posts the edited code back as a message. Treat that code as the current version of the diagram from then on. Never fall back to an earlier version you wrote yourself.

Edits made inside the widget exist only in the widget. They are lost if the chat redraws it, and they are never written to a file or repo on their own. If the user wants the diagram saved, they have to send it to the chat first, and then you commit it only when asked.

## Diagram style

The template styles every diagram as a clean, flat flow: soft blue rounded boxes with dark blue text, thin grey right-angle connectors, and white pill-shaped edge labels. The team chose this look because it reads well in documents and presentations, so write diagrams to suit it:

- Use `flowchart LR` by default. Use `TD` only when the flow is clearly vertical, such as a long approval chain.
- Draw steps as rounded boxes with quoted labels: `A1("Create repo")`. Break long labels with `<br/>` so boxes stay compact, for example `A1("Create GitHub repo<br/>and grant access")`. Remember the escaping in the textarea step above, so `<br/>` becomes `&lt;br/>`.
- Mark decisions, reviews and classification steps as dashed, paler boxes. Add these two lines at the bottom of the diagram, then list the node ids in the `class` line:

  ```
  classDef review fill:#F5FAFE,stroke:#B5D4F4,stroke-dasharray:4 3
  class A3 review
  ```

- Keep edge labels to one or two words, such as `Yes`, `No` or `Feedback`: `A3 -->|Feedback| B1`.
- Do not use `subgraph` swimlanes unless the user asks for lanes. Lane frames clash with the flat style, and the lane's owner can usually go into the box label instead.

The pill labels and rounded corners come from the widget's own styling. The same code pasted into GitHub or mermaid.live shows the colors from `classDef` but square corners and plain labels. The connectors have square bends because Mermaid 10.9.1 has no rounded option.

## Writing diagrams that render reliably

- Put node labels in double quotes. Labels with spaces, slashes or brackets break without them.
- Never use the lowercase word `end` as a node id or label. It closes a subgraph.
- Keep labels short. Long labels make wide diagrams that are hard to read in a chat-width widget. Five or six boxes in a row is about the limit.
- Prefer one idea per diagram. Offer a second diagram over a crowded first one.

## If the widget cannot be used

The widget needs the visualizer tools, and its preview needs the browser to reach cdnjs. If the tools are missing, or the user reports that the preview shows a library error, do not keep retrying. Give the code as plain text instead and suggest a place to view it, such as a Markdown file on GitHub (which renders Mermaid natively) or mermaid.live.

## Giving the code as plain text

Chat interfaces often render a Mermaid code fence as a finished diagram and hide the code. When the user needs to copy the code, use a fence tagged `text`, not `mermaid`. If the user wants to paste it into a Markdown file, tell them to change the tag to `mermaid` there.

## Repository conventions

Diagram sources are text and belong in the repository next to the documents they illustrate. Follow the folder layout the repository already uses, and ask the user where a new file should go instead of inventing a folder.
