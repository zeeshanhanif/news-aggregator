# Software Requirements Specification: AI News Aggregator

> Version: 1.0 · Status: Finalized · Last updated: 2026-06-29

## Revision History

| Version | Date | Author | Changes |
| :------ | :--- | :----- | :------ |
| 1.0 | 2026-06-29 | Zeeshan | Initial finalized specification — 117 functional, 8 external-interface, 39 non-functional requirements across 12 capability areas. |

## 1. Introduction

### 1.1 Purpose
This document specifies the complete functional and non-functional requirements
for the **AI News Aggregator** (working title), a web-based system that
automatically collects news from a curated set of free, publicly available
sources, removes duplicate coverage of the same story, and uses AI to generate a
concise summary, an original rewrite, and an analytical take on each story,
presented on a public responsive web app.

It is the single source of truth for downstream architecture, UX, and
implementation-planning activities. The intended audience is the product owner,
engineering, design, QA, and any stakeholders reviewing scope.

### 1.2 Product vision & scope

**Problem.** Readers face fragmented, repetitive, and source-biased news
coverage. The same story is reported by many outlets, forcing readers to wade
through duplicates and read multiple articles to form a view.

**Vision.** A web app that continuously ingests news from pre-specified free,
public sources (via RSS/Atom feeds, with web-scraping fallback for feedless
sources), detects when multiple sources cover the same story and keeps a single
source's version (preferring English; duplicates are discarded but counted), and
for each unique story produces:
- a **2-line summary**,
- an **original one-paragraph rewrite** in the system's own words, and
- an **analytical take** (editorial opinion/commentary plus sentiment & impact).

Foreign-language sources are translated to English. Stories are organized by
category, ranked by how widely they are covered ("trending / most-covered"), and
made searchable. Each story attributes and links out to its original source; the
system does not republish full source article text.

**In scope (MVP)**
- Automated ingestion from pre-configured RSS/Atom feeds and web-scraping
  fallback, restricted to free, publicly available content.
- Cross-source deduplication / clustering of the same story.
- AI processing per story: translation (to English) of foreign sources, 2-line
  summary, one-paragraph original rewrite, category classification, and analysis
  (opinion/commentary + sentiment & impact).
- Public responsive web reading experience with **no end-user login**: category
  browsing, trending/most-covered ranking, free-text search, story detail with
  source attribution and link-out.
- Admin back-office: secure admin authentication, source management,
  ingestion-health monitoring, AI-output review/moderation, and category
  management.
- Audit logging of administrative actions; operational alerting on ingestion
  failures.

**Out of scope (MVP)**
- End-user accounts, personalization, follows, bookmarks, comments, social
  features.
- Native mobile apps; public/developer API.
- Paywalled, licensed, or otherwise non-free content; full reproduction of
  source article text.
- Multi-language **reader** output (reader-facing content is English only;
  multi-language applies only to *ingestion* of foreign sources, which are
  translated to English).
- Monetization (ads/sponsorship) — assumed deferred (see Open Questions).

### 1.3 Goals & success metrics
The MVP pursues two complementary goals — **audience/traffic growth** and
**differentiated AI insight**.

| Goal | Success metric (target to be confirmed) |
| :--- | :--- |
| Audience / traffic | Monthly unique visitors; return-visit rate; average time-on-site; pages per session. |
| Differentiated AI insight | Perceived value of the AI summary/rewrite/analysis; low AI-output correction/rejection rate in moderation; reader engagement with the analysis section. |
| Coverage & freshness (enabling) | Ingestion-to-publish latency; deduplication accuracy (precision/recall of story clustering). |

*(Numeric targets are specified as measurable NFRs in §3.3 and confirmed during
Phase 3.)*

### 1.4 Definitions, acronyms & abbreviations
See **Appendix A — Glossary**.

### 1.5 References
- ISO/IEC/IEEE 29148 (Requirements engineering).
- Source outlet RSS/Atom feeds and websites (pre-specified list — TBD).
- LLM provider documentation (AI summarization/translation/analysis).

## 2. Overall Description

### 2.1 Product perspective
The system is a new, self-contained web application with two faces: a **public
reader web app** and an **admin back-office**. Behind them runs an automated
**ingestion-and-AI pipeline**. External systems it depends on:
- **News sources** — third-party websites exposing RSS/Atom feeds or scrapeable
  HTML (free, public).
- **LLM / AI service** — for translation, summarization, rewriting,
  classification, and analysis.
- (Optional/operational) email or messaging service for admin alerts.

### 2.2 Product functions (summary)
- Content ingestion & source acquisition (feeds + scraping).
- Source management (admin).
- Deduplication & story clustering.
- AI content processing (translate, summarize, rewrite, classify, analyze).
- Content publishing, moderation & lifecycle.
- Reader experience (browse, categories, trending, story detail).
- Search.
- Admin authentication & account management.
- Admin back-office (dashboards, ingestion monitoring, category management).
- Notifications & alerting (operational).
- Audit logging.
- Legal / attribution / compliance pages.

### 2.3 User classes & characteristics
| User class | Description & needs |
| :--- | :--- |
| **Anonymous Reader** | General public. Browses, reads, searches; no account. Wants fast, de-duplicated, trustworthy, easy-to-scan news with a clear AI take. Varied devices (mobile/desktop), varied technical skill. |
| **Administrator / Content Operator** | Internal staff. Manages the source list, monitors ingestion health, reviews and moderates AI-generated output before/after publish, manages categories. Needs efficient back-office tools and reliable operational visibility. |
| **Automated Ingestion & AI Pipeline** *(system actor)* | Non-human actor that fetches, deduplicates, processes, and publishes stories on a schedule. Modeled as an actor in use cases; not a user with a login. |

### 2.4 Operating environment
Responsive web application accessed via modern desktop and mobile browsers.
Server-side ingestion pipeline runs on a schedule. Depends on outbound internet
access to sources and the LLM service. (Detailed hosting/infra is a downstream
architecture decision.)

### 2.5 Constraints
- **Free, public content only** — no paywalled, licensed, or subscription
  content may be ingested.
- **No full-text republication** — reader pages show AI-generated content plus
  attribution and a link to the original; they must not reproduce full source
  article text.
- **Respect source terms** — scraping must honor robots.txt and source terms of
  service.
- **Responsive web only** for MVP (no native apps).
- **AI dependency** — core value relies on an external LLM service; output must
  pass moderation before reader exposure (per Publishing area).

### 2.6 Assumptions & dependencies
- A list of free, public sources is provided/approved by stakeholders.
- Targeted sources permit automated access (feeds/scraping) under their terms.
- An LLM service with sufficient quality and availability is accessible.
- Foreign-language ingestion is limited to languages the chosen LLM can reliably
  translate.

## 3. Specific Requirements

### 3.1 Functional requirements
*(Filled area-by-area during Phase 2. Each requirement: unique ID, testable
"shall" statement, priority, status, supporting rules.)*

#### 3.1.1 Content Ingestion & Source Acquisition

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-INGEST-001 | The system shall fetch articles from each configured RSS/Atom feed source on a recurring schedule. | Must | Active | Feed list managed in Source Management (§3.1.2). |
| FR-INGEST-002 | The system shall acquire articles by web scraping for configured sources that do not provide an RSS/Atom feed. | Must | Active | HTML extraction of title, body, date, author, image. |
| FR-INGEST-003 | The system shall respect each source's robots.txt directives and configured crawl-rate limits when scraping. | Must | Active | Legal constraint (§2.5). |
| FR-INGEST-004 | The system shall ingest only free, publicly accessible content and shall skip any article that is paywalled or requires authentication. | Must | Active | Skip entirely — not even snippet-ingested. |
| FR-INGEST-005 | The system shall extract and store, for each ingested article, its title, source URL, publication date/time, author (if available), source identifier, body text, detected language, and lead image (if available). | Must | Active | Missing optional fields stored as null. |
| FR-INGEST-006 | The system shall detect the language of each ingested article and flag non-English articles for downstream translation. | Must | Active | Feeds AI translation (§3.1.4). |
| FR-INGEST-007 | The system shall skip articles already ingested, matched by canonical URL or content hash, to avoid re-processing the same raw article. | Must | Active | Raw-level dedup; distinct from cross-source clustering (§3.1.3). |
| FR-INGEST-008 | The system shall handle fetch failures (timeouts, HTTP errors, malformed feeds) without aborting the overall ingestion run, applying retry with backoff and recording each failure. | Must | Active | Failure detail surfaced to monitoring (§3.1.9). |
| FR-INGEST-009 | The system shall record an ingestion run history per source capturing items fetched, newly added, skipped, and errored, with timestamps. | Must | Active | Source of ingestion-health dashboard. |
| FR-INGEST-010 | The system shall run ingestion automatically on a single system-wide, administrator-configurable interval applied to all sources. | Must | Active | Default ≈ every 2–3 hours; one global interval, not per-source. |
| FR-INGEST-011 | The system shall allow an administrator to trigger an on-demand ingestion run for all sources or a selected source. | Should | Active | Manual refresh from back-office. |
| FR-INGEST-012 | The system shall discard fetched items that are not valid news articles, including items below a configurable minimum length and non-article pages (index/section pages). | Should | Active | Minimum length configurable. |
| FR-INGEST-013 | The system shall identify itself with a configured User-Agent and use conditional requests (ETag / Last-Modified) where supported to avoid redundant fetching. | Should | Active | Source etiquette / efficiency. |
| FR-INGEST-014 | The system shall skip and log any article whose full free text cannot be reliably extracted, excluding it from downstream processing. | Should | Active | Prevents low-quality input to the AI pipeline. |

#### 3.1.2 Source Management

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-SRC-001 | The system shall allow an administrator to add a news source specifying its name, URL, type (RSS/Atom feed or web-scrape), default category, and source language. | Must | Active | Source language seeds translation handling. |
| FR-SRC-002 | The system shall allow an administrator to edit an existing source's configuration. | Must | Active | |
| FR-SRC-003 | The system shall display a list of all sources with, for each, its enabled/disabled state, last-fetched time, and a health indicator. | Must | Active | Health derived from recent ingestion runs (§3.1.1). |
| FR-SRC-004 | The system shall allow an administrator to enable or disable a source without deleting it. | Must | Active | Disabled sources are excluded from ingestion. |
| FR-SRC-005 | The system shall allow an administrator to remove a source via soft-delete, retaining attribution on stories previously published from it. | Must | Active | No hard delete; preserves historical traceability. |
| FR-SRC-006 | The system shall validate a source when it is added or edited by probing its feed/URL and presenting a sample-fetch preview before saving. | Must | Active | Prevents misconfigured sources. |
| FR-SRC-007 | The system shall extract article content from scrape-type sources using automatic article extraction, without requiring per-source selectors. | Must | Active | Decision: auto-extract only. |
| FR-SRC-008 | The system shall let an administrator view a source's ingestion history and error log. | Must | Active | |
| FR-SRC-009 | The system shall allow an administrator to search and filter the source list by name, type, and status. | Should | Active | |
| FR-SRC-010 | The system shall allow an administrator to bulk-import sources from an OPML or CSV file, validating each entry and reporting per-row errors. | Should | Active | Failed rows reported; valid rows imported. |

#### 3.1.3 Deduplication & Single-Source Selection

> Model: the system does **not** merge multiple sources into a multi-source
> story. It detects duplicate coverage, keeps exactly **one** source's article
> per story, discards the rest, and tracks a coverage count for ranking. There is
> no manual merge/split override in the MVP (fully automated).

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-DEDUP-001 | The system shall compare each newly ingested article against existing stories, using semantic similarity of title and body, to determine whether it reports the same real-world story. | Must | Active | Comparison scoped by the recency window (FR-DEDUP-004). |
| FR-DEDUP-002 | The system shall translate non-English articles to English before similarity comparison so that cross-language duplicates are detected. | Must | Active | Uses AI translation (§3.1.4). |
| FR-DEDUP-003 | The system shall use a configurable similarity threshold to decide whether two articles report the same story. | Should | Active | Tunable to balance false merges vs. misses. |
| FR-DEDUP-004 | The system shall apply a configurable recency/time window outside which two articles are not treated as the same story. | Must | Active | Prevents merging unrelated stories on a recurring topic. |
| FR-DEDUP-005 | For each story the system shall retain exactly one source's article and discard all other articles found to be duplicates of it. | Must | Active | Single-source model; no multi-source merge. |
| FR-DEDUP-006 | When selecting which source to keep for a story, the system shall prefer an English-language source over a non-English source. | Must | Active | English preference. |
| FR-DEDUP-007 | When candidate sources for a story are equal under the language preference, the system shall keep the one with the most complete extracted article text. | Must | Active | Tiebreak by content completeness. |
| FR-DEDUP-008 | When a newly ingested article is found to duplicate an already-published story, the system shall discard the new article's content and leave the published story's AI content unchanged. | Must | Active | Published content is stable; see FR-DEDUP-009 for the count. |
| FR-DEDUP-009 | The system shall maintain a per-story coverage count reflecting the number of distinct sources observed reporting that story, incremented each time a duplicate is detected and discarded. | Must | Active | Drives "trending / most-covered" ranking (§3.1.6); count is not shown as duplicate content. |

#### 3.1.4 AI Content Processing

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-AI-001 | The system shall translate non-English article text to English before generating any reader-facing output. | Must | Active | Shared with dedup comparison (§3.1.3). |
| FR-AI-002 | The system shall generate a 2-line summary of each story. | Must | Active | Enforced length (≈2 lines / configurable char cap); English. |
| FR-AI-003 | The system shall generate an original one-paragraph rewrite of each story in the system's own words. | Must | Active | One paragraph; must not reproduce source text verbatim. |
| FR-AI-004 | The system shall generate an analytical take for each story comprising a neutral analytical commentary plus a sentiment and impact assessment. | Must | Active | Neutral stance; labeled AI-generated (§3.1.12). |
| FR-AI-005 | The system shall classify each story into exactly one category from the configured category taxonomy. | Must | Active | Taxonomy managed in §3.1.9. |
| FR-AI-006 | The system shall use the original source headline as the story's displayed title and shall not generate an alternative headline. | Must | Active | Decision: reuse source headline. |
| FR-AI-007 | The system shall enforce output constraints — 2-line summary, one-paragraph rewrite, English language — and shall reject and regenerate any output that violates them. | Must | Active | Format/length guardrail. |
| FR-AI-008 | The system shall verify that generated outputs are faithful to the source article and introduce no facts unsupported by it, regenerating or withholding outputs that fail. | Must | Active | Faithfulness guardrail. |
| FR-AI-009 | The system shall detect graphic, hateful, or otherwise unsafe content and flag the affected story for admin review instead of auto-publishing it. | Must | Active | Routes to moderation queue (§3.1.5). |
| FR-AI-010 | The system shall record, with each story's AI outputs, the AI model/version and prompt version used to produce them. | Should | Active | Traceability and reproducibility. |
| FR-AI-011 | The system shall retry AI processing with backoff on transient AI-service failures and, if processing ultimately fails, hold the story in an error state surfaced to monitoring rather than publishing incomplete content. | Must | Active | No partial publication. |
| FR-AI-012 | The system shall allow an administrator to manually re-trigger AI processing for a specific story. | Should | Active | Supports recovery after moderation rejection or errors. |
| FR-AI-013 | The system shall support a configurable cap on AI-processing volume per run/period, deferring stories beyond the cap to a later run. | Could | Active | Operational cost control. |

#### 3.1.5 Content Publishing, Moderation & Lifecycle

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-PUB-001 | The system shall maintain a lifecycle status for each story (e.g., Ingested, Processing, Processed, Published, Held-for-review, Rejected, Archived). | Must | Active | Drives all downstream visibility rules. |
| FR-PUB-002 | The system shall automatically publish a processed story that passes all AI guardrails. | Must | Active | Guardrails per FR-AI-007/008/009. |
| FR-PUB-003 | The system shall route any story flagged or failed by a guardrail (sensitive content, faithfulness, format) to a moderation queue in Held-for-review status instead of publishing it. | Must | Active | Flag reason retained. |
| FR-PUB-004 | The system shall let an administrator view the moderation queue of held stories, showing each story's flag reason. | Must | Active | |
| FR-PUB-005 | The system shall let an administrator approve a held story, transitioning it to Published. | Must | Active | |
| FR-PUB-006 | The system shall let an administrator reject a story with a recorded reason, keeping it out of reader view. | Must | Active | |
| FR-PUB-007 | The system shall let an administrator edit a story's AI-generated outputs (summary, rewrite, analysis, category) before or after publishing. | Must | Active | Edited content replaces AI output; change logged (§3.1.11). |
| FR-PUB-008 | The system shall let an administrator unpublish (take down) an already-published story. | Must | Active | Transitions to a non-public status. |
| FR-PUB-009 | The system shall record publication metadata for each published story: the published-at timestamp and, when manually approved, the approving administrator. | Must | Active | |
| FR-PUB-010 | The system shall prevent publication of any story missing a required AI output (summary, rewrite, analysis, or category). | Must | Active | Completeness gate. |
| FR-PUB-011 | The system shall expose only stories in Published status to readers. | Must | Active | Held/Rejected/Archived never appear in the live feed. |
| FR-PUB-012 | The system shall automatically archive published stories older than a configurable retention window, removing them from the main feed while keeping them stored and searchable. | Must | Active | Configurable window (e.g., 30/90 days). |
| FR-PUB-013 | The system shall record the status-transition history (actor and timestamp) for each story. | Should | Active | Feeds audit log (§3.1.11). |

#### 3.1.6 Reader Experience

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-READ-001 | The system shall present a home feed of published stories ordered newest-first by default. | Must | Active | Published-only (FR-PUB-011). |
| FR-READ-002 | The system shall let a reader browse stories filtered by category. | Must | Active | Categories per §3.1.9. |
| FR-READ-003 | The system shall provide a trending / most-covered view ranking stories by coverage count. | Must | Active | Coverage count from FR-DEDUP-009. |
| FR-READ-004 | The system shall provide a story detail page showing the source headline, 2-line summary, one-paragraph rewrite, analysis (commentary plus sentiment & impact), category, publish time, and source attribution with a link to the original article. | Must | Active | No full source text reproduced (§3.1.12). |
| FR-READ-005 | The system shall visibly label AI-generated content (summary, rewrite, analysis) as AI-generated. | Must | Active | Transparency; see §3.1.12. |
| FR-READ-006 | The system shall paginate or infinitely scroll all story lists. | Must | Active | |
| FR-READ-007 | The system shall provide empty-state, loading-state, and error-state handling on all reader views. | Must | Active | |
| FR-READ-008 | The system shall render responsively across supported mobile and desktop browsers. | Must | Active | Targets per NFR-USE/NFR-COMPAT. |
| FR-READ-009 | The system shall display a story's lead image when one is available. | Should | Active | |
| FR-READ-010 | The system shall let a reader sort story lists by newest or by trending. | Should | Active | |
| FR-READ-011 | The system shall provide SEO essentials: human-readable URLs, per-story meta/OpenGraph tags, and a sitemap. | Should | Active | Supports the audience/traffic goal. |
| FR-READ-012 | The system shall display the AI sentiment as a visual badge/indicator on stories. | Should | Active | |
| FR-READ-013 | The system shall provide social-share and copy-link actions on each story. | Should | Active | |
| FR-READ-014 | The system shall show related stories from the same category on the story detail page. | Should | Active | Increases pages/session. |
| FR-READ-015 | The system shall publish an RSS/Atom output feed of published stories for readers to subscribe to. | Should | Active | Feed carries AI summary + link, not full source text. |
| FR-READ-016 | The system shall integrate a third-party web analytics tool to measure reader traffic and engagement. | Should | Active | E.g., GA/Plausible; cookie/consent per §3.1.12. |

#### 3.1.7 Search

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-SRCH-001 | The system shall provide free-text keyword search across published stories' headline, summary, rewrite, and analysis text. | Must | Active | Reader-facing AI content only. |
| FR-SRCH-002 | The system shall rank search results by relevance to the query. | Must | Active | |
| FR-SRCH-003 | The system shall allow filtering search results by category and by date range. | Must | Active | |
| FR-SRCH-004 | The system shall allow sorting search results by relevance or by newest. | Should | Active | |
| FR-SRCH-005 | The system shall paginate search results. | Must | Active | |
| FR-SRCH-006 | The system shall provide clear empty-result handling with guidance when no stories match a query. | Must | Active | |
| FR-SRCH-007 | The system shall include archived stories in search results. | Must | Active | Consistent with FR-PUB-012. |
| FR-SRCH-008 | The system shall highlight matched query terms within search results. | Should | Active | |

#### 3.1.8 Admin Authentication & Account Management

> Authorization model (MVP): a single **Admin** role with full back-office
> access; the public reader site requires no authentication. No public
> self-registration exists.

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-AUTH-001 | The system shall allow an administrator to sign in with email and password. | Must | Active | Email + password only (no SSO in MVP). |
| FR-AUTH-002 | The system shall restrict admin account creation to invitation by an existing administrator and shall not provide public self-registration. | Must | Active | |
| FR-AUTH-003 | The system shall require an invited administrator to verify their email and set an initial password before the account is activated. | Must | Active | Invite link time-limited. |
| FR-AUTH-004 | The system shall allow an administrator to log out, ending the active session. | Must | Active | |
| FR-AUTH-005 | The system shall provide forgot-password and password-reset via a time-limited emailed link. | Must | Active | Link expiry configurable (e.g., 1h). |
| FR-AUTH-006 | The system shall allow an authenticated administrator to change their password. | Must | Active | Requires current password. |
| FR-AUTH-007 | The system shall enforce a configurable password-strength policy. | Must | Active | Policy per NFR-SEC. |
| FR-AUTH-008 | The system shall expire admin sessions after a configurable period of inactivity and support session refresh. | Must | Active | |
| FR-AUTH-009 | The system shall lock an admin account after a configurable number of consecutive failed login attempts. | Must | Active | Unlock via reset/admin action. |
| FR-AUTH-010 | The system shall rate-limit authentication endpoints to mitigate brute-force attacks. | Must | Active | Per NFR-SEC. |
| FR-AUTH-011 | The system shall allow an administrator to deactivate and reactivate admin accounts without hard-deleting them. | Must | Active | Preserves audit attribution. |
| FR-AUTH-012 | The system shall grant all administrators a single Admin role with full back-office access and shall require no authentication for the public reader site. | Must | Active | Authorization model for MVP. |

#### 3.1.9 Admin Back-office

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-ADMIN-001 | The system shall provide an admin dashboard showing key operational statistics: stories published today, moderation-queue size, ingestion-health summary, and recent error counts. | Must | Active | |
| FR-ADMIN-002 | The system shall provide an ingestion-and-AI monitoring view showing each source's last run status, item counts, and errors, plus a list of stories whose AI processing failed. | Must | Active | Backed by FR-INGEST-009, FR-AI-011. |
| FR-ADMIN-003 | The system shall provide a story-management view to list and search all stories across every status and open a story to view, moderate, or edit it. | Must | Active | Entry point to FR-PUB actions. |
| FR-ADMIN-004 | The system shall allow an administrator to create, rename, and enable/disable categories used for AI classification and reader browsing. | Must | Active | Disabling hides a category from readers but retains existing assignments. |
| FR-ADMIN-005 | The system shall provide an admin settings UI to view and update tunable system settings, including the global ingestion interval, retention window, similarity threshold, minimum article length, AI-processing cap, password policy, and session timeout. | Must | Active | Changes audited (§3.1.11); effective from next run. |
| FR-ADMIN-006 | The system shall provide an admin user-management UI to invite, list, and deactivate/reactivate administrators. | Must | Active | Surfaces §3.1.8. |
| FR-ADMIN-007 | The system shall allow search/filter and pagination of story and source lists within the back-office. | Should | Active | |

#### 3.1.10 Notifications & Alerting (operational)

> MVP delivers operational alerts **in-app only**; an email channel is planned for
> a later release. Transactional auth emails (invite, reset) are still in scope.

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-NOTIF-001 | The system shall raise an in-app admin alert when a source fails repeatedly or an ingestion run's errors exceed a configurable threshold. | Must | Active | Thresholds per settings (§3.1.9). |
| FR-NOTIF-002 | The system shall raise an in-app admin alert when the AI-processing failure rate exceeds a configurable threshold or the AI service is unavailable. | Must | Active | |
| FR-NOTIF-003 | The system shall surface an in-app indicator when new stories enter the moderation queue. | Must | Active | Ties to FR-PUB-003. |
| FR-NOTIF-004 | The system shall send the transactional emails required by admin authentication (account invitation and password reset). | Must | Active | Only auth transactional email in MVP; operational alerts remain in-app. |
| FR-NOTIF-005 | The system shall allow configuration of alert thresholds. | Should | Active | |

#### 3.1.11 Audit Logging

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-AUDIT-001 | The system shall record an audit-log entry for security-relevant and data-changing admin actions, including login/logout, source create/edit/disable/delete, story publish/reject/edit/unpublish, settings changes, and admin user management. | Must | Active | |
| FR-AUDIT-002 | Each audit entry shall capture the acting administrator, action type, affected entity, timestamp, and before/after values where applicable. | Must | Active | |
| FR-AUDIT-003 | The system shall provide an admin-viewable audit log with filtering/search by actor, action, entity, and date range. | Must | Active | |
| FR-AUDIT-004 | The system shall store audit entries append-only; they shall not be editable or deletable through the application. | Must | Active | Integrity. |
| FR-AUDIT-005 | The system shall retain audit entries for a configurable retention period. | Should | Active | |

#### 3.1.12 Legal, Attribution & Compliance

> Privacy posture (MVP): basic Terms & Privacy pages only; formal cookie-consent
> handling is deferred (see Open Questions — compliance risk). No takedown
> mechanism in MVP; source removal uses general admin capabilities (FR-SRC-005,
> FR-PUB-008).

| ID | Requirement | Priority | Status | Notes / rules |
| :-- | :---------- | :------- | :----- | :------------ |
| FR-LEGAL-001 | The system shall display source attribution and a link to the original article on every story. | Must | Active | Reinforces FR-READ-004. |
| FR-LEGAL-002 | The system shall not reproduce full source article text anywhere in the product. | Must | Active | Core copyright constraint (§2.5). |
| FR-LEGAL-003 | The system shall clearly disclose that summaries, rewrites, and analyses are AI-generated, both inline and via an About/Methodology page. | Must | Active | Inline label per FR-READ-005. |
| FR-LEGAL-004 | The system shall provide Terms of Service and Privacy Policy pages. | Must | Active | |
| FR-LEGAL-005 | The system shall disclose its data-retention behavior in the Privacy Policy. | Should | Active | Aligns with FR-PUB-012, FR-AUDIT-005. |

<!-- END OF FUNCTIONAL AREAS (3.1). Next: 3.2 External Interfaces, 3.3 NFRs. -->

### 3.2 External interface requirements

**User interfaces (requirements level).** Two web UIs: a public responsive
reader site (no login) and an authenticated admin back-office. UI must satisfy
NFR-USE-001/003. Detailed UX is a downstream concern.

| ID | Interface | Requirement | Priority |
| :-- | :-------- | :---------- | :------- |
| EIR-UI-001 | Reader web UI | Responsive HTML site supporting the reader functions of §3.1.6–3.1.7 across supported browsers. | Must |
| EIR-UI-002 | Admin web UI | Authenticated back-office supporting §3.1.2, 3.1.5, 3.1.8–3.1.11. | Must |
| EIR-SW-001 | LLM / AI service | The system shall integrate an external LLM service for translation, summarization, rewriting, classification, and analysis, over an authenticated API, with configurable model selection and graceful handling of unavailability. | Must |
| EIR-SW-002 | News sources | The system shall consume RSS/Atom feeds and scrape HTML from configured source sites over HTTP(S). | Must |
| EIR-SW-003 | Web analytics | The system shall embed a third-party web-analytics service on the reader site (FR-READ-016). | Should |
| EIR-SW-004 | Email service | The system shall send transactional admin emails via an external email/SMTP service (FR-NOTIF-004). | Must |
| EIR-COM-001 | Output feed | The system shall expose an RSS/Atom output feed of published stories (FR-READ-015) conforming to the relevant feed standard. | Should |
| EIR-COM-002 | Network | All external communication shall occur over HTTPS/TLS (NFR-SEC-001). | Must |

### 3.3 Non-functional requirements

Targets assume **Medium** scale: ~200 sources; tens of thousands of stories per
month; up to **1,000 concurrent readers**; ~500k monthly visitors.

| ID | Category | Requirement (metric, target, condition) | Priority |
| :-- | :------- | :-------------------------------------- | :------- |
| NFR-PERF-001 | Performance | Reader pages shall achieve p75 Largest Contentful Paint < 2.5 s at target load (1,000 concurrent readers). | Must |
| NFR-PERF-002 | Performance | 95% of reader page/API requests shall complete server-side within 500 ms at 1,000 concurrent readers. | Must |
| NFR-PERF-003 | Performance | 95% of search queries shall return within 1 s at target load. | Should |
| NFR-PERF-004 | Performance | A full ingestion cycle across all configured sources shall complete within 30 minutes so that cycles do not overlap at the default interval. | Must |
| NFR-PERF-005 | Performance | AI processing of a single story shall complete within 60 s (p95), excluding queue wait. | Should |
| NFR-SCAL-001 | Scalability | The system shall support at least 200 configured sources and ingest their resulting article volume each cycle. | Must |
| NFR-SCAL-002 | Scalability | The system shall sustain 1,000 concurrent readers without breaching performance NFRs. | Must |
| NFR-SCAL-003 | Scalability | The system shall store and serve at least 12 months of stories while maintaining search performance (NFR-PERF-003). | Should |
| NFR-SCAL-004 | Scalability | The architecture shall scale horizontally to 2× target load without redesign. | Should |
| NFR-AVAIL-001 | Availability | The public reader site shall maintain ≥ 99.9% uptime measured monthly. | Must |
| NFR-AVAIL-002 | Reliability | The reader site shall continue serving last-published content when the ingestion/AI pipeline is degraded or down (graceful degradation). | Must |
| NFR-AVAIL-003 | Recoverability | Automated backups shall run at least daily, with RPO ≤ 24 h and RTO ≤ 4 h. | Must |
| NFR-AVAIL-004 | Reliability | Transient source/AI failures shall not abort the pipeline; failed items shall be retried and isolated (per FR-INGEST-008, FR-AI-011). | Must |
| NFR-SEC-001 | Security | All network traffic shall use TLS 1.2 or higher. | Must |
| NFR-SEC-002 | Security | Credentials and sensitive data at rest shall be encrypted (AES-256 or equivalent). | Must |
| NFR-SEC-003 | Security | Admin passwords shall be stored using a strong adaptive hash (bcrypt/argon2); password policy shall require ≥ 12 characters with complexity. | Must |
| NFR-SEC-004 | Security | The application shall mitigate the OWASP Top 10 (injection, XSS, CSRF, broken access control, etc.). | Must |
| NFR-SEC-005 | Security | Admin session cookies shall be Secure and HttpOnly and expire per FR-AUTH-008. | Must |
| NFR-SEC-006 | Security | Authentication and public endpoints shall be rate-limited to mitigate brute-force and abuse. | Must |
| NFR-SEC-007 | Security | Secrets (AI/API/email keys) shall be held in a secrets manager or environment configuration, never in source code. | Must |
| NFR-SEC-008 | Security | Dependencies shall be vulnerability-scanned; critical patches applied within 14 days. | Should |
| NFR-USE-001 | Usability | The UI shall render responsively across viewport widths from 320 px to large desktop. | Must |
| NFR-USE-002 | Accessibility | The site shall apply best-effort accessibility (semantic HTML, keyboard navigability, image alt text, sufficient contrast), targeting WCAG 2.2 AA without formal certification in MVP. | Should |
| NFR-USE-003 | Compatibility | The site shall support the latest two stable versions of Chrome, Safari, Firefox, and Edge on desktop and mobile. | Must |
| NFR-COMPAT-001 | Compatibility | The output feed (FR-READ-015) and OpenGraph/meta tags shall conform to their respective standards. | Should |
| NFR-MAINT-001 | Maintainability | Core pipeline logic (ingest, dedup, AI, publish) shall have ≥ 70% automated test coverage. | Should |
| NFR-MAINT-002 | Maintainability | Tunable parameters (FR-ADMIN-005) shall be configurable without a code deployment. | Must |
| NFR-MAINT-003 | Maintainability | Pipeline stages shall be modular so any stage can be changed independently. | Should |
| NFR-PORT-001 | Portability | The system shall be deployable via containers to a standard cloud environment. | Should |
| NFR-COMP-001 | Compliance | Scraping shall honor robots.txt and per-source terms of service (FR-INGEST-003). | Must |
| NFR-COMP-002 | Compliance | The system shall ingest only free, publicly available content; no paywalled/licensed content. | Must |
| NFR-COMP-003 | Compliance | Terms and Privacy pages shall be published; formal cookie-consent handling is deferred (documented risk, Appendix B). | Should |
| NFR-LOC-001 | Localization | Reader UI and content shall be English; non-English sources shall be ingested and translated to English. | Must |
| NFR-LOC-002 | Localization | Story timestamps shall be displayed in a consistent, clearly labeled time zone. | Should |
| NFR-OBS-001 | Observability | The system shall emit structured logs across ingestion, dedup, AI, and publishing stages. | Must |
| NFR-OBS-002 | Observability | The application and pipeline shall expose health-check endpoints. | Must |
| NFR-OBS-003 | Observability | Operational metrics (ingestion runs, dedup rate, AI success/failure, publish counts) shall be captured and surfaced in the admin dashboard (FR-ADMIN-001). | Must |
| NFR-OBS-004 | Observability | Pipeline failures shall raise in-app alerts (per FR-NOTIF-001/002). | Must |
| NFR-OBS-005 | Observability | Unhandled exceptions shall be captured by an error-tracking mechanism. | Should |

### 3.4 Other requirements
Data retention is governed by FR-PUB-012 (story archival), FR-AUDIT-005 (audit
retention), and disclosed per FR-LEGAL-005. Compliance and localization
requirements are specified as NFR-COMP-* and NFR-LOC-* above. No payment, PCI, or
health-data (PCI-DSS/HIPAA) requirements apply to the MVP.

## Appendix A — Glossary
| Term | Definition |
| :--- | :--- |
| Story | A single real-world news event, represented once by one chosen source's article. Duplicate coverage from other sources is discarded but counted. |
| Source | A configured news outlet, accessed via RSS/Atom feed or web scraping. |
| Ingestion | The automated process of fetching raw articles from sources. |
| Deduplication | Detecting that an incoming article reports a story already captured, then discarding the duplicate and keeping a single source's version. |
| Coverage count | The number of distinct sources observed reporting a story; used for trending / most-covered ranking. |
| AI take / analysis | System-generated opinion/commentary plus sentiment & impact for a story. |
| Rewrite | An original, system-authored paragraph restating a story in its own words. |
| Moderation | Admin review of AI-generated output before/while it is exposed to readers. |

## Appendix B — Open questions / TBD
- Product name (working title: "AI News Aggregator").
- Monetization (ads/sponsorship) — in or out of MVP?
- Target ingestion/refresh frequency default value (to be fixed as an NFR;
  mechanism resolved — single global interval, FR-INGEST-010).
- Final pre-specified source list and per-source legal/ToS review.
