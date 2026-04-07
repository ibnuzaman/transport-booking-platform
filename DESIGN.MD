# Technical Design & Implementation Roadmap

Referensi dokumen:
- Project Charter: [PROJECT_CHARTER.MD](PROJECT_CHARTER.MD)
- Product Requirements: [PRD.MD](PRD.MD)
- Architecture Rules: [ARCHITECTURE_RULES.MD](ARCHITECTURE_RULES.MD)

## 1. Design Goals

- Menjaga arsitektur tetap modular dan scalable.
- Menjamin konsistensi booking (anti double booking).
- Menyiapkan sistem untuk event-driven processing.
- Menyiapkan fondasi production readiness (CI/CD, observability, security).

## 2. High-Level Architecture Direction

- API service untuk request sinkron (auth, browse, booking request).
- Worker service untuk proses async berbasis event.
- PostgreSQL sebagai source of truth data transaksional.
- Redis untuk caching dan optimasi read-heavy endpoint.
- Kafka untuk komunikasi event async antar komponen.

## 3. 12-Week Roadmap

## Month 1 — Foundation (System Design + DB)

### Week 1 — Clean Architecture + Basic API

**Target**
- Setup project yang rapi dan berlapis.

**Tasks**
- Setup project Go dengan struktur modular.
- Implement register/login berbasis JWT.
- Setup PostgreSQL dan koneksi dasar.

**Daily Plan**
- Day 1-2: setup project + struktur folder
- Day 3-4: auth (JWT)
- Day 5: DB schema
- Day 6: basic CRUD user
- Day 7: refactor clean architecture

### Week 2 — Ticketing Core

**Target**
- Core bisnis booking berjalan end-to-end.

**Tasks**
- Definisikan entity: Event/Route, Schedule, Seat, Booking.
- Bangun endpoint: create event, get schedule, booking ticket.
- Implement anti double booking via `SELECT ... FOR UPDATE` atau optimistic locking.

**Daily Plan**
- Day 1-2: schema design
- Day 3-4: booking logic
- Day 5: seat availability logic
- Day 6: concurrency test
- Day 7: refactor + cleanup

### Week 3 — Database Deep Dive

**Target**
- Query efisien dan terukur.

**Tasks**
- Analisis query menggunakan `EXPLAIN ANALYZE`.
- Tambahkan BTREE index dan partial index sesuai kebutuhan.
- Optimasi query booking & schedule.

**Daily Plan**
- Day 1: belajar EXPLAIN
- Day 2: analisis query lambat
- Day 3: implement index
- Day 4: benchmark before/after
- Day 5: optimize join
- Day 6-7: cleanup + dokumentasi

### Week 4 — Basic System Design

**Target**
- Fondasi performa dan reliability awal.

**Tasks**
- Integrasi Redis untuk caching dasar.
- Implement cache schedule API + invalidation strategy awal.
- Implement rate limiting dasar.

**Daily Plan**
- Day 1-2: integrate Redis
- Day 3: cache schedule API
- Day 4: cache invalidation
- Day 5: basic rate limiting
- Day 6-7: review & improvement

## Month 2 — Scaling & Distributed

### Week 5 — Kafka Introduction

**Target**
- Memulai event-driven flow.

**Tasks**
- Setup Kafka via Docker.
- Implement producer untuk booking event.
- Implement consumer worker dasar.

**Daily Plan**
- Day 1: install Kafka
- Day 2: basic producer
- Day 3: basic consumer
- Day 4-5: integrasi ke booking flow
- Day 6-7: debugging

### Week 6 — Async Processing

**Target**
- Booking diproses asynchronous dengan worker.

**Tasks**
- Ubah flow: Booking -> Kafka -> Worker process.
- Tambahkan retry mechanism.
- Tambahkan logging proses worker.

**Daily Plan**
- Day 1-2: redesign flow
- Day 3-4: worker logic
- Day 5: retry logic
- Day 6-7: testing

### Week 7 — Idempotency & Consistency

**Target**
- Tahan duplicate request dan transient failure.

**Tasks**
- Implement idempotency key di API.
- Pastikan retry worker tidak menyebabkan double insert.
- Perkuat konsistensi booking.

**Daily Plan**
- Day 1: konsep idempotency
- Day 2-3: implement di API
- Day 4-5: implement di worker
- Day 6-7: stress test

### Week 8 — Advanced Caching

**Target**
- Meningkatkan performa endpoint yang read-heavy.

**Tasks**
- Cache schedule dan seat availability.
- Tentukan TTL strategy.
- Finalisasi cache invalidation.

**Daily Plan**
- Day 1-2: caching strategy
- Day 3-4: implement
- Day 5: invalidation
- Day 6-7: testing

## Month 3 — Production Ready

### Week 9 — Docker & Service Split

**Target**
- Menyiapkan service boundary.

**Tasks**
- Pisahkan API service dan worker service.
- Dockerize seluruh service.

**Daily Plan**
- Day 1-2: split service
- Day 3-4: Dockerfile
- Day 5: docker-compose
- Day 6-7: testing

### Week 10 — Kubernetes

**Target**
- Deployment yang scalable.

**Tasks**
- Deploy API, worker, Redis, Kafka ke Kubernetes.
- Atur service dan scaling policy dasar.

**Daily Plan**
- Day 1: konsep Kubernetes
- Day 2-3: deployment
- Day 4: service
- Day 5: scaling
- Day 6-7: debugging

### Week 11 — CI/CD + Cloud

**Target**
- Otomasi build-test-deploy.

**Tasks**
- Setup pipeline CI/CD (build, test, deploy).
- Deploy ke Google Cloud Platform.

**Daily Plan**
- Day 1-2: CI setup
- Day 3-4: CD pipeline
- Day 5: deploy cloud
- Day 6-7: testing

### Week 12 — Hardening & Final Polish

**Target**
- Kualitas production-level.

**Tasks**
- Tambahkan rate limiter.
- Structured logging + monitoring basic.
- Security hardening: JWT best practice dan input validation.

**Daily Plan**
- Day 1-2: security
- Day 3-4: logging
- Day 5: monitoring
- Day 6-7: final testing

## 4. Exit Criteria (End of Month 3)

- Booking flow stabil dan anti double booking.
- Async processing berjalan dengan retry & idempotency.
- Layanan ter-container dan siap deploy di Kubernetes.
- Pipeline CI/CD aktif untuk build-test-deploy.