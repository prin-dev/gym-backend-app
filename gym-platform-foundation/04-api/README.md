# API Directory

This directory initially defines API conventions and capability requirements.

The machine-readable OpenAPI documents are intentionally added only after the backend bootstrap:
- `openapi/gym-api-v1.yaml`
- `openapi/better-auth.json` (or the reviewed generated format)

After approval:
1. Record backend commit and generation tool/version.
2. Validate generated spec against committed canonical spec in CI.
3. Generate KMP client packages separately for auth and business APIs.
4. Do not hand-edit generated clients.
5. Changes to business behavior require updating the foundation contract and backend together.

See `api-guidelines.md` and `endpoint-capability-map.md`.
