# avq_sa_ai_poc

Proof of concept: using an AI assistant (Claude) to support Solution Architects in producing solution design documents.

> **Status:** PoC / experimental. Content may change without notice.

## Goal

Find out whether a repeatable AI-assisted workflow can help Solution Architects move from source material to a reviewed solution design document faster, without losing quality or traceability.

## Workflow

1. The Solution Architect creates a GitHub repository (or uses this one) and grants the Claude GitHub App access to it.
2. The Solution Architect creates a Claude Project for the topic and uploads the relevant documentation.
3. With that context in place, the Solution Architect and the assistant draft the solution design document together.

## Capabilities being evaluated

| Capability | Purpose |
|---|---|
| Read and write Word (.docx) | Source documents and the design document deliverable |
| Read and write Excel (.xlsx) | Assessments, inventories, estimates |
| Read Visio (.vsdx) | Existing architecture diagrams |
| Create Mermaid diagrams | Diagrams as text, rendered natively on GitHub |
| Create and edit draw.io diagrams | Editable diagrams for stakeholders |

Visio files are read as structure (shapes, text, connections), not visual layout. For best results, also provide a PDF or image export.

## Conventions

- **Diagrams as text.** Keep Mermaid and `.drawio` sources in the repo so they can be diffed and reviewed.
- **Authorship.** Commits made by the assistant use the author name "AI contributor". A human reviews everything the assistant produces.
- **Confidentiality.** Do not commit customer-identifying or confidential data unless it has been approved for this repository.

## Open questions

- Which document template(s) should the assistant follow?
- Which parts of the workflow benefit most from automation?
- What review and sign-off steps are required for AI-assisted documents?
