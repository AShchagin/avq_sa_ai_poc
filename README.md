# avq_sa_ai_poc

Proof of concept: using an AI assistant (Claude) to support Solution Architects in producing solution design documents.

> **Status:** PoC / experimental. Content may change without notice.

## Goal

Find out whether a repeatable AI-assisted workflow can help Solution Architects move from source material to a reviewed solution design document faster, without losing quality or traceability.

## Workflow

1. The Solution Architect creates a GitHub repository (or uses this one) and grants the Claude GitHub App access to it.
2. The Solution Architect creates a Claude Project for the topic and uploads the relevant documentation.
3. With that context in place, the Solution Architect and the assistant draft the solution design document together.
4. The assistant commits its work as "AI contributor", and the Solution Architect reviews it and gives feedback until it is ready.

## Repository structure

| Folder | Purpose |
|---|---|
| `00.Archive/` | Superseded or retired material |
| `01.Master documents/` | Source specifications, document templates and the master solution designs |
| `02.Diagramms/` | Diagram sources (`.drawio`, Mermaid) |
| `03.Client versions/` | Client-specific versions generated from a finalized master document |
| `.claude/skills/` | Reusable skills for the assistant (see below) |

Git does not track empty folders. Empty folders hold a placeholder file, `.gitkeep`, which can be deleted once the folder has real content.

Templates currently in `01.Master documents/`: the F317 Solution Description templates (full and simple) and the Requirement Form.

## Planned use cases

Not yet tested. Each one starts from the same flow: source material in the repo, a draft by the assistant, review by the Solution Architect.

| Use case | What the assistant does |
|---|---|
| Regulatory projects | Drafts a master solution design per regulation, then creates client-specific versions after review |
| Released upgrades | Same flow, based on release lists, the CaseWise banking map export and CUGs |
| Upselling | Finds similar delivered solutions, helps fill the high-level estimate (HLE) matrix, prepares Jira tickets, drafts the client design |
| FINMA | Reviews exported Excel matrices for inconsistencies and notifies the reference Solution Architect |
| MVP Factory | Proposes changes for a demo or PoC environment; the Solution Architect deploys |

Upselling and FINMA need Jira and SharePoint access, which is not yet enabled.

## Capabilities being evaluated

| Capability | Purpose |
|---|---|
| Read and write Word (.docx) | Source documents and the design document deliverable |
| Read and write Excel (.xlsx) | Assessments, inventories, estimates |
| Read Visio (.vsdx) | Existing architecture diagrams |
| Create Mermaid diagrams | Diagrams as text, rendered natively on GitHub |
| Create and edit draw.io diagrams | Editable diagrams for stakeholders |

Visio files are read as structure (shapes, text, connections), not visual layout. For best results, also provide a PDF or image export.

## Skills

| Skill | Purpose |
|---|---|
| `mermaid-preview` | Shows a Mermaid diagram in the chat as an editable widget, with a full-size editor page |
| `drawio-flow` | Creates and reads draw.io flow diagrams in the team style on the standard A4 page |

Skills live in `.claude/skills/` so that anyone who uses this repository gets them.

## Conventions

- **Diagrams as text.** Keep Mermaid and `.drawio` sources in the repo so they can be diffed and reviewed.
- **Authorship.** Commits made by the assistant use the author name "AI contributor". A human reviews everything the assistant produces.
- **Confidentiality.** Do not commit customer-identifying or confidential data unless it has been approved for this repository. This includes Avaloq source code and documentation.

## Open questions

- Which of the F317 templates should the assistant follow, and for which use case?
- How can the assistant learn ACP and Avaloq Script, and what material may be stored here?
- Which parts of the workflow benefit most from automation?
- What review and sign-off steps are required for AI-assisted documents?
