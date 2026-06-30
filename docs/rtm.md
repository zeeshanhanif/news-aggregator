# Requirements Traceability Matrix: AI News Aggregator

> Source: `docs/srs.md`, `docs/use-cases.md` · Last updated: 2026-06-29
> Design and Test columns are placeholders, filled in downstream phases.
> Source key: **Interview** = stakeholder elicitation 2026-06-29; **Goal** = business
> goal (audience/AI insight); **Legal** = legal/copyright constraint; **Sec/Comp** =
> security/compliance driver.

## Functional requirements

| Req ID | Requirement (short) | Priority | Status | Source | Use case(s) | Design ref | Test ref |
| :----- | :------------------ | :------- | :----- | :----- | :---------- | :--------- | :------- |
| FR-INGEST-001 | Fetch RSS/Atom feeds on schedule | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-002 | Web-scrape feedless sources | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-003 | Respect robots.txt / crawl limits | Must | Active | Legal | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-004 | Ingest only free/public; skip paywalled | Must | Active | Legal | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-005 | Extract article metadata | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-006 | Detect language; flag non-English | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-007 | Raw-level dedup (URL/hash) | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-008 | Graceful fetch-failure handling/retry | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-009 | Per-source ingestion run history | Must | Active | Interview | UC-001, UC-016 | _TBD_ | _TBD_ |
| FR-INGEST-010 | Global configurable ingestion interval | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-011 | On-demand admin ingestion | Should | Active | Interview | UC-019 | _TBD_ | _TBD_ |
| FR-INGEST-012 | Discard non-article / too-short items | Should | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-013 | User-Agent + conditional requests | Should | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-INGEST-014 | Skip/log unextractable articles | Should | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| FR-SRC-001 | Add source | Must | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-002 | Edit source | Must | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-003 | List sources with status/health | Must | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-004 | Enable/disable source | Must | Active | Interview | UC-001, UC-014 | _TBD_ | _TBD_ |
| FR-SRC-005 | Soft-delete source, keep attribution | Must | Active | Legal | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-006 | Validate/preview source on save | Must | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-007 | Auto-extract for scrape sources | Must | Active | Interview | UC-001, UC-014 | _TBD_ | _TBD_ |
| FR-SRC-008 | View source ingestion history/errors | Must | Active | Interview | UC-014, UC-016 | _TBD_ | _TBD_ |
| FR-SRC-009 | Search/filter source list | Should | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-SRC-010 | Bulk-import OPML/CSV | Should | Active | Interview | UC-014 | _TBD_ | _TBD_ |
| FR-DEDUP-001 | Semantic same-story detection | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-002 | Translate before comparison | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-003 | Configurable similarity threshold | Should | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-004 | Configurable recency window | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-005 | Keep one source, discard duplicates | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-006 | Prefer English source | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-007 | Tiebreak by most complete text | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-008 | Discard duplicate of published story | Must | Active | Interview | UC-002 | _TBD_ | _TBD_ |
| FR-DEDUP-009 | Maintain coverage count | Must | Active | Interview | UC-002, UC-007 | _TBD_ | _TBD_ |
| FR-AI-001 | Translate to English | Must | Active | Interview | UC-002, UC-003 | _TBD_ | _TBD_ |
| FR-AI-002 | 2-line summary | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-003 | One-paragraph original rewrite | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-004 | Neutral analysis + sentiment/impact | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-005 | Classify into category | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-006 | Reuse source headline | Must | Active | Interview | UC-003, UC-008 | _TBD_ | _TBD_ |
| FR-AI-007 | Format/length guardrail | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-008 | Faithfulness guardrail | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-009 | Sensitive-content flagging | Must | Active | Interview | UC-003, UC-015 | _TBD_ | _TBD_ |
| FR-AI-010 | Record model/prompt version | Should | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-011 | Retry/hold on AI failure | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-AI-012 | Admin re-trigger AI processing | Should | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-AI-013 | Configurable AI-processing cap | Could | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| FR-PUB-001 | Story lifecycle status | Must | Active | Interview | UC-004 | _TBD_ | _TBD_ |
| FR-PUB-002 | Auto-publish clean stories | Must | Active | Interview | UC-004 | _TBD_ | _TBD_ |
| FR-PUB-003 | Route flagged to moderation queue | Must | Active | Interview | UC-004, UC-015 | _TBD_ | _TBD_ |
| FR-PUB-004 | View moderation queue | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-PUB-005 | Approve held story | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-PUB-006 | Reject story with reason | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-PUB-007 | Edit AI outputs | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-PUB-008 | Unpublish story | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-PUB-009 | Record publication metadata | Must | Active | Interview | UC-004, UC-015 | _TBD_ | _TBD_ |
| FR-PUB-010 | Block publish if AI output missing | Must | Active | Interview | UC-004 | _TBD_ | _TBD_ |
| FR-PUB-011 | Readers see only Published | Must | Active | Interview | UC-004, UC-006 | _TBD_ | _TBD_ |
| FR-PUB-012 | Auto-archive aged stories | Must | Active | Interview | UC-005 | _TBD_ | _TBD_ |
| FR-PUB-013 | Status-transition history | Should | Active | Interview | UC-004, UC-015 | _TBD_ | _TBD_ |
| FR-READ-001 | Newest-first home feed | Must | Active | Interview | UC-006 | _TBD_ | _TBD_ |
| FR-READ-002 | Browse by category | Must | Active | Interview | UC-006 | _TBD_ | _TBD_ |
| FR-READ-003 | Trending/most-covered view | Must | Active | Interview | UC-007 | _TBD_ | _TBD_ |
| FR-READ-004 | Story detail (AI content + attribution) | Must | Active | Interview | UC-008 | _TBD_ | _TBD_ |
| FR-READ-005 | Label AI-generated content | Must | Active | Legal | UC-008 | _TBD_ | _TBD_ |
| FR-READ-006 | Pagination/infinite scroll | Must | Active | Interview | UC-006 | _TBD_ | _TBD_ |
| FR-READ-007 | Empty/loading/error states | Must | Active | Interview | UC-006 | _TBD_ | _TBD_ |
| FR-READ-008 | Responsive rendering | Must | Active | Interview | UC-006, UC-008 | _TBD_ | _TBD_ |
| FR-READ-009 | Lead image display | Should | Active | Interview | UC-008 | _TBD_ | _TBD_ |
| FR-READ-010 | Sort newest/trending | Should | Active | Interview | UC-006 | _TBD_ | _TBD_ |
| FR-READ-011 | SEO essentials | Should | Active | Goal | — | _TBD_ | _TBD_ |
| FR-READ-012 | Visual sentiment badge | Should | Active | Interview | UC-008 | _TBD_ | _TBD_ |
| FR-READ-013 | Social-share / copy-link | Should | Active | Goal | UC-008 | _TBD_ | _TBD_ |
| FR-READ-014 | Related stories | Should | Active | Goal | UC-008 | _TBD_ | _TBD_ |
| FR-READ-015 | RSS/Atom output feed | Should | Active | Interview | UC-010 | _TBD_ | _TBD_ |
| FR-READ-016 | Third-party analytics integration | Should | Active | Goal | — | _TBD_ | _TBD_ |
| FR-SRCH-001 | Free-text keyword search | Must | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-002 | Relevance ranking | Must | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-003 | Filter by category/date | Must | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-004 | Sort relevance/newest | Should | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-005 | Paginate results | Must | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-006 | Empty-result handling | Must | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-007 | Include archived stories | Must | Active | Interview | UC-005, UC-009 | _TBD_ | _TBD_ |
| FR-SRCH-008 | Highlight matched terms | Should | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| FR-AUTH-001 | Admin sign-in | Must | Active | Interview | UC-011 | _TBD_ | _TBD_ |
| FR-AUTH-002 | Invite-only admin creation | Must | Active | Interview | UC-013 | _TBD_ | _TBD_ |
| FR-AUTH-003 | Invite acceptance + verify + set pw | Must | Active | Interview | UC-013 | _TBD_ | _TBD_ |
| FR-AUTH-004 | Logout | Must | Active | Interview | UC-011 | _TBD_ | _TBD_ |
| FR-AUTH-005 | Forgot/reset password | Must | Active | Interview | UC-012 | _TBD_ | _TBD_ |
| FR-AUTH-006 | Change password | Must | Active | Interview | — | _TBD_ | _TBD_ |
| FR-AUTH-007 | Password-strength policy | Must | Active | Sec/Comp | UC-012 | _TBD_ | _TBD_ |
| FR-AUTH-008 | Session expiry/refresh | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| FR-AUTH-009 | Account lockout | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| FR-AUTH-010 | Auth rate limiting | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| FR-AUTH-011 | Deactivate/reactivate admins | Must | Active | Interview | UC-013 | _TBD_ | _TBD_ |
| FR-AUTH-012 | Single Admin role; public no-auth | Must | Active | Interview | UC-011 | _TBD_ | _TBD_ |
| FR-ADMIN-001 | Admin dashboard stats | Must | Active | Interview | UC-016 | _TBD_ | _TBD_ |
| FR-ADMIN-002 | Ingestion/AI monitoring view | Must | Active | Interview | UC-016 | _TBD_ | _TBD_ |
| FR-ADMIN-003 | Story-management view | Must | Active | Interview | UC-015 | _TBD_ | _TBD_ |
| FR-ADMIN-004 | Category management | Must | Active | Interview | UC-017 | _TBD_ | _TBD_ |
| FR-ADMIN-005 | System settings UI | Must | Active | Interview | UC-018 | _TBD_ | _TBD_ |
| FR-ADMIN-006 | Admin user-management UI | Must | Active | Interview | UC-013 | _TBD_ | _TBD_ |
| FR-ADMIN-007 | Back-office search/pagination | Should | Active | Interview | UC-014, UC-015 | _TBD_ | _TBD_ |
| FR-NOTIF-001 | Alert on source/ingestion failures | Must | Active | Interview | UC-001, UC-016 | _TBD_ | _TBD_ |
| FR-NOTIF-002 | Alert on AI failures | Must | Active | Interview | UC-003, UC-016 | _TBD_ | _TBD_ |
| FR-NOTIF-003 | Alert on new moderation items | Must | Active | Interview | UC-004, UC-016 | _TBD_ | _TBD_ |
| FR-NOTIF-004 | Transactional auth emails | Must | Active | Interview | UC-012, UC-013 | _TBD_ | _TBD_ |
| FR-NOTIF-005 | Configurable alert thresholds | Should | Active | Interview | UC-018 | _TBD_ | _TBD_ |
| FR-AUDIT-001 | Log admin/data-changing actions | Must | Active | Sec/Comp | UC-013, UC-014, UC-015, UC-017, UC-018, UC-020 | _TBD_ | _TBD_ |
| FR-AUDIT-002 | Audit entry detail (who/what/when) | Must | Active | Sec/Comp | UC-020 | _TBD_ | _TBD_ |
| FR-AUDIT-003 | View/filter audit log | Must | Active | Sec/Comp | UC-020 | _TBD_ | _TBD_ |
| FR-AUDIT-004 | Append-only audit log | Must | Active | Sec/Comp | UC-020 | _TBD_ | _TBD_ |
| FR-AUDIT-005 | Configurable audit retention | Should | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| FR-LEGAL-001 | Source attribution + link | Must | Active | Legal | UC-008 | _TBD_ | _TBD_ |
| FR-LEGAL-002 | No full source-text reproduction | Must | Active | Legal | UC-008, UC-010 | _TBD_ | _TBD_ |
| FR-LEGAL-003 | AI-generated disclosure + About page | Must | Active | Legal | UC-008 | _TBD_ | _TBD_ |
| FR-LEGAL-004 | Terms & Privacy pages | Must | Active | Legal | — | _TBD_ | _TBD_ |
| FR-LEGAL-005 | Data-retention disclosure | Should | Active | Legal | — | _TBD_ | _TBD_ |

## External interface requirements

| Req ID | Requirement (short) | Priority | Status | Source | Use case(s) | Design ref | Test ref |
| :----- | :------------------ | :------- | :----- | :----- | :---------- | :--------- | :------- |
| EIR-UI-001 | Responsive reader web UI | Must | Active | Interview | UC-006, UC-008 | _TBD_ | _TBD_ |
| EIR-UI-002 | Admin back-office web UI | Must | Active | Interview | UC-011–UC-020 | _TBD_ | _TBD_ |
| EIR-SW-001 | LLM/AI service integration | Must | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| EIR-SW-002 | News-source feeds/scraping | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| EIR-SW-003 | Web-analytics integration | Should | Active | Goal | — | _TBD_ | _TBD_ |
| EIR-SW-004 | Email/SMTP service | Must | Active | Interview | UC-012, UC-013 | _TBD_ | _TBD_ |
| EIR-COM-001 | RSS/Atom output feed | Should | Active | Interview | UC-010 | _TBD_ | _TBD_ |
| EIR-COM-002 | HTTPS/TLS for all comms | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |

## Non-functional requirements

| Req ID | Requirement (short) | Priority | Status | Source | Use case(s) | Design ref | Test ref |
| :----- | :------------------ | :------- | :----- | :----- | :---------- | :--------- | :------- |
| NFR-PERF-001 | p75 LCP < 2.5s at target load | Must | Active | Goal | — | _TBD_ | _TBD_ |
| NFR-PERF-002 | p95 server response < 500ms @1k | Must | Active | Goal | — | _TBD_ | _TBD_ |
| NFR-PERF-003 | p95 search < 1s | Should | Active | Goal | UC-009 | _TBD_ | _TBD_ |
| NFR-PERF-004 | Ingestion cycle < 30 min | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| NFR-PERF-005 | AI per-story < 60s p95 | Should | Active | Interview | UC-003 | _TBD_ | _TBD_ |
| NFR-SCAL-001 | ≥ 200 sources | Must | Active | Interview | UC-001 | _TBD_ | _TBD_ |
| NFR-SCAL-002 | 1,000 concurrent readers | Must | Active | Goal | — | _TBD_ | _TBD_ |
| NFR-SCAL-003 | ≥ 12 months stories searchable | Should | Active | Interview | UC-009 | _TBD_ | _TBD_ |
| NFR-SCAL-004 | Horizontal scale to 2× | Should | Active | Goal | — | _TBD_ | _TBD_ |
| NFR-AVAIL-001 | ≥ 99.9% uptime | Must | Active | Goal | — | _TBD_ | _TBD_ |
| NFR-AVAIL-002 | Graceful degradation if pipeline down | Must | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-AVAIL-003 | Backups; RPO≤24h, RTO≤4h | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-AVAIL-004 | Pipeline fault isolation | Must | Active | Interview | UC-001, UC-003 | _TBD_ | _TBD_ |
| NFR-SEC-001 | TLS 1.2+ in transit | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-SEC-002 | Encryption at rest (AES-256) | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-SEC-003 | Adaptive password hashing; ≥12 chars | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| NFR-SEC-004 | OWASP Top 10 mitigation | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-SEC-005 | Secure/HttpOnly session cookies | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| NFR-SEC-006 | Endpoint rate limiting | Must | Active | Sec/Comp | UC-011 | _TBD_ | _TBD_ |
| NFR-SEC-007 | Secrets in secrets manager | Must | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-SEC-008 | Vuln scanning; patch ≤14d | Should | Active | Sec/Comp | — | _TBD_ | _TBD_ |
| NFR-USE-001 | Responsive 320px–desktop | Must | Active | Interview | UC-006, UC-008 | _TBD_ | _TBD_ |
| NFR-USE-002 | Best-effort accessibility (→WCAG 2.2 AA) | Should | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-USE-003 | Support latest 2 major browsers | Must | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-COMPAT-001 | Standards-conformant feed/OG tags | Should | Active | Goal | UC-010 | _TBD_ | _TBD_ |
| NFR-MAINT-001 | ≥70% core test coverage | Should | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-MAINT-002 | Settings configurable without deploy | Must | Active | Interview | UC-018 | _TBD_ | _TBD_ |
| NFR-MAINT-003 | Modular pipeline stages | Should | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-PORT-001 | Container-deployable to cloud | Should | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-COMP-001 | Honor robots.txt / source ToS | Must | Active | Legal | UC-001 | _TBD_ | _TBD_ |
| NFR-COMP-002 | Free/public content only | Must | Active | Legal | UC-001 | _TBD_ | _TBD_ |
| NFR-COMP-003 | Terms/Privacy; consent deferred | Should | Active | Legal | — | _TBD_ | _TBD_ |
| NFR-LOC-001 | English UI; translate foreign sources | Must | Active | Interview | UC-002, UC-003 | _TBD_ | _TBD_ |
| NFR-LOC-002 | Consistent labeled timestamps | Should | Active | Interview | UC-008 | _TBD_ | _TBD_ |
| NFR-OBS-001 | Structured logging across stages | Must | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-OBS-002 | Health-check endpoints | Must | Active | Interview | — | _TBD_ | _TBD_ |
| NFR-OBS-003 | Operational metrics in dashboard | Must | Active | Interview | UC-016 | _TBD_ | _TBD_ |
| NFR-OBS-004 | In-app pipeline alerts | Must | Active | Interview | UC-016 | _TBD_ | _TBD_ |
| NFR-OBS-005 | Error tracking for exceptions | Should | Active | Interview | — | _TBD_ | _TBD_ |

## Coverage notes

- **Requirements with no use case ("—")** are system-level or cross-cutting
  (most NFRs, SEO/analytics, Terms/Privacy pages, audit retention). This is
  expected and not an orphan defect.
- **Every functional requirement traces to at least one use case** except
  inherently non-interactive ones (FR-AUTH-006 change-password and FR-READ-011/016,
  FR-LEGAL-004/005, FR-AUDIT-005), which are static/config-level and verified
  directly.
- Design ref / Test ref columns are populated by the architecture and testing
  phases.
