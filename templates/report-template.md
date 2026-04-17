# Security Audit Report Template

## 1) Executive Summary
- Target:
- Audit mode: Full / Quick
- In-scope components:
- Out-of-scope components:
- Vulnerability instances (raw count):
- Root-cause findings (deduplicated):
- Highest risk summary:

## 2) Attack Surface Inventory
- HTTP/API:
- RPC/MQ/Jobs:
- File import/export:
- Admin/internal interfaces:
- External dependencies and trust boundaries:

## 3) Findings

### [F-001] <Short title>
1. Location (file/class/method/API):
2. Type + root cause:
3. Source -> transfer -> sink evidence chain:
4. Exploit prerequisites:
5. Impact scope:
6. Severity + evidence label:
7. Minimal-fix remediation:
8. Retest checklist:

### [F-002] <Short title>
1. Location (file/class/method/API):
2. Type + root cause:
3. Source -> transfer -> sink evidence chain:
4. Exploit prerequisites:
5. Impact scope:
6. Severity + evidence label:
7. Minimal-fix remediation:
8. Retest checklist:

## 4) Deduplication Summary
- Root cause R1:
  - Instances:
  - Exposed endpoints/callers:
- Root cause R2:
  - Instances:
  - Exposed endpoints/callers:

## 5) Prioritized Remediation Plan
- P0 (immediate containment):
- P1 (short-term hardening):
- P2 (systemic improvements):

## 6) Retest Plan
- Verify sink protection is effective:
- Verify bypass variants are blocked:
- Verify tenant/auth boundaries are enforced:
- Verify logging/alerting for exploit attempts:

## 7) Notes
- Assumptions:
- Uncertain points / required runtime validation:
