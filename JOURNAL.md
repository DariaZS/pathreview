# PathReview Contribution Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/159

**Issue title:** structlog output is not captured by pytest caplog — log assertions fail suite-wide

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The application logs through structlog, but structlog isn't configured to propagate its output into Python's standard library logging system during tests. Because pytest's `caplog` fixture only sees events that go through stdlib logging, every test that asserts on `caplog` fails — even though the code under test is correctly emitting the expected log event (visible directly on stderr). This is a suite-wide problem, not a single-file bug, since any test anywhere that checks "did we log this correctly" depends on the same broken wiring. A successful fix configures structlog in `tests/conftest.py` — likely using `structlog.stdlib` processors or a `capture_logs` helper — so that `caplog`-based assertions correctly detect the log events again.

**Scope reasoning ("Is this right for me?" checklist):**
- *Actually open?* Confirmed via the issue sidebar — no branches or linked PRs. One other commenter (`amanadhav`) stated intent to work on it but has no commits or branch yet.
- *Scope clear?* Yes — the issue names the exact root cause (structlog not propagating into stdlib logging), gives concrete reproduction steps, and points to the likely fix location (`tests/conftest.py`).
- *Right size?* Likely small — this is a test-configuration fix localized to one file, not a multi-service change.
- *Maintainer active?* N/A — this is a class repository, not an actively maintained open-source project.
- *Matches where I am?* Yes — I spent this week's setup debugging Docker health checks and container logging output, so I have direct, recent experience with "the underlying behavior is correct, but the check/assertion around it is misconfigured," which is exactly this bug's shape.

**Branch name:** docs/159-structured-caplog-fix

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger