# Report Template for Jira Comment

The comment is a delta to the ticket, not a restatement. Include only what the reader does not already know from the ticket itself. If the work matches the ticket exactly, the entire comment is the summary line and nothing else.

---

## <ISSUE_KEY>

One or two sentences. What was delivered, framed as outcome. Do not repeat the ticket title or requirements - assume the reader has read them.

### Verify (optional)

Include only when the diff exposes a path that is not obvious from the ticket - a hidden flag, an edge case, a side effect, a non-trivial integration the reader could miss. Skip entirely for self-explanatory tickets.

- Scenario and expected outcome
- Another non-obvious path

### Deploy (optional)

Include only for non-routine steps. Skip routine deploys such as pushing code, restarting services, clearing caches, or running standard migrations.

- Env var, feature flag, or migration the reader must act on
