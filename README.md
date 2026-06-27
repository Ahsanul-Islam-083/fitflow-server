# FitFlow Server

[![Node.js](https://img.shields.io/badge/Node.js-18.x-green)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-Framework-lightgray)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-success)](https://www.mongodb.com/)

FitFlow Server is the backend API for the FitFlow fitness marketplace. It provides secure authentication, role-based access control, content management, and booking workflows for users, trainers, and admins.

---

## What this server does

This backend exposes a complete REST API for FitFlow features, including:

- User profile retrieval and admin user management
- Notification storage, reading, and deletion
- Forum posting, commenting, voting, and ownership checks
- Trainer application workflow with admin approval/rejection
- Class creation, approval, updates, and trainer workflows
- Booking creation, retrieval, and class attendee lists
- Favorite class tracking for users

---

## Quick Start

### Requirements

- Node.js 18 or newer
- MongoDB Atlas or local MongoDB instance
- Frontend application configured with `CLIENT_URL`

### Run locally

```bash
npm install
npm run dev
```

For production:

```bash
npm start
```

Default server port: `8000`

---

## Environment Variables

| Variable       | Required | Description                                 |
| -------------- | -------- | ------------------------------------------- |
| `PORT`         | No       | Port for the server                         |
| `MONGODB_URI`  | Yes      | MongoDB connection string                   |
| `CLIENT_URL`   | Yes      | Frontend origin used for CORS and JWKS path |

---

## API Overview

### Health Check

- `GET /`
  - Returns a simple response to verify the server is running.

### Users

- `GET /users/me` — returns the authenticated user profile.
- `GET /trainers` — returns featured public trainers.
- `GET /users` — returns all users (admin only).
- `PATCH /users/:id/block` — block a user (admin only).
- `PATCH /users/:id/unblock` — unblock a user (admin only).
- `PATCH /users/:id/feature` — mark a user as featured/unfeatured (admin only).

### Notifications

- `GET /notifications/:userId` — fetch notifications for a user.
- `PATCH /notifications/:id/read` — mark a notification as read.
- `PATCH /notifications/user/:userId/read-all` — mark all notifications as read.
- `DELETE /notifications/:id` — delete a notification.
- `DELETE /notifications/user/:userId` — delete all notifications for a user.

### Forum Posts

- `POST /forum-posts` — create a forum post (authenticated and not blocked).
- `GET /forum-posts` — list forum posts with pagination and filters.
- `GET /forum-posts/:id` — retrieve a single forum post.
- `PATCH /forum-posts/:id` — update a forum post (owner or admin).
- `DELETE /forum-posts/:id` — delete a forum post (owner or admin).
- `POST /forum-posts/:id/vote` — upvote or downvote a post.

### Forum Comments

- `GET /forum-posts/:postId/comments` — list comments for a post.
- `POST /forum-posts/:postId/comments` — add a comment to a post.
- `PATCH /forum-comments/:id` — edit a comment (owner or admin).
- `PATCH /forum-comments/:id/like` — like or unlike a comment.
- `DELETE /forum-comments/:id` — delete a comment (owner or admin).

### Trainer Applications

- `POST /trainer-applications` — submit a trainer application.
- `GET /trainer-applications` — view trainer applications.
- `PATCH /trainer-applications/:id` — approve or reject applications (admin only).

### Classes

- `POST /classes` — create a new class (trainer authenticated).
- `GET /classes/stats/summary` — get a summary of class stats.
- `GET /classes` — list classes with filters.
- `GET /classes/:id` — retrieve a single class.
- `PATCH /classes/:id/status` — update class approval status (admin only).
- `PATCH /classes/:id` — update class details (trainer authenticated).
- `DELETE /classes/:id` — delete a class (trainer authenticated).

### Bookings

- `POST /bookings` — create a booking.
- `GET /bookings` — get all bookings (admin only).
- `GET /bookings/user/:userId` — fetch bookings for a user.
- `GET /bookings/trainer/:trainerId` — fetch bookings for a trainer.
- `GET /bookings/trainer-and-user/:id` — fetch combined bookings for a trainer/user.
- `GET /bookings/class/:classId/attendees` — fetch attendees for a class.

### Favorite Classes

- `GET /favorite-classes/:userId` — list favorite class IDs for a user.
- `POST /favorite-classes` — add a favorite class.
- `DELETE /favorite-classes` — remove a favorite class.

---

## Security and Access Control

- `verifyToken` validates JWT tokens on protected routes.
- `verifyAdmin` restricts access to admin-only functionality.
- `verifyTrainer` restricts trainer-specific endpoints to trainers or admins.
- `verifyNotBlocked` prevents blocked users from creating or modifying resources.

---

## Notes

- The code is implemented entirely in `index.js`.
- Business logic is organized by route and middleware within that file.
- No code changes were made during this README update.
