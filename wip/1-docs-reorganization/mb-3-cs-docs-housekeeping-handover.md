# mb-3-cs Docs Housekeeping — Handover

**Purpose:** Status of the `mb-3-cs` `project-library` document-sorting work, separate from the
PDLF-system handover (`pdlf-handover-document.md`). This was real housekeeping on a live project,
not PDLF development — bring this into the next session if you continue it there.

---

## 1. The rules that were established (apply going forward)

1. **`docs/` = 100% canonical to this app.** Nothing reusable ever lives there. Fully emptied on
   migration to a new app.
2. **`specifications/` = Mybizz-wide specification content.** Persists across every app.
3. **`policy/` = Mybizz-wide policy content.** Persists across every app. Separate from
   specifications — a policy is a governance rule ("how decisions get made"); a specification is
   a technical standard.
4. **Zero duplication, anywhere.** Any fact/value/rule has exactly one home; every other document
   references it, never restates it.
5. **`templates-documents/`** holds blank templates for standard document types. The moment a
   template is filled in for this app, the result is canonical and moves to `docs/` — the
   template itself is never edited.
6. **ADR split (global vs. local) is designed but explicitly unresolved** — do not act on this
   yet, criteria not decided.
7. **On new-app migration, these clear/reset**: `.scratch/`, `gstack-outputs/`,
   `matt-skills-output/`, `memory/`, `opencode-outputs/`, `project-library/pdlf/`, `prompts/`, `screens/`,
   `wireframes/`, `V2/`, `workspace/`, `WIP/`, and `docs/` itself.
8. **`.opencode/`** is OpenCode's own concern at init, not a PDLF/manual folder-setup task.

**One thing worth checking, not yet confirmed either way:** early in this work, a filename-prefix
convention was used (`spec-` prefix = specification, no prefix = canonical, `-policy` suffix =
policy) as the determinant of category *regardless of folder*. Later, folder location itself
became the determinant (rule 1–3 above). It's not confirmed whether the prefix convention is now
redundant (folder membership alone decides) or still meaningful alongside folder placement —
worth resolving so files aren't carrying a now-meaningless prefix.

## 2. Splits completed this session (produced as artifacts, not yet physically applied by you)

- **`vault-system.md`** trimmed to app-specific only; generic Vault pattern → `spec-security.md`.
- **`spec-coding-standards.md`** fully absorbed into `spec-anvil-platform-standards.md` (Sections
  15–18 added) — safe to delete, nothing lost.
- **`spec-development-policy.md`** resolved as a Policy/Specification mix. Genuine policy content
  → `development-policy.md` (no prefix). Its one unique specification fact (M3 native/partial
  component list) → `spec-m3-design-standards.md` Section 18. The original mixed file's fate
  (delete vs. keep) was not explicitly confirmed — check before removing.
- **`test-specifications.md`** → `spec-testing.md` (Canonical, this app's actual test plan).
  `testing-standards.md` retired — its content was already in `testing-methodology-standards.md`.
- **`platform-architecture.md`** trimmed — five-app model → new
  `specifications/five-app-architecture-model.md`.
- **`security-architecture.md`** trimmed twice — reusable security patterns →
  `spec-security.md` (earlier pass); regulatory baseline → new
  `specifications/regulatory-compliance-baseline.md` (this session).

**Important caveat: none of the above were physically written to your real filesystem by me** —
I only have the documents you upload into this conversation. Every file I "produced" is a
proposed replacement; the actual copy/delete/move on disk is yours to perform.

## 3. Confirmed relocations (destination decided, physical move not verified done)

- `client-activation-runbook.md`, `onboarding-implementation-plan.md` → `specifications/`
  (confirmed Mybizz-wide).
- `architectural-sanity-audit.md`, `concept-sanity-audit.md`, `ideal-state-artifact.md` →
  confirmed already in `project-library/pdlf/` (not a `docs/` matter).
- `clarifications.md`, `questions-to-raise-with-anvil.md` → `wip/` (resolved-issue logs / running
  question lists, not live reference).
- **Correction to note carefully:** `discovery.md` was renamed `pricing-strategy-consultation.md`
  and I initially said it should go to `wip/` — **you overrode this** and confirmed it **stays in
  `docs/`**. Don't move it to WIP if picking this back up; the WIP recommendation was superseded.

## 4. Duplicate/dead files identified for deletion

- `security-architecture-old.md` — fully superseded, content-verified as present in the new file.
- `theme_mui_patch.css`, `theme_patched.css` — both redundant; the live `theme.css` already has
  the patch merged in.
- The four `(2)`-suffixed files in `specifications/` — you confirmed these already removed.

## 5. The final `templates-documents/` copy list (just produced, not yet executed)

20 files, from the current `docs/` listing you gave me: `architecture.md`,
`authoritative-schema.md`, `build-plan.md`, `business-requirements-document.md`, `CHANGELOG.md`,
`CONTEXT.md`, `custom-component-requirements-matrix.md`, `deployment-procedures.md`, `DESIGN.md`,
`globals-contract.md`, `integration-architecture.md`, `observability-requirements.md`,
`platform-architecture.md`, `product-requirements-document.md`, `README.md`, `scaffold-spec.md`,
`security-architecture.md`, `software-requirements-specification.md`, `user-flows.md`,
`vault-system.md`.

**Not a copy candidate, flagged as misplaced:** `anvil-platform-standards.md` (belongs in the
PDLF Standards Library, not `mb-3-cs`'s `docs/`) and a stray PDLF-system task file that had landed
in `docs/` by accident.

**Not yet done:** an equivalent list for the `checklists/` folder was never produced — only the
two-stage screen-compliance checklist *pattern* was discussed, not a concrete list of blank
checklist templates to seed.

## 6. Real open items, unresolved

1. **`README.md` needs a full rewrite.** The current one describes the *old* structure — no
   mention of `specifications/`, `policy/`, `templates-documents/`, the cleared-folder list, or
   the ADR split; uses the old `C:\mybizz\...` path convention; and references a devlog/todo
   system (`C:\mybizz\devlog\`, `todo\`) whose current status was never confirmed.
2. **Is that devlog/todo system still live, or superseded?** Asked directly, never answered —
   don't assume either way before touching the README.
3. **ADR global/local split** — designed, criteria undecided, explicitly not started.
4. **`spec-development-policy.md`'s original file** — confirm whether it's deleted now that its
   content is split, or still sitting there as a stale duplicate.

---

*mb-3-cs docs housekeeping handover v1.0. Companion to `pdlf-handover-document.md` and
`pdlf-outstanding-items.md`, but this file is about the live project's document housekeeping, not
PDLF-system development.*
