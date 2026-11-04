# Learning Management System Module Pack

| Atribut | Nilai |
|---|---|
| Document Code | MOD-LMS |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Learning/HR Owner |
| Related Documents | `../../01-requirements/frs.md`, `../../02-architecture/module-architecture/module-architecture-specification.md`, `../../04-backend/event-queue-specification.md`, `../../../docs/MTS.md` |

## 1. Module Overview

Owns courses, learning paths, lessons/materials, enrollment, assignments, quizzes/assessments, certification, progress and completion records. Integrates HRM/Employee, Organization and Notification.

## 2. Functional Specification

Manage course/category/path/lesson/material; enroll learner; assign content; deliver assignment/quiz/assessment; record progress/completion/certificate; report learning history. Exact content formats and grading policy TBD.

## 3. Business Process

Authorized owner publishes course → assign/open enrollment → employee enrolls → consume lessons/submit assessment → record progress/result → issue certificate/completion if criteria satisfied → HRM reference/history and notification. Content approval/expiry not defined.

## 4. Use Cases

Create course/path; assign/enroll employee; view lesson/material; submit assignment/quiz; grade/record assessment; issue certificate; report completion. Actors: learner, course manager/instructor and HR operator.

## 5. Business Rules

MTS defines capability only. Enrollment eligibility, completion thresholds, grading, retake, certificate validity, mandatory training and credit rules are TBD; no compliance requirement assumed.

## 6. Module Architecture

LMS owns course/learning records. HRM owns employee/employment; Organization supplies scope; Core Documents stores protected files; Notification delivers reminders. Do not write HRM employee table.

## 7. Database Design

Candidate courses, course_categories, learning_paths, lessons, learning_materials, enrollments, assignments, quizzes, assessments, submissions/results, certifications, progress/completion_records. Schema, content storage and version policy TBD.

## 8. Entity Relationship Diagram

Conceptual: Course has lessons/materials; learner enrollment links Employee/User to course/path; enrollment progress links lesson/assessment; completion can produce certificate. Retakes, versions and multi-course path cardinalities TBD.

## 9. Data Dictionary

See DOC-015. Candidate course status/version, enrollment date/status, progress, score/result, completion time, certificate identifier/validity. PII/assessment retention and scoring precision TBD.

## 10. API Specification

Candidate `/api/v1/courses`, `/learning-paths`, `/enrollments`, `/assignments`, `/assessments`, `/progress`. Content/download authorization and endpoint response/validation/pagination specified in DOC-019 before release.

## 11. Event & Queue Specification

Candidate enrollment created, assessment submitted/graded, course completed, certificate issued; content processing and reminder/report jobs asynchronous. Idempotent completion/certificate issue; payload privacy and retry in DOC-020.

## 12. UI/UX Specification

Course catalog, learner progress, lesson/material viewer, assignment/quiz flow, certificate, instructor/admin authoring and reports. Accessible media/transcript requirements, playback/device support and mobile/offline learning TBD.

## 13. Permission Matrix

| Action | Candidate permission | Scope |
|---|---|---|
| Browse/enroll | `lms.course.view`, `lms.enrollment.create` | Eligible learner |
| Manage course | `lms.course.create/update/publish` | Course owner |
| Submit assessment | `lms.assessment.submit` | Own enrollment |
| Grade/issue certificate | `lms.assessment.grade`, `lms.certificate.issue` | Instructor/authorized scope |
| View report | `lms.report.view` | HR/course scope |

Catalog and instructor assignment rules TBD.

## 14. Workflow Specification

Enrollment or certificate approval may use generic workflow if policy requires; MTS does not mandate it. Completion transitions and version handling defined per approved course policy; engine reference DOC-022.

## 15. Integration Specification

HRM employee and training record; Organization eligibility/scope; Core identity/files/notification; Analytics controlled aggregate. HRM record handoff contract and course content storage provider TBD.

## 16. Validation Rules

Validate active course/version, enrollment eligibility, deadline, submission/file safety, assessment attempt and completion criteria after policy approval. Exact limits, scoring, dates and certificate validity TBD.

## 17. Error Handling

403 ineligible/out-of-scope; 404 protected content; 409 stale course version/attempt conflict; 422 invalid submission; async file/media processing exposes status. Do not leak answer keys or assessment content.

## 18. Test Specification

Enrollment and scope isolation; content authorization; progress/retake/completion rules; duplicate certificate/notification; file scanning/access; HRM sync; assessment security (answer exposure); reporting privacy and migration.

## 19. Acceptance Criteria

Approved learning/completion rules; protected content and learner access; progress/certification tested; HRM integration reconciles; accessible content criteria set; module owner signoff/no critical defects.

## Constraints and Open Issues

Course authoring/versioning, formats/streaming, LMS-HRM ownership, grading/retakes, certificate policy, mandatory training/regulatory scope, mobile/offline access, content retention and accessibility standard TBD.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | LMS module pack draft | Documentation Agent |
