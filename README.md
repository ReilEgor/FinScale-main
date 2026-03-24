# FinScale-main

> **FinScale** is a high-performance microservices platform for personal finance and crypto-asset management.  
> The system provides precision transaction tracking, real-time currency monitoring, and secure financial document storage.

---

## 📦 Repository Structure

```
FinScale-backend/
├── AuthValidatorService/  # JWT validation & Keycloak OIDC integration
├── CurrencyService/       # Exchange rates (CryptoCompare + Redis cache)
├── TransactionService/    # Transaction ledger (PostgreSQL + AWS S3/MinIO)
├── UserService/           # User profile synchronization & metadata
├── deployments/           # Docker Compose & infrastructure configs
└── gateway/               # Nginx API Gateway configurations
```

---

## ✨ Features

- 💰 **Multi-Currency Ledger** — Native support for fiat and cryptocurrencies with 8-decimal precision using `NUMERIC` types
- 📈 **Real-time Rates** — Automated rate fetching via CryptoCompare API with low-latency Redis caching
- 📎 **Digital Receipts** — Direct upload of receipts and documents to AWS S3 with Presigned URLs for secure private access
- 🔐 **Enterprise Auth** — Robust authentication powered by Keycloak (OIDC) with centralized validation at the API Gateway level
- 🏛️ **Clean Architecture** — Strict separation of business logic from implementation details (DBs, External APIs, Cloud Providers)

---

## 🏗️ Architecture

```
Client → Nginx Gateway → AuthValidatorService → [TransactionService / CurrencyService / UserService]
                                                          ↓                      ↓
                                                   PostgreSQL + S3          Redis Cache
```

### Services

| Service | Description |
|---|---|
| **Gateway (Nginx)** | Single entry point managing routing and initial authorization handshakes |
| **Auth Validator** | Middleware service for validating Keycloak tokens and managing session states |
| **Transaction Service** | Core engine: manages transaction history in PostgreSQL and maps file keys to S3 objects |
| **Currency Service** | Aggregates external API data and minimizes overhead via Redis caching |
| **User Service** | Synchronizes user data between Keycloak and internal system databases |

---

## 🛠️ Core Stack

### Backend
![Go](https://img.shields.io/badge/Go-1.22+-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)

### Infrastructure
![Keycloak](https://img.shields.io/badge/Keycloak-4D4D4D?style=for-the-badge&logo=keycloak&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS_S3-FF9900?style=for-the-badge&logo=amazons3&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

---

## 🚀 Quick Start

### Prerequisites

- Docker + Docker Compose
- AWS Account (or LocalStack / MinIO for local dev)
- Configured Keycloak Realm

### Run System

```bash
# 1. Clone the repository
git clone https://github.com/ReilEgor/FinScale-backend
cd FinScale-backend/deployments

# 2. Setup environment
cp .env.example .env
# Important: fill in AWS_ACCESS_KEY and KEYCLOAK_CLIENT_SECRET

# 3. Spin up infrastructure
docker-compose up --build
```


## 📋 Environment Variables

| Parameter | Description |
|---|---|
| `DB_URL` | PostgreSQL connection string |
| `REDIS_ADDR` | Redis instance address (e.g. `redis:6379`) |
| `AWS_S3_BUCKET` | S3 bucket name for file storage |
| `COMPARE_FIN_API_KEY` | CryptoCompare API key |
| `KEYCLOAK_AUTH_SERVER` | Keycloak server base URL |

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

**Developed by [YehorReil](https://github.com/ReilEgor)**
