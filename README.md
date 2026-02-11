# TheFriendRequest

**TheFriendRequest** is a microservices-based social + event discovery platform that helps students find friends and communities around shared interests and upcoming events.

At a high level:

**Frontend (React)** → **API Gateway** → **Composite (Orchestrator) Service** → **Atomic Services (Users / Events / Feed)** → **MySQL Databases**  
…and Pub/Sub + email notifications for key events.  [oai_citation:0‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service)

---

## Repositories

| Repo | Purpose |
|------|---------|
| `frontend-service` | React + TypeScript SPA (Firebase Auth, talks to API Gateway)  [oai_citation:1‡GitHub](https://github.com/TheFriendRequest/frontend-service) |
| `API-Gateway-Service` | Single entrypoint: Firebase token verification + request routing to Composite service  [oai_citation:2‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service) |
| `Main-Backend-Service` | “Composite Service” orchestrator: aggregates endpoints, manages GraphQL, Pub/Sub subscribers, cross-service workflows  [oai_citation:3‡GitHub](https://github.com/TheFriendRequest/Main-Backend-Service) |
| `Users-Service` | User profiles, interests, search, friend requests; backed by MySQL  [oai_citation:4‡GitHub](https://github.com/TheFriendRequest/Users-Service) |
| `Event-Service` | Event CRUD + interest-based discovery; publishes `event-created` messages  [oai_citation:5‡GitHub](https://github.com/TheFriendRequest/Event-Service) |
| `Feed-Service` | Posts + feed generation + likes; backed by MySQL  [oai_citation:6‡GitHub](https://github.com/TheFriendRequest/Feed-Service) |
| `DB-Service` | MySQL schema + init scripts for Users / Events / Feed databases  [oai_citation:7‡GitHub](https://github.com/TheFriendRequest/DB-Service) |
| `Chatting-Service` | Chat service (WIP / minimal docs currently)  [oai_citation:8‡GitHub](https://github.com/TheFriendRequest/Chatting-Service) |
| `AI-Recommendations-Service` | AI recommendations service (WIP / minimal docs currently)  [oai_citation:9‡GitHub](https://github.com/TheFriendRequest/AI-Recommendations-Service) |
| `.github` | Org-level GitHub configuration |

---

## Architecture

### Request flow
- **Frontend** calls the **API Gateway**
- **API Gateway** verifies Firebase ID tokens and injects `x-firebase-uid` for downstream services  [oai_citation:10‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service)
- **Composite Service** coordinates calls across atomic services and exposes unified endpoints (including a combined user feed)  [oai_citation:11‡GitHub](https://github.com/TheFriendRequest/Main-Backend-Service)
- **Users / Events / Feed Services** handle their own domain logic + MySQL persistence  [oai_citation:12‡GitHub](https://github.com/TheFriendRequest/Users-Service)

### Async notifications
- Event Service publishes `event-created` messages
- Composite Service subscribes and sends email notifications (SMTP)  [oai_citation:13‡GitHub](https://github.com/TheFriendRequest/Event-Service)

---

## Tech Stack

- **Frontend:** React 18 + TypeScript (Create React App), React Router, Context API, Firebase Auth  [oai_citation:14‡GitHub](https://github.com/TheFriendRequest/frontend-service)
- **Backend:** Python (FastAPI/Uvicorn), microservices architecture  [oai_citation:15‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service)
- **Data:** MySQL (local or Cloud SQL), schema scripts in `DB-Service`  [oai_citation:16‡GitHub](https://github.com/TheFriendRequest/DB-Service)
- **Cloud (optional):** Cloud Run, Cloud Storage static hosting, Pub/Sub  [oai_citation:17‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service)

---

## Local Development (Quickstart)

### 1) Prereqs
- Python **3.9+**
- Node.js **16+**
- MySQL **8.0+**
- Firebase project + service account key (`serviceAccountKey.json`)  [oai_citation:18‡GitHub](https://github.com/TheFriendRequest/API-Gateway-Service)

---

### 2) Databases (MySQL)
Use `DB-Service` init scripts:

```sql
CREATE DATABASE user_db;
CREATE DATABASE event_db;
CREATE DATABASE feed_db;