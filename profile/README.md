<div align="center">

# 🛡️ DataShield

**Local-first PII/PHI de-identification platform — for teams, not just scripts.**

No cloud. No LLM. No external API calls. Detection and transformation run entirely on your own
machine or cluster.

</div>

---

DataShield finds and transforms PII/PHI in files, database tables, and structured exports — then
lets an organization run that as a governed, auditable, multi-user workflow instead of a one-off
script. Upload a file or point it at a database connection, pick a compliance policy (HIPAA, GDPR,
CCPA, PCI-DSS, SOC 2, or a custom one), review what the detection engine found, and download a
de-identified output with a full audit trail. Reversible runs issue a recovery key so authorized
users can re-identify later — everyone else only ever sees the de-identified version.

Everything — detection models, the database, the API, the UI — runs on `127.0.0.1`. Nothing about
your data or its contents is ever sent anywhere else.

## Why DataShield

- **Local-first, always** — Presidio + spaCy + a local RoBERTa NER pass, all on-device. No file
  content, detected entity, or token map is ever sent to a third-party API.
- **Fail-closed** — uncertain values are flagged for review, not silently passed through. Every
  policy has a default rule so nothing falls through un-transformed.
- **Multi-tenant from the ground up** — organizations, invites, and three fixed roles (Org Admin /
  Operator / Auditor). Sessions, policies, connections, and entity types can be private, shared
  with specific people, or org-wide.
- **More than files** — upload CSV/JSON/XML/Excel/Parquet/PDF/plain text, *or* connect directly to
  a database as a source and write de-identified output straight to a sink table.
- **Reversible when you choose it** — tokenize/pseudonym/encrypt operators can be reversed later
  with the session's recovery key + token map; every other operator is a one-way transform with
  nothing to leak.
- **Scales past a laptop** — a pluggable executor seam runs detection/de-identification
  sequentially by default, or distributed across a Spark cluster for large files.
- **Built-in + custom compliance** — HIPAA Safe Harbor, GDPR, CCPA, PCI-DSS, and SOC 2 ship out of
  the box; organizations can also define their own policies and entity types.

## Repositories

| Repo | What it is |
|------|-----------|
| [`data-shield-app`](https://github.com/DataShield-sp18/data-shield-app) | Main application — FastAPI backend + Next.js frontend, the de-identification/re-identification pipeline |
| [`data-shield-docs`](https://github.com/DataShield-sp18/data-shield-docs) | Public user manual (Docusaurus) |
| [`data-shield-terraform`](https://github.com/DataShield-sp18/data-shield-terraform) | Infrastructure as code |
| [`data-shield-floci-env`](https://github.com/DataShield-sp18/data-shield-floci-env) | Experiment repo — safe local deploy testing via the foci AWS emulator, feeding a future AWS-specific architecture |

## Security Model

- Localhost-only binding — the API never binds `0.0.0.0` outside Docker.
- Raw uploads never touch disk unencrypted — in-memory session state only.
- AES-256-GCM for the encrypt operator and token map; key material never logged.
- Audit log stores a hash of the original value, never the plaintext.
- No cloud egress — all detection models run locally.

## Contact

Questions or access requests — reach out to an org admin.
