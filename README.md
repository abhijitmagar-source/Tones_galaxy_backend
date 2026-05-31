# 🎵 Tones Galaxy — Backend

A RESTful API backend for **Tones Galaxy**, a platform where users can upload, discover, like, and download ringtones. Built with Django REST Framework and JWT authentication.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Framework | Django 5.1.2 + Django REST Framework 3.15.2 |
| Auth | JWT via `djangorestframework-simplejwt` |
| API Docs | `drf-spectacular` (Swagger UI) |
| CORS | `django-cors-headers` |
| Image Handling | Pillow |
| Database | SQLite (development) |

---

## 📁 Project Structure

```
Tones_galaxy_backend/
├── backend/            # Core Django project (settings, URLs, WSGI/ASGI)
├── api/                # API root — routes to users & ringtones
├── users/              # User registration, login, profile management
├── ringtones/          # Ringtone upload, listing, likes, download
├── manage.py
└── requirements.txt
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/Tones_galaxy_backend.git
cd Tones_galaxy_backend
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Apply migrations

```bash
python manage.py migrate
```

### 5. Create a superuser (optional)

```bash
python manage.py createsuperuser
```

### 6. Run the development server

```bash
python manage.py runserver
```

The API will be available at `http://localhost:8000`.

---

## 🔑 Authentication

This project uses **JWT (JSON Web Tokens)**.

- Access token lifetime: **120 minutes**
- Refresh token lifetime: **2 days**
- Login supports both **username** and **email**

Include the access token in the `Authorization` header for protected endpoints:

```
Authorization: Bearer <access_token>
```

---

## 📡 API Endpoints

### Base URL: `/api/v1/`

---

### 👤 Users — `/api/v1/users/`

| Method | Endpoint | Auth Required | Description |
|---|---|---|---|
| `POST` | `register/` | No | Register a new user |
| `POST` | `login/` | No | Login with username or email |
| `POST` | `token/refresh/` | No | Refresh access token |
| `GET` | `profile/` | ✅ Yes | Get authenticated user's profile |
| `PUT` | `profile/` | ✅ Yes | Update profile (bio, picture, website) |
| `DELETE` | `profile/delete/` | ✅ Yes | Delete account and profile |

#### Register — `POST /api/v1/users/register/`

```json
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "pass1234"
}
```

> Password must be at least 8 characters and contain both letters and numbers.

#### Login — `POST /api/v1/users/login/`

```json
{
  "username": "john_doe",
  "password": "pass1234"
}
```

Response:

```json
{
  "access": "<access_token>",
  "refresh": "<refresh_token>"
}
```

#### Update Profile — `PUT /api/v1/users/profile/`

Supports `multipart/form-data` for profile picture uploads.

```json
{
  "bio": "Music lover 🎧",
  "website": "https://mysite.com",
  "profile_picture": "<file>"
}
```

---

### 🎵 Ringtones — `/api/v1/ringtones/`

| Method | Endpoint | Auth Required | Description |
|---|---|---|---|
| `GET` | `/` | No | List all ringtones (newest first) |
| `POST` | `/` | ✅ Yes | Create/upload a ringtone |
| `GET` | `{id}/` | No | Get ringtone details |
| `PUT/PATCH` | `{id}/` | ✅ Owner only | Update a ringtone |
| `DELETE` | `{id}/` | ✅ Owner only | Delete a ringtone |
| `GET` | `{id}/download/` | No | Download ringtone file |
| `POST` | `{id}/like/` | ✅ Yes | Like / unlike a ringtone |
| `GET` | `liked_by_user/` | ✅ Yes | Get ringtones liked by current user |
| `POST` | `upload/` | ✅ Yes | Upload a ringtone (multipart) |

#### Upload Ringtone — `POST /api/v1/ringtones/upload/`

Multipart form data:

| Field | Type | Required |
|---|---|---|
| `file` | Audio file | ✅ |
| `name` | String | ✅ |
| `genre` | String (see choices) | ✅ |
| `description` | String | No |
| `tags` | Comma-separated string | No |

#### Supported Genres

`pop` · `rock` · `hiphop` · `jazz` · `classical` · `electronic` · `instrumental` · `bgm` · `remix` · `slowed` · `lofi` · `indie` · `k-pop` · `rnb` · `others`

---

## 📖 API Documentation

Interactive Swagger docs are available at:

```
http://localhost:8000/docs/
```

OpenAPI schema:

```
http://localhost:8000/schema/
```

---

## 🔐 Permissions

- **Read** operations on ringtones are open to all (unauthenticated) users.
- **Write/Delete** operations on ringtones are restricted to the **owner** of the ringtone.
- Profile and like endpoints require authentication.

---

## ⚙️ Configuration Notes

- CORS is configured to allow `http://localhost:3000` (Next.js frontend in development). Update `CORS_ALLOWED_ORIGINS` in `settings.py` for production.
- Media files (ringtones, profile pictures) are stored under the `media/` directory.
- Replace `SECRET_KEY` in `settings.py` with a secure value before deploying to production.
- Switch from SQLite to PostgreSQL or another production-grade database for deployment.

---

## 📦 Requirements

```
Django==5.1.2
djangorestframework==3.15.2
djangorestframework-simplejwt==5.3.1
django-cors-headers==4.6.0
drf-spectacular==0.29.0
Pillow==11.0.0
django-extensions==3.2.3
```

---

## 📄 License

This project is open source. Add your preferred license here.
