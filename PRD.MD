# Product Requirements Document (PRD)

## 1. Product Name

Scalable Ticketing System (Event / Transport Booking Platform)

## 2. Objective

Membangun sistem ticketing yang:

- Scalable: mampu menangani traffic tinggi.
- Reliable: mencegah double booking.
- Modular: siap berkembang ke arsitektur microservice.
- Production-ready: siap CI/CD, observability, dan deployment cloud.

## 3. User Types

### 3.1 Customer

- Registrasi dan login.
- Menelusuri event/route.
- Melihat jadwal.
- Melakukan booking tiket.
- Melihat riwayat booking.

### 3.2 Admin

- Membuat dan mengelola event/route.
- Membuat dan mengelola schedule.
- Mengelola seat/inventori tiket.

### 3.3 System (Worker)

- Memproses job booking secara asynchronous.
- Menangani retry proses yang gagal.
- Mengirim notifikasi (simulasi).

## 4. Core Features

### 4.1 Customer Features

- Authentication: register/login.
- Catalog browsing: event/route listing.
- Schedule discovery: melihat jadwal.
- Booking flow: pesan tiket.
- Booking history: lihat status booking.

### 4.2 Admin Features

- Event/route management.
- Schedule management.
- Seat management.

### 4.3 System Features

- Async booking processing.
- Retry mechanism.
- Notification simulation.

## 5. Non-Functional Requirements

- Konsistensi data booking untuk menghindari double booking.
- Performa stabil pada traffic tinggi.
- Ketahanan proses async terhadap kegagalan sementara.
- Skalabilitas horizontal untuk API dan worker.

## 6. Technical Stack (Target)

- Backend language: Golang
- HTTP framework: net/http dan chi untuk middleware
- Database: PostgreSQL
- Cache: Redis
- Message broker: Apache Kafka
- Containerization: Docker
- Orchestration: Kubernetes
- Cloud target: Google Cloud Platform