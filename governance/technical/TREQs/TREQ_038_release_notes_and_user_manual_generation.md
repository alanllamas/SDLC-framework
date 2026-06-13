---
extends_schema: "/governance/templates/master_metadata.yaml"
id: TREQ_038
type: TREQ
title: "Release Notes & User Manual Delta Generation"
status: AUTHORIZED
version_tag:
  target_version: "v1.0.0"
  milestone: "v0.11.0 — Agent 8: Product Delivery Translator"
relationships:
  vertical:
    governing_bdr: null
    parent_id: TDR_021
  cross_plane:
    architecture_adr: ADR_004
  history:
    supersedes_document:
      document_id: null
      link_reason: null
provenance_ledger:
  generation_layer: { agent_identity: "Tech Lead", timestamp: "2026-06-03T00:00:00Z" }
  hitl_signatory: "Alan Llamas via Console Approval"
---

# Technical Requirement: TREQ_038 — Release Notes & User Manual Delta Generation

## 1. Intent & Scope
Defines generation logic for release notes and user manual
delta outputs, and the consumed-flag update on sign-off.

## 2. Technical Constraints

### Release Notes Generation
```python
def generate_release_notes(
    chronicle_ids: list[str], librarian: "Librarian"
) -> dict[str, str]:
    """Returns {milestone: markdown_content} grouped by
    version_tag.milestone across the given chronicles."""
    by_milestone = {}
    for cid in chronicle_ids:
        chronicle = librarian.read_chronicle(cid)
        ticket = librarian.read_dev_ticket(chronicle["relationships"]["parent_id"])
        milestone = chronicle["version_tag"]["milestone"]
        by_milestone.setdefault(milestone, {"FEATURE": [], "BUG": [], "DEBT": []})
        bullet = translate_to_plain_language(chronicle["technical_summary"])
        ticket_type = ticket.get("type", "FEATURE")
        by_milestone[milestone][ticket_type].append(bullet)

    outputs = {}
    for milestone, groups in by_milestone.items():
        content = [f"# Release Notes — {milestone}\n"]
        if groups["FEATURE"]:
            content.append("## New Features\n" + "\n".join(f"- {b}" for b in groups["FEATURE"]))
        if groups["BUG"]:
            content.append("## Fixes\n" + "\n".join(f"- {b}" for b in groups["BUG"]))
        if groups["DEBT"]:
            content.append("## Internal Improvements\n" + "\n".join(f"- {b}" for b in groups["DEBT"]))
        outputs[milestone] = "\n\n".join(content)
    return outputs
```
* Output path: `docs/release_notes/RELEASE_NOTES_[milestone].md`
* If file already exists (prior partial release notes for this
  milestone), TDR_008 diff presentation applies — new bullets
  appended, not a full rewrite.

### User Manual Delta Generation
```python
def generate_user_manual_delta(
    chronicle_ids: list[str], librarian: "Librarian"
) -> dict[str, str]:
    """Returns {section_slug: markdown_content}."""
    outputs = {}
    for cid in chronicle_ids:
        chronicle = librarian.read_chronicle(cid)
        ticket = librarian.read_dev_ticket(chronicle["relationships"]["parent_id"])
        placeholder = ticket.get("documentation_assignment")
        if not placeholder:
            continue
        slug = slugify(placeholder)
        outputs[slug] = translate_to_user_manual_section(
            placeholder_title=placeholder,
            technical_summary=chronicle["technical_summary"],
            ticket=ticket
        )
    return outputs
```
* Output path: `docs/user_manual/[section_slug].md`
* Existing section file → diff presentation per TDR_008,
  content appended/merged under existing heading.

### Consumed Flag Update
* On sign-off of either output type, for every chronicle_id
  that contributed content:
  `librarian.update_chronicle_consumed_flag(chronicle_id, true)`
  — atomic per TREQ_007, generates `CHRONICLE_CONSUMED` Ledger entry.

## 3. Verification & QA Criteria
* Chronicles grouped correctly by version_tag.milestone.
* Release notes sections (Features/Fixes/Internal Improvements)
  populated only when corresponding ticket types present.
* Existing release notes file → diff/append, not full rewrite.
* User manual delta uses ticket's documentation_assignment as
  section slug source.
* Sign-off sets consumed_by_agent_8=true on all contributing
  chronicles, logs CHRONICLE_CONSUMED per chronicle.
