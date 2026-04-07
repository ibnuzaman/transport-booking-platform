ticketing-system/
├── cmd/
│   └── api/
│       └── main.go
│
├── internal/
│   ├── bootstrap/
│   ├── module/
│   │   └── booking/
│   │       ├── delivery/
│   │       │   └── http/
│   │       ├── usecase/
│   │       ├── repository/
│   │       └── entity/
│   ├── platform/
│   │   ├── config/
│   │   ├── database/
│   │   ├── cache/
│   │   ├── queue/
│   │   └── logger/
│   └── shared/
│       ├── errors/
│       ├── response/
│       └── middleware/
│
├── pkg/
│   └── utils/
│
├── migrations/
├── docker/
├── scripts/
└── go.mod

Referensi aturan implementasi dan dependensi:
- Lihat [ARCHITECTURE_RULES.MD](ARCHITECTURE_RULES.MD)