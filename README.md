# DevTinder - Full Stack Application

A Tinder-like application for developers to connect and collaborate.

## Project Structure

```
DevTinder/
├── backend/          # Express.js API server
├── frontend/         # React + Vite application
└── package.json      # Root package manager
```

## Tech Stack

### Backend

- Node.js + Express
- MongoDB + Mongoose
- JWT Authentication
- Socket.io (Real-time chat)
- Razorpay (Payments)
- AWS SES (Email)
- bcrypt, validator, cors

### Frontend

- React 18
- Redux Toolkit (State management)
- React Router (Navigation)
- Tailwind CSS + DaisyUI
- Socket.io Client
- Axios

## Getting Started

### Prerequisites

- Node.js (v20+)
- MongoDB
- npm or yarn

### Installation

1. Install all dependencies:

```bash
npm run install:all
```

Or install separately:

```bash
npm run install:backend
npm run install:frontend
```

### Running the Application

#### Development Mode

Run both frontend and backend concurrently:

```bash
npm install concurrently
npm run dev:all
```

Or run separately:

Backend only:

```bash
npm run dev
```

Frontend only:

```bash
npm run client
```

#### Production Mode

Build frontend:

```bash
npm run build:frontend
```

Start backend:

```bash
npm start
```

## Environment Variables

Create `.env` files in the backend directory with:

- MongoDB connection string
- JWT secret
- AWS SES credentials
- Razorpay keys
- Other configuration

## API Documentation

Check `backend/apiList.md` for available endpoints.

## Author

Manikanta
