# Project Charter

## 1. Project Identity

- Project Name: Scalable Ticketing System
- Domain: Event / Transport Booking Platform
- Start Date: April 2026
- Duration: 12 minggu (3 bulan)

## 2. Vision & Purpose

Membangun platform ticketing yang scalable, reliable, dan modular untuk menangani proses booking end-to-end dengan konsistensi data tinggi, siap dioperasikan pada lingkungan production.

## 3. Problem Statement

Sistem booking dengan traffic tinggi memiliki risiko:

- Double booking karena race condition.
- Performa turun saat beban baca/tulis meningkat.
- Sulit scale jika struktur codebase tidak modular.
- Sulit operasional jika belum ada pipeline deployment dan observability.

## 4. Objectives

- Menyediakan alur booking yang konsisten dan anti double booking.
- Menyiapkan arsitektur modular berbasis service + worker.
- Mengimplementasikan asynchronous processing untuk reliability.
- Menyediakan fondasi production readiness: containerization, orchestration, CI/CD, dan security baseline.

## 5. Scope

### In Scope

- Authentication dasar (register/login).
- Browse event/route dan schedule.
- Booking ticket + booking history.
- Admin management (event/route, schedule, seat).
- Async booking processing (Kafka + worker).
- Caching (Redis), retry mechanism, idempotency.
- Deployable services dengan Docker dan Kubernetes.
- CI/CD pipeline dasar untuk build, test, deploy.

### Out of Scope (Phase Berikutnya)

- Payment gateway production.
- Real notification channel (email/SMS/WA) non-simulasi.
- Advanced analytics/BI dashboard.
- Multi-region disaster recovery.

## 6. Stakeholders & Roles

- Product Owner: menentukan prioritas bisnis dan acceptance criteria.
- Tech Lead: keputusan teknis, quality gate, arsitektur.
- Backend Engineer: implementasi API, usecase, repository, worker.
- DevOps Engineer: CI/CD, container, Kubernetes, cloud deployment.
- QA Engineer: test strategy, integration test, regression test.

## 7. Success Criteria

- Functional:
  - Core flow customer dan admin berjalan sesuai PRD.
  - Worker async memproses booking dan retry dengan benar.
- Technical:
  - Tidak ada double booking pada skenario concurrent test utama.
  - API dan worker dapat di-deploy containerized.
  - Arsitektur mengikuti aturan di ARCHITECTURE_RULES.MD.
- Delivery:
  - Roadmap 12 minggu selesai dengan output per milestone.

## 8. Milestones (12 Weeks)

- Month 1 (Foundation): arsitektur, auth, DB schema, core booking.
- Month 2 (Scaling): Kafka, async worker, retry, idempotency, advanced caching.
- Month 3 (Production Ready): service split, Docker, Kubernetes, CI/CD, hardening.

## 9. Dependencies

- Infrastruktur PostgreSQL, Redis, Kafka tersedia.
- Akses environment container (Docker) dan cluster (Kubernetes).
- Akses cloud environment target (Google Cloud Platform).
- Repo dan branching strategy siap untuk CI/CD.

## 10. Risks & Mitigation

- Risiko: race condition saat booking.
  - Mitigasi: locking strategy, idempotency, stress test.
- Risiko: kompleksitas async flow.
  - Mitigasi: contract event yang jelas, observability, retry policy.
- Risiko: dev velocity turun karena scope melebar.
  - Mitigasi: jaga boundary scope per milestone dan weekly review.
- Risiko: arsitektur drift.
  - Mitigasi: enforce architecture rules pada code review.

## 11. Governance & Ways of Working

- Sprint cadence: weekly milestone review.
- Decision log: keputusan teknis penting dicatat terpisah.
- Definition of Done per fitur:
  - kode selesai + test relevan lulus,
  - dokumentasi diperbarui,
  - tidak melanggar aturan arsitektur,
  - siap deploy ke environment target.

## 12. References

- PRD: PRD.MD
- Technical Design: Design.MD
- Architecture Rules: ARCHITECTURE_RULES.MD
- Structure Overview: architecture.MD
