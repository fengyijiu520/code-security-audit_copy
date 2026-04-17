---
name: code-security-audit
description: Evidence-driven bilingual code security audit skill for source-code review, vulnerability hunting, exploitability triage, remediation planning, and fix retesting. Use when users ask to audit code, review security, find vulnerabilities, scan for OWASP/API/auth/business-logic risks, prepare penetration testing, verify fixes, or analyze Java, Python, Go, PHP, JavaScript/TypeScript/Node.js, C/C++, .NET/C#, Ruby, Rust, mobile, serverless, LLM, infrastructure, or dependency security. Focus on real code evidence, source-to-transfer-to-sink proof chains, deduplicated root causes, severity by exploitability and blast radius, P0/P1/P2 remediation, and retest checklists.
---

# Code Security Audit

Deliver actionable, fix-ready security results, not keyword-hit lists. Match the user's language for the final report; use Chinese when the user writes Chinese, English when they write English, and bilingual terms when useful.

## Core Contract

- Base every finding on files actually read from the target project.
- Never report a vulnerability from a grep hit alone.
- For each finding, prove `Source -> Transfer -> Sink`, or downgrade it to `SUSPECTED`/`INFO`.
- Prefer fewer verified findings over many speculative ones.
- Deduplicate repeated vulnerable call paths into one root-cause finding and separately count instances.
- Prioritize exploitability and blast radius over CWE label prestige.
- Do not perform destructive, live exploitation, credential use, or networked attacks unless the user explicitly authorizes them.

## Modes

Select the mode from the user's request. If unspecified, use `standard`.

| Mode | Use When | Output |
| --- | --- | --- |
| `quick` | User asks for fast scan, CI check, quick triage, or top risks only | High-risk candidates validated as far as possible; P0/P1 only |
| `standard` | Normal security audit or code review | Full static audit over relevant attack surfaces; confirmed and suspected findings |
| `deep` | User asks for deep/full/comprehensive audit, penetration-test preparation, compliance, or critical systems | Multi-pass attack-surface, taint, control, business-logic, and attack-chain review |
| `quick-diff` | User asks PR/diff/incremental review or provides a base/head range | Only changed files plus direct callers/callees and changed configs/dependencies |

## Required Workflow

1. Scope the target, exclusions, tech stack, build files, frameworks, roles, trust boundaries, and deployment assumptions.
2. Inventory attack surfaces: HTTP/API, RPC, MQ, scheduled jobs, CLI, file import/export, admin/internal routes, auth flows, storage, external services, and dependency/config surfaces.
3. Load only the references needed for the detected stack and mode.
4. Discover candidates with semantic reading, `rg`, framework routing maps, dependency/config inspection, and optional static tools.
5. Convert candidates into evidence chains by reading code from input source through transforms and authorization checks to dangerous sinks.
6. Verify exploit prerequisites, sanitizers, allowlists, auth/tenant checks, feature flags, and environment constraints.
7. Deduplicate by root cause and assign severity/confidence.
8. Produce the report with remediation and retest steps using `templates/report-template.md`.

Do not skip steps unless the user explicitly requests `quick` mode.

## Reference Loading

Load progressively. Do not read the entire reference tree by default.

Always available:
- `templates/report-template.md` for final report shape.
- `references/quick-grep-rules.md` for quick triage and candidate discovery.

Load for all `standard` and `deep` audits:
- `references/checklists/coverage_matrix.md`
- `references/checklists/universal.md`
- `references/core/anti_hallucination.md`
- `references/core/comprehensive_audit_methodology.md`
- `references/core/taint_analysis.md`
- `references/core/sinks_sources.md`
- `references/core/verification_methodology.md`
- `references/core/false_positive_filter.md`

Load by language:
- Java: `references/checklists/java.md`, `references/languages/java.md`, and specific Java deep dives as needed: `java_deserialization.md`, `java_fastjson.md`, `java_gadget_chains.md`, `java_jndi_injection.md`, `java_script_engines.md`, `java_xxe.md`, `java_practical.md`.
- Python: `references/checklists/python.md`, `references/languages/python.md`, and `python_deserialization.md` when serialization/YAML/pickle is present.
- Go: `references/checklists/go.md`, `references/languages/go.md`, and `go_security.md` for concurrency, `unsafe`, cgo, SSRF, and Gin-style projects.
- PHP: `references/checklists/php.md`, `references/languages/php.md`, and `php_deserialization.md` for POP chains, Phar, upload, and include risks.
- JavaScript/TypeScript/Node.js: `references/checklists/javascript.md`, `references/languages/javascript.md`.
- .NET/C#: `references/checklists/dotnet.md`, `references/languages/dotnet.md`.
- C/C++: `references/checklists/c_cpp.md`, `references/languages/c_cpp.md`.
- Ruby: `references/checklists/ruby.md`, `references/languages/ruby.md`.
- Rust: `references/checklists/rust.md`, `references/languages/rust.md`.

Load by framework when detected:
- Spring: `references/frameworks/spring.md`
- Java web/Shiro/Struts-style stacks: `references/frameworks/java_web_framework.md`
- MyBatis: `references/frameworks/mybatis_security.md`
- FastAPI: `references/frameworks/fastapi.md`
- Django: `references/frameworks/django.md`
- Flask: `references/frameworks/flask.md`
- Express: `references/frameworks/express.md`
- Koa: `references/frameworks/koa.md`
- NestJS/Fastify: `references/frameworks/nest_fastify.md`
- Gin: `references/frameworks/gin.md`
- Laravel: `references/frameworks/laravel.md`
- Rails: `references/frameworks/rails.md`
- Rust web: `references/frameworks/rust_web.md`
- .NET web: `references/frameworks/dotnet.md`

Load by security domain when the project surface indicates it:
- API/REST/GraphQL: `references/security/api_security.md`, `references/security/graphql.md`
- OAuth/OIDC/SAML/JWT/JWK: `references/security/oauth_oidc_saml.md`, `references/security/cryptography.md`
- Gateway/proxy/cache/host header: `references/security/api_gateway_proxy.md`, `references/security/cache_host_header.md`, `references/security/http_smuggling.md`
- File upload/read/traversal: `references/security/file_operations.md`
- Auth/business logic/IDOR/tenant isolation: `references/security/authentication_authorization.md`, `references/security/business_logic.md`, `references/security/cross_service_trust.md`
- MQ/async/jobs/realtime: `references/security/message_queue_async.md`, `references/security/scheduled_tasks.md`, `references/security/realtime_protocols.md`
- Race/concurrency: `references/security/race_conditions.md`
- Dependencies/supply chain/IaC: `references/security/dependencies.md`, `references/security/infra_supply_chain.md`
- Crypto/secrets/logging: `references/security/cryptography.md`, `references/security/logging_security.md`
- LLM/serverless/mobile/native: `references/security/llm_security.md`, `references/security/serverless.md`, `references/security/mobile_security.md`, `references/security/memory_native.md`

Use WooYun references only as pattern inspiration, never as proof for the target project:
- `references/wooyun/INDEX.md`
- Relevant case family files under `references/wooyun/`

## Evidence Labels

- `CONFIRMED`: Complete code-backed source-to-transfer-to-sink chain, reachable source, no effective mitigation, and concrete exploit path or reproducible proof.
- `HIGH_CONFIDENCE`: Complete chain and missing/weak mitigation, but runtime-specific exploit details remain untested.
- `SUSPECTED`: Partial chain, grep/semantic candidate, uncertain reachability, or incomplete preconditions.
- `INFO`: Hardening issue, design concern, or low-confidence clue.

Downgrade confidence when a source, sink, reachability condition, or missing-mitigation proof is incomplete.

## Severity Rules

- `Critical`: Unauthenticated or low-complexity RCE, arbitrary file write leading to code execution, full database compromise, cross-tenant takeover, or cloud/control-plane compromise.
- `High`: Auth bypass, privilege escalation, IDOR with sensitive data or cross-tenant impact, high-impact SSRF, exploitable injection, unsafe deserialization, broad secret exposure.
- `Medium`: Authenticated exploit with meaningful but bounded impact, constrained SSRF, stored XSS with realistic victim path, moderate information disclosure, business-logic abuse with limited blast radius.
- `Low`: Narrow local impact, weak hardening, verbose errors, low-value information exposure, or exploit paths requiring strong preconditions.
- `Info`: Defensive observations without demonstrated exploitability.

When attack chaining changes reachability or impact, rate both the individual issue and the combined chain.

## Output Contract

Every finding must contain these 8 fields:

1. Location: file/class/method/API and line when available.
2. Type + root cause.
3. Source -> transfer -> sink evidence chain.
4. Exploit prerequisites.
5. Impact scope.
6. Severity + evidence label.
7. Minimal-fix remediation.
8. Retest checklist.

The final report must also include:
- Audit scope and mode.
- Attack surface inventory.
- Raw vulnerability instance count.
- Deduplicated root-cause finding count.
- P0/P1/P2 remediation plan.
- Assumptions, uncertainty, and runtime validation still required.

## Candidate Discovery

Use `rg` for search. Prefer targeted, stack-aware searches rather than broad noisy patterns.

For `quick` mode:
1. Read `references/quick-grep-rules.md`.
2. Run high-signal searches for injection, command execution, deserialization, XXE, SSRF, auth/authz bypass, upload/read/traversal, secrets, and business-logic risks.
3. Group hits into candidate chains.
4. Validate the Top exploitable P0/P1 chains first.
5. Report only issues with evidence, or mark partial chains as `SUSPECTED`.

For `standard` and `deep` modes:
- Start from routes/controllers/handlers, jobs, message consumers, CLI commands, and file import/export entry points.
- Search for sinks and trace backward to user-controlled sources.
- Search for sources and trace forward to security-sensitive sinks.
- Inspect configs and dependencies for controls that change exploitability.
- Use checklists only to verify coverage after initial exploration; do not let checklists replace code reading.

## Deep Audit Additions

In `deep` mode:
- Track coverage across authentication, authorization, injection, file operations, SSRF/network egress, deserialization/parsing, secrets/crypto, dependency/config, business logic, and audit/logging dimensions.
- Build attack chains from individually bounded issues.
- Review tenant/ownership checks and state transitions, not only technical sinks.
- Use multiple passes: broad attack-surface map, focused taint/control-flow tracing, then cross-chain verification.
- If the runtime and user authorization permit parallel agents, split work by disjoint modules or dimensions and merge only evidence-backed findings.

## Deduplication Rules

Merge as one root-cause finding when:
- The same vulnerable function is called by multiple endpoints.
- The same unsafe parser, middleware, auth rule, or config affects multiple modules.
- The same missing tenant/ownership check is repeated through one shared service layer.
- The same sink is reached by multiple wrappers with identical mitigation gaps.

Always report both:
- Vulnerability instances: all affected endpoints/files/callers.
- Root-cause findings: deduplicated remediation units.

## Remediation Principles

- Stop active risk first (`P0`), then systemic hardening (`P1`/`P2`).
- Prefer parameterization over escaping for SQL/command/template injection.
- Prefer allowlists, canonicalization, and server-side policy checks over denylists.
- Enforce auth, authorization, ownership, and tenant checks server-side.
- Isolate upload storage and enforce non-executable paths.
- Disable dangerous parser features rather than filtering payload strings.
- Add regression tests for every fixed root cause and relevant bypass variants.

## Final Quality Gate

Before finishing:
- Verify every finding has all 8 required fields.
- Verify every finding cites real code locations that were read.
- Verify every `Critical`/`High` has at least `HIGH_CONFIDENCE`.
- Verify grep-only candidates are not reported as confirmed vulnerabilities.
- Verify duplicates are merged by root cause.
- Verify the report includes remediation priority and retest steps.
- If evidence is incomplete, say exactly what remains unverified.
