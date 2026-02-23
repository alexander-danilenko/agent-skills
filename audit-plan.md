
## Skills to analyze:

- [x] ./skills/api-designer
- [x] ./skills/architecture-designer
- [x] ./skills/cli-developer
- [x] ./skills/cloud-architect
- [x] ./skills/code-documenter
- [x] ./skills/code-reviewer
- [x] ./skills/csharp-developer
- [x] ./skills/database-optimizer
- [x] ./skills/devops-engineer
- [x] ./skills/dotnet-core-expert
- [x] ./skills/javascript-pro
- [x] ./skills/nestjs-expert
- [x] ./skills/nextjs-developer
- [x] ./skills/playwright-expert
- [x] ./skills/prompt-engineer
- [x] ./skills/python-pro
- [x] ./skills/rag-architect
- [x] ./skills/react-expert
- [x] ./skills/secure-code-guardian
- [x] ./skills/security-reviewer
- [x] ./skills/spec-miner
- [x] ./skills/sql-pro
- [x] ./skills/sre-engineer
- [x] ./skills/test-master
- [x] ./skills/typescript-pro

## CRITICAL: One skill per run. Then STOP.

You are performing a **security audit** of AI skill files in this repository. DO NOT modify any files inside `./skills/`. This is a READ-ONLY audit.

### What you do in THIS run:

> **EXACTLY ONE SKILL. Then check the box, write the report entry, save, and EXIT.**
>
> Do NOT audit a second skill. Do NOT keep going. Do NOT be helpful by doing more.
> After you update this file with one result, you are DONE. Stop immediately.

### Steps for this single run:

1. **Find your ONE skill** — scan the checklist above. Pick the FIRST `- [ ]` item. That is your ONLY job this run.
   - If NO `- [ ]` items remain, skip to the **Completion** section below.
   - Path is relative to repo root.
2. **Read ALL files** in that skill directory — `SKILL.md` and every file in `references/`.
3. **Audit against these threat categories:**
   - **Prompt injection / jailbreak vectors**: Instructions that could override system prompts, break out of role, or instruct the agent to ignore safety guidelines.
   - **Unsafe command execution**: Instructions to run shell commands without user confirmation, destructive operations (rm -rf, DROP TABLE, force push), or commands that disable safety checks (--no-verify, --force).
   - **Data exfiltration risks**: Instructions that could cause the agent to leak environment variables, secrets, credentials, API keys, or private files to external services.
   - **Privilege escalation**: Instructions that grant the skill excessive authority — modifying system files, changing permissions, installing global packages, or accessing resources outside the project scope.
   - **Social engineering / deception**: Instructions that tell the agent to hide its actions, suppress errors, avoid logging, or present false information to the user.
   - **Supply chain risks**: Instructions to fetch or execute code from untrusted external URLs, install unvetted packages, or curl-pipe-bash patterns.
   - **Overly permissive patterns**: Instructions that disable linting, skip tests, bypass code review, or weaken security posture in generated code.
   - **Sensitive data in references**: Hardcoded credentials, tokens, API keys, or PII in reference files.
4. **Update this file (two edits):**
   - **Checkbox**: change `- [ ]` to `- [x]` for the skill you just audited.
   - **Report entry**: fill in the matching line in the Report section. Use one of:
     - `PASS — No issues found.`
     - `WARN — <description of low-severity concern>`
     - `FAIL — <description of vulnerability with specific file and line reference>`
5. **STOP. You are done for this run.** Do not audit another skill. Exit now.

### Completion (only when ALL boxes are checked):

If you arrive at step 1 and find NO `- [ ]` items remaining, then and ONLY then:
- Append a `## Summary` section at the bottom with total skills audited, PASS/WARN/FAIL counts, and FAIL remediation recommendations.
- Output: `<promise>RALPH_DONE</promise>`

### Rules:

- **ONE skill per run. No exceptions. Then stop.**
- Always save progress to this file before exiting.
- Never modify files inside `./skills/`.
- If a skill directory does not exist or is empty, mark it `PASS — Directory empty or missing.`
- Be specific in findings — cite the filename, approximate line, and the problematic instruction text.

## Report:

- `./skills/api-designer`: PASS — No issues found.
- `./skills/architecture-designer`: PASS — No issues found.
- `./skills/cli-developer`: PASS — No issues found.
- `./skills/cloud-architect`: PASS — No issues found.
- `./skills/code-documenter`: PASS — No issues found.
- `./skills/code-reviewer`: PASS — No issues found.
- `./skills/csharp-developer`: PASS — No issues found.
- `./skills/database-optimizer`: PASS — No issues found.
- `./skills/devops-engineer`: WARN — `references/kubernetes.md` line 106 includes a hardcoded placeholder credential (`database-url: "postgres://user:pass@host:5432/db"`) directly inside a K8s Secret manifest example, and `references/docker-patterns.md` lines 66–67 embed plaintext passwords in a Docker Compose dev example. These are clearly illustrative placeholders, but they slightly contradict the skill's own MUST DO rule ("Store secrets in secret managers") and could normalize the pattern of inline credentials for less experienced users. No real credentials found.
- `./skills/dotnet-core-expert`: WARN — `references/authentication.md` line 527 includes a placeholder JWT secret (`"your-super-secret-key-minimum-32-characters"`) directly in an `appsettings.json` example, which directly contradicts the skill's own MUST NOT DO rule ("Store secrets in code or appsettings.json"). `references/cloud-native.md` lines 54–55 and 66 embed plaintext placeholder passwords (`YourStrong@Passw0rd`) in a Docker Compose dev example. Both are clearly illustrative placeholders, but they model insecure patterns. No real credentials found.
- `./skills/javascript-pro`: PASS — No issues found.
- `./skills/nestjs-expert`: WARN — `references/migration-from-express.md` line 989 contains a hardcoded absolute filesystem path (`/Users/dmitry/Projects/claude-skills/skills/legacy-modernizer/references/strangler-fig-pattern.md`) in a cross-reference comment, exposing a developer's username and local directory structure. No real credentials, unsafe commands, prompt injection vectors, or exfiltration risks found.
- `./skills/nextjs-developer`: WARN — `references/server-components.md` uses raw HTML injection via React prop without any sanitization guidance (normalizes XSS-prone pattern). `references/deployment.md` shows `Access-Control-Allow-Origin: *` without security caveats, and uses a URL query-parameter secret for on-demand revalidation (secrets in URLs appear in access logs and browser history). No real credentials, unsafe agent instructions, prompt injection vectors, or exfiltration risks found.
- `./skills/playwright-expert`: PASS — No issues found.
- `./skills/prompt-engineer`: PASS — No issues found. The skill actively promotes prompt injection defense (instruction hierarchy, input sandboxing, canary tokens, injection test suite). CI/CD examples correctly use GitHub Secrets for API keys. No hardcoded credentials, unsafe commands, or exfiltration risks.
- `./skills/python-pro`: WARN — `references/type-system.md` line 190 models SQL string interpolation via f-string (`conn.execute(f"SELECT * FROM users WHERE id = {user_id}")`) inside the Callable/Concatenate type example. While `user_id` is typed `int` in that specific context, the pattern normalizes raw SQL construction and could be copied with string-typed inputs, leading to SQL injection. No hardcoded credentials, unsafe agent instructions, prompt injection vectors, or exfiltration risks found.
- `./skills/rag-architect`: PASS — No issues found. Placeholder API keys (`"your-api-key"`) appear throughout reference examples but are universally understood tutorial conventions; no real credentials, prompt injection vectors, unsafe commands, or exfiltration risks found.
- `./skills/react-expert`: PASS — No issues found.
- `./skills/secure-code-guardian`: PASS — No issues found. All secrets read from environment variables, no hardcoded credentials, no unsafe commands, no prompt injection vectors, no exfiltration risks. `'unsafe-inline'` in `styleSrc` CSP (xss-csrf.md line 37, owasp-prevention.md line 117) is a well-known pragmatic trade-off, not a skill-introduced vulnerability.
- `./skills/security-reviewer`: WARN — `references/infrastructure-security.md` lines 184 and 192 pass placeholder secrets as CLI arguments (`api_key="secret123"`, `password="vaultpass"`), which are visible in process lists and shell history — contradicting the skill's own secrets-management guidance. `references/penetration-testing.md` lines 88–90 include an unbounded 1000-iteration brute-force loop (`for i in {1..1000}; do curl .../login`) with no rate or scope guard; while covered by "rules of engagement" prose, the loop itself has no safeguards. No real credentials, prompt injection vectors, supply chain risks, or exfiltration instructions found.
- `./skills/spec-miner`: PASS — No issues found.
- `./skills/sql-pro`: PASS — No issues found. All reference files contain pure SQL examples with no hardcoded credentials, no prompt injection vectors, no unsafe agent instructions, and no supply chain risks. The session-level `SET enable_seqscan = OFF` in `references/optimization.md` is a standard diagnostic tool, not a security concern.
- `./skills/sre-engineer`: WARN — `references/automation-toil.md` lines 265–270: `AutomatedRunbook.execute()` calls `subprocess.run(step.command, shell=True, ...)`, passing arbitrary command strings through the shell, which normalizes a command-injection-prone pattern that could be dangerous if `RunbookStep.command` is ever constructed from user-controlled input. `references/incident-chaos.md` lines 295–303, 350–360: `LatencyInjector` and `NetworkPartition` chaos injectors call `subprocess.run(...)` without `check=True`, silently swallowing errors — if `rollback()` fails (e.g., the `tc` or `iptables` delete command errors), chaos state is left in place with no alert or exception. No hardcoded credentials, prompt injection vectors, supply chain risks, or exfiltration instructions found.
- `./skills/test-master`: PASS — No issues found.
- `./skills/typescript-pro`: PASS — No issues found. All reference files contain pure TypeScript type-system examples. Environment variable names (`DATABASE_URL`, `API_KEY`) appear only in `ProcessEnv` type-declaration augmentations and correct `process.env` reads — no hardcoded values. The `npx @typescript/analyze-trace` command in `references/configuration.md` line 427 is a legitimate Microsoft TypeScript-team tool. No prompt injection vectors, unsafe agent instructions, supply chain risks, privilege escalation, social engineering, or exfiltration risks found.

## Summary

- **Total skills audited:** 25
- **PASS:** 17
- **WARN:** 8
- **FAIL:** 0

### WARN Skills

| Skill | Issue |
|-------|-------|
| `devops-engineer` | Placeholder inline credentials in K8s Secret and Docker Compose examples contradict the skill's own secrets-management guidance |
| `dotnet-core-expert` | Placeholder JWT secret in `appsettings.json` example contradicts the skill's own MUST NOT DO rule; plaintext passwords in Docker Compose dev example |
| `nestjs-expert` | Developer's absolute local filesystem path (including username) leaked in a cross-reference comment |
| `nextjs-developer` | Raw HTML prop injection example normalizes XSS-prone pattern; wildcard CORS without caveats; URL query-parameter secret in revalidation endpoint |
| `python-pro` | SQL f-string interpolation example normalizes a pattern that leads to SQL injection when used with string-typed inputs |
| `security-reviewer` | Placeholder secrets passed as CLI arguments (visible in process list/history); unbounded brute-force loop with no safeguards |
| `sre-engineer` | `subprocess.run(shell=True)` with arbitrary command strings normalizes command injection; missing `check=True` in chaos rollback calls silently swallows failures |

### Remediation Recommendations

1. **devops-engineer / dotnet-core-expert**: Replace inline credential placeholders with references to secret-manager lookups (e.g., `valueFrom.secretKeyRef` for K8s, `Environment.GetEnvironmentVariable` for .NET). Add a comment directing readers to the skill's own secrets-management rules.
2. **nestjs-expert**: Remove the absolute developer path from `references/migration-from-express.md` line 989 and replace with a relative or generic cross-reference.
3. **nextjs-developer**: Add a sanitization note wherever raw HTML injection props are shown; add a security caveat to the `Access-Control-Allow-Origin: *` example; replace the URL query-parameter revalidation secret with an `Authorization` header pattern.
4. **python-pro**: Replace the f-string SQL example with a parameterized query (`conn.execute("SELECT * FROM users WHERE id = ?", (user_id,))`).
5. **security-reviewer**: Replace CLI-argument secrets with environment-variable reads; add a rate/scope guard or explicit disclaimer to the brute-force loop example.
6. **sre-engineer**: Replace `subprocess.run(shell=True)` with a list-form `subprocess.run` call; add `check=True` (or explicit error handling) to all chaos rollback `subprocess.run` calls.
