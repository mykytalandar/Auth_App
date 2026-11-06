# Auth App

A full-stack authentication application that provides secure user registration, email verification, login, password recovery, and profile management.

## Features

### Authentication & Account Management
- **User Registration** — register with name, email, and password.
- **Password Validation** — inform users about password requirements and validate their input.
- **Email Verification** — activate accounts through unique confirmation links sent via email.
- **User Login** — authenticate using email and password.
- **JWT Authentication** — secure API endpoints using Access and Refresh Tokens.
- **Protected Routes** — restrict access to pages based on authentication status.
- **Logout** — terminate the authenticated session.

### Password Management
- **Password Recovery** — request a password reset link via email.
- **Password Reset** — set a new password using a unique reset token.
- **Password Change** — change the current password by providing the old password, new password, and confirmation.

### Profile Management
- **Profile Information** — view and update personal information.
- **Name Update** — change the account name.
- **Email Change** — request an email address change with password verification and new email confirmation.
- **Email Notifications** — notify the previous email address when the account email is changed.

### User Experience
- **Form Validation** — validate form inputs and display relevant feedback.
- **Loading States** — indicate ongoing operations.
- **404 Page** — handle requests to unavailable routes.
- **Authentication-based Navigation** — redirect users according to their authentication status.

## Tech Stack

### Frontend
- React 19
- TypeScript
- Vite
- React Router 7
- React Context API
- Axios
- React Tooltip

### Backend
- Node.js
- Express 5
- Sequelize ORM
- PostgreSQL
- JSON Web Tokens (JWT)
- bcrypt
- Nodemailer
- Cookie Parser
- CORS

## Architecture

The application follows a client-server architecture with separate frontend and backend applications.

### Frontend
- **React Context API** — manage global authentication state and user information.
- **React Router** — handle application routing and navigation.
- **Formik** — manage forms and form validation.
- **Axios** — communicate with the backend REST API.

### Backend
The backend uses a modular structure with separated responsibilities:

- **Controllers** — handle incoming requests and coordinate application logic.
- **Services** — encapsulate business logic and operations such as authentication, token management, and email delivery.
- **Models** — define database entities using Sequelize.
- **Routes** — define REST API endpoints.
- **Middlewares** — handle authentication and request processing.
- **Exceptions** — manage application-specific errors.
- **Utils** — provide reusable helper functions.

### Authentication Flow

The application uses JWT-based authentication with two types of tokens:

- **Access Token** — used to authorize API requests through the Authorization header. Expires after 60 seconds.
- **Refresh Token** — stored in an HTTP-only cookie and persisted in the PostgreSQL database.

The backend validates Access Tokens through authentication middleware. Refresh Tokens are used to obtain new authentication tokens.

### Email Service

Nodemailer is used to send transactional emails through an SMTP server.

Email notifications are implemented for:
- Account activation.
- Password reset.
- New email address confirmation.
- Notifications about email address changes.

## Database

The application uses PostgreSQL with Sequelize ORM.

### Models

**User**
- `id`
- `name`
- `email`
- `password`
- `activationToken`
- `resetToken`
- `pendingEmail`
- `emailChangeToken`

**Token**
- `id`
- `refreshToken`
- `userId`

The `User` and `Token` models have a one-to-one relationship.

## API Endpoints

### Authentication

| Method | Endpoint | Description |
|---|---|---|
| POST | `/registration` | Register a new user |
| POST | `/activation/:activationToken` | Activate an account |
| POST | `/login` | Authenticate a user |
| GET | `/refresh` | Refresh authentication tokens |
| GET | `/logout` | Log out |
| POST | `/reset-password` | Request a password reset |
| POST | `/reset-password/confirm` | Confirm a new password |

### Profile

| Method | Endpoint | Description |
|---|---|---|
| PATCH | `/profile/name` | Update account name |
| PATCH | `/profile/password` | Change account password |
| PATCH | `/profile/email` | Request an email change |
| GET | `/profile/confirm-email-change/:emailChangeToken` | Confirm a new email address |

## Project Structure

```text
Auth_App/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── exceptions/
│   │   ├── middlewares/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── services/
│   │   ├── utils/
│   │   └── index.js
│   ├── .env.example
│   ├── package.json
│   └── setup.js
│
├── frontend/
│   ├── src/
│   │   ├── api/
│   │   ├── context/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── types/
│   │   ├── utils/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── style.css
│   ├── package.json
│   └── vite.config.ts
│
└── README.md
```

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js
- npm
- PostgreSQL

You will also need an SMTP account for sending emails.

### Installation

Clone the repository:

```bash
git clone https://github.com/mykytalandar/Auth_App.git
cd Auth_App
```

Install backend dependencies:

```bash
cd backend
npm install
```

Install frontend dependencies:

```bash
cd ../frontend
npm install
```

### Environment Variables

Create a `.env` file in the `backend` directory based on the provided `.env.example` file.

Configure the following variables:

```env
PORT=3005

DB_HOST=localhost
DB_USER=postgres
DB_PASSWORD=your_database_password
DB_DATABASE=postgres

SERVER_HOST=http://localhost:3005
CLIENT_HOST=http://localhost:5173

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_smtp_app_password

JWT_KEY=your_access_token_secret
JWT_REFRESH_KEY=your_refresh_token_secret
```

**Important:**
- Configure the database credentials according to your local PostgreSQL installation.
- Configure SMTP credentials to enable email delivery.
- `CLIENT_HOST` must match the frontend URL provided by Vite.
- Use strong, unique secrets for JWT signing.
- Never commit your actual `.env` file or credentials to the repository.

### Database Setup

Make sure PostgreSQL is running and the database specified in your environment variables exists.

Initialize the database by running the setup script from the backend directory:

```bash
cd backend
node setup.js
```

**Note:** The setup script uses Sequelize `sync({ force: true })`, which recreates database tables and removes existing data. Run it only when initializing or intentionally resetting the database.

### Running the Application

Start the backend server:

```bash
cd backend
npm start
```

The backend will run at:

```text
http://localhost:3005
```

Start the frontend development server in a separate terminal:

```bash
cd frontend
npm run dev
```

Vite will display the frontend development URL in the terminal (usually `http://localhost:5173`).

## Author

Developed as a full-stack learning project focused on authentication, account management, email verification, and secure API development.
