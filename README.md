# E1 isolated synthetic PostgreSQL CI sandbox

This public repository contains only a disposable synthetic PostgreSQL fixture, security-negative tests, fixed codec vectors, and one manually triggered GitHub Actions workflow. It contains no Core application source, customer records, credentials, hosting deployment configuration, or external database connections.

## Approval and one-run controls

The workflow accepts only `workflow_dispatch` on `main` in `RiGi26/webzoka-e1-ci-sandbox`. The dispatch input `approved_sha`, repository variable `E1_ISOLATED_CI_APPROVED_SHA`, and actual workflow commit must all match exactly. Run attempts other than 1 are rejected. A SHA variable is not a consumable token: the Owner-approved single dispatch, protected environment, recorded run ID, and removal of the temporary approval are required together. Do not redispatch or rerun without new authority.

Environment `e1-isolated-ci` requires human review by Owner `RiGi26`, permits only the branch `main`, and disallows administrator bypass. The Owner authorized self-review only for this maximum one-run window. After execution or failure, self-review prevention must be enabled again and temporary SHA authorization removed. The automation must never approve its own environment job.

Only the standard public GitHub-hosted `ubuntu-latest` runner is permitted. Token permissions are `contents: read`; checkout credentials are not persisted. There are no artifact uploads, caches, paid runners, application builds, or automatic workflow triggers.

## Isolation and assertions

The approved configuration and single SQL fixture are copied into a new runner-temporary directory. The checksum-pinned CLI starts only PostgreSQL using the reviewed image digest, with other services disabled. CLI start and exact-project cleanup run with a cleared environment. No login, linked project, cloud database, real migration, seed, remote URL, or real E1 writer is used.

Tests check the container identity and image, effective role privileges, SELECT-only access, RLS/FORCE RLS flags, synthetic cross-Company read restrictions, denied writer invocations and client identity fields, codec vectors, and unchanged synthetic records. Cleanup checks that the exact fixture containers and volumes are absent. Bootstrap, tests and cleanup have time limits; runner disposal is the final isolation boundary.

The fixture uses fabricated UUIDs and a supplied synthetic GUC. It does not validate Supabase Auth, session revocation, trusted Core identity transport, managed Supabase role equivalence, real E1 locking/rollback, or Production behavior. E1 remains disabled. Typed PostgreSQL UUID acceptance is intentionally broader than the strict string reference gate.

## Provenance and local validation

The SQL and TOML bytes are unchanged from the previously reviewed H1.G1 six-file candidate. Codec vectors are frozen public-safe JSON derived from that candidate's shared synthetic test vectors; no protected source module is published. Tests and workflow are adapted only for this standalone repository, manual dispatch, exact-SHA controls, and public-safe output.

Local checks use `node --check tests/e1-negative.test.mjs` and `node --test tests/e1-negative.test.mjs`. Local checks are static only. SQL runtime requires the exact approved Actions context and disposable container; no workstation container or database installation is permitted.
