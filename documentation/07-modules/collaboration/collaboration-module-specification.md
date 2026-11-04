# Collaboration Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-COLLAB |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Collaboration Product Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../04-backend/event-queue-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Provides internal announcements, discussion/comments, direct/group messaging, calendar/meeting, shared documents, activity feed, mentions and notification.

## 2. Functional Specification

Create/publish announcements; threads/comments; direct/group messages; calendar/meeting entries; attach/share documents; activity feed/mentions and notifications. Access follows organization or project context.

## 3. Business Process

Authorized user creates content in context → validate audience/scope → persist → notify subscribers asynchronously → recipients view/read/reply under same policy. Shared file access rechecks parent context permission.

## 4. Use Cases

Publish announcement; discuss project/task; send direct/group message; schedule meeting; mention user; share document; mark notification read. Realtime delivery not assumed.

## 5. Business Rules

MTS requires context-appropriate access for messages/documents. Retention, edits/deletion, moderation, membership, read receipts, delivery and privacy policy are TBD. No public external messaging assumed.

## 6. Module Architecture

Collaboration owns content, message/thread, calendar and activity records; Core owns identity, access, files/notification/audit. Project/Organization supply context references; direct cross-module table writes prohibited.

## 7. Database Design

Candidate announcements, discussions, comments, messages, conversations/members, calendar_events, meeting_refs, activity_items, mentions, shared_document_relations. Message store/search/realtime and retention model TBD; secure document bytes owned by Core Documents.

## 8. Entity Relationship Diagram

Conceptual: conversation has members/messages; discussion has comments; calendar event has invitees; content/mention references user and optional organization/project context; attachment relation points to Core file. Membership cardinality and deletion TBD.

## 9. Data Dictionary

See DOC-015. Content, actor, context reference, audience, created/edited timestamps, delivery/read state. Message privacy/classification, edit history and retention need approval.

## 10. API Specification

Candidate `/api/v1/announcements`, `/conversations`, `/messages`, `/calendar-events`, `/activity`; endpoint and pagination contracts per DOC-019. Context authorization mandatory; no unauthenticated public endpoints specified.

## 11. Event & Queue Specification

Candidate announcement published, message created, mention created; notification delivery, digest and scheduled meeting reminders may queue. Idempotency and retry avoid duplicate notification. Realtime transport not selected; DOC-020.

## 12. UI/UX Specification

Announcement feed, thread/comments, conversation list, message composer, calendar/meeting, mention and notification center. Clearly show membership/access context; empty/loading/error, keyboard support and responsive layout per DOC-023.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| View/publish announcement | `collaboration.announcement.view/create/publish` | Audience/org scope |
| Read/send message | `collaboration.message.view/create` | Conversation membership |
| Comment | `collaboration.discussion.comment` | Parent context scope |
| Manage calendar | `collaboration.calendar.view/create/update` | Invitee/org scope |
| Share file | `collaboration.document.share` | Parent record + file permission |

Final moderation/admin and membership policy TBD.

## 14. Workflow Specification

No mandatory approval workflow specified by MTS. Optional announcement approval may be configured only after policy decision; generic workflow DOC-022. Scheduled reminders are not approval workflow.

## 15. Integration Specification

Core Identity/Organization/access/notification/documents; Project context; Analytics aggregate only if privacy approved. Email/messaging providers are external candidates, not selected. No direct write to referenced domains.

## 16. Validation Rules

Validate audience/membership/context, content length/format, attachment authorization/type/size, recipient scope, meeting date/time/timezone, mention target. Numeric limits and retention TBD.

## 17. Error Handling

403 non-member or invalid context; 404 disclosure-safe; 422 malformed content; 409 stale membership/update; async delivery failure shown separately from persisted message. No secret/content leakage in error logs.

## 18. Test Specification

Conversation membership isolation; cross-org/project access; attachment access; mention delivery dedupe; message persistence on provider outage; edit/retention policy after approved; search/index permissions; realtime behavior only if chosen.

## 19. Acceptance Criteria

Context access enforced server-side; content and notification lifecycle tested; files protected; privacy/retention rules approved; async failure observable; collaboration owner signoff/no critical defect.

## Constraints and Open Issues

Realtime since v1, message encryption, search, retention, edit/delete, moderation, external messaging, calendars/timezones, notification preferences, conversation membership and privacy policy TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Collaboration module pack draft | Documentation Agent |
