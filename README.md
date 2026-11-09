# 🔐 Secure Auth Engine

A production-ready, highly secure authentication backend engine built with **Node.js, Express, and MongoDB**. Designed using a decoupled layered architecture, it features schema validation, automated OTP email verification, secure cookie-based token rotation, and Google OAuth2 integration.

---

## 🚀 Architectural & Security Highlights
*   **Decoupled Architecture:** Follows a clean Service-Repository-Controller layout to completely isolate business logic from server entry points.
*   **Robust Session Management:** Implements single-session termination as well as global invalidation across all user devices.
*   **Dual-Token Security:** Utilizes short-lived JSON Web Tokens (JWT) for route authorization alongside cryptographically strong, HTTP-only cookie-based Refresh Tokens.
*   **Automated Lifecycle Verification:** Blocks account access completely until OTP codes transmitted securely via SMTP (Nodemailer) are verified.

---

## 🛠️ Technology Stack
*   **Core Execution:** Node.js, JavaScript (ES6+)
*   **Server Framework:** Express.js
*   **Data Modeling:** MongoDB with Mongoose ODM
*   **Authentication & Cryptography:** JWT (`jsonwebtoken`), `bcrypt` (10 rounds salting), `cookie-parser`
*   **Third-Party Integrations:** Google OAuth2 (`googleapis`), SMTP Services (Nodemailer)

---

## 📂 Project Architecture
The backend structure isolates responsibilities for maintainability and clean horizontal scaling:
```text
├── config/           # Database connections and API credentials configuration
├── model/            # Mongoose schemas definition (User, Session, Token states)
├── services/         # Isolated business logic layer (Auth logic, Mail triggers)
├── src/              # HTTP layer controllers and application route bindings
├── utils/            # Shared utilities (OTP generator, response error formatters)
├── index.js          # Core entry point initialized with server configurations
└── .env.example      # Context setup template for environmental keys
```

---

## 📡 API Reference & Protocol Contracts

### 🔓 Authentication Strategy (Public Access)

#### **1. Account Creation**
*   **Endpoint:** `POST /api/auth/register`
*   **Payload Profile:**
    ```json
    {
      "username": "john_doe",
      "email": "john@example.com",
      "password": "securepassword123"
    }
    ```

#### **2. Email Validation OTP**
*   **Endpoint:** `POST /api/auth/verify-email`
*   **Payload Profile:**
    ```json
    {
      "email": "john@example.com",
      "otp": "847291"
    }
    ```

#### **3. Identity Authentication (Login)**
*   **Endpoint:** `POST /api/auth/login`
*   **Payload Profile:**
    ```json
    {
      "email": "john@example.com",
      "password": "securepassword123"
    }
    ```

#### **4. Token Rotation Matrix**
*   **Endpoint:** `GET /api/auth/refresh-token`
*   **Behavior:** Assesses the current state of incoming HTTP-only cookies to exchange expired access tokens for fresh ones seamlessly.

---

### 🔒 Active State Management (Bearer Protected)
> *Requires headers containing: `Authorization: Bearer <access_token>`*

| Method | Endpoint | Action Context |
| :--- | :--- | :--- |
| `GET` | `/api/auth/get-me` | Extracts and returns the full current user context payload. |
| `GET` | `/api/auth/logout` | Revokes current browser cookies and single-session footprint. |
| `GET` | `/api/auth/logout-all` | Destroys all stored device keys tied to the target account. |

---

## ⚙️ Local Engine Initialization

### 1. Replicate Project State
```bash
git clone https://github.com](https://github.com/daniyalali047-commits/authentication-backend-system)
cd authentication-backend-system
```

### 2. Dependency Resolution
```bash
npm install
```

### 3. Contextual Environmental Binding
Configure your localized parameters by creating a `.env` instance template inside the root framework layout:
```env
PORT=3000
MONGODB_URI=your_mongodb_connection_uri
JWT_SECRET=your_cryptographic_access_secret_key
GOOGLE_CLIENT_ID=your_google_cloud_client_id
GOOGLE_CLIENT_SECRET=your_google_cloud_client_secret
GOOGLE_REFRESH_TOKEN=your_google_oauth_refresh_token
GOOGLE_USER=your_verified_system_email@gmail.com
```

### 4. Boot Ecosystem
```bash
# Run local staging instance with automatic hot reloading
npm run dev
```
The endpoint engine binds cleanly to: `http://localhost:3000`

---

## 🚀 System Optimizations Pipeline
*   Implement persistent storage layer blacklisting using an in-memory database store like **Redis** for expired tokens.
*   Introduce standard system assertion suites utilizing tools like **Jest** and **Supertest** to cover controller assertions.
*   Pack systemic deployment boundaries inside automated **Docker containers** for unified cloud-native scaling workflows.
