# Photo Album App — Design Spec

**Date:** 2026-06-11  
**Purpose:** A shared photo album app where anyone with a link can view and upload photos. No authentication required.

## Overview

A minimal Node.js web application for creating photo albums and sharing them via unique links. Friends and family can upload photos and view/download them without needing accounts.

## Architecture

- **Runtime:** Node.js with Express.js framework
- **Templating:** EJS (server-rendered HTML)
- **Database:** SQLite via `better-sqlite3` for album/photo metadata
- **File storage:** Local filesystem (`./uploads/<album-id>/`)
- **Upload handling:** Multer middleware
- **Album sharing:** UUID-based URLs — each album gets a unique ID

### Key Dependencies

- `express` — HTTP server and routing
- `ejs` — HTML templating
- `better-sqlite3` — SQLite database access
- `multer` — multipart file upload handling
- `uuid` — unique album/photo ID generation

## Data Model

### Albums

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT (UUID) | Primary key, used in shareable URL |
| name | TEXT | Album display name |
| created_at | TEXT (ISO 8601) | Creation timestamp |

### Photos

| Column | Type | Description |
|--------|------|-------------|
| id | TEXT (UUID) | Primary key |
| album_id | TEXT | Foreign key → albums.id |
| filename | TEXT | Stored filename (`<id>.<ext>`) |
| original_name | TEXT | Original upload filename |
| uploaded_at | TEXT (ISO 8601) | Upload timestamp |

## File Storage

Photos are stored at: `./uploads/<album_id>/<photo_id>.<ext>`

The `uploads/` directory is created automatically on first upload. Each album gets its own subdirectory.

## Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Homepage — form to create a new album |
| `/` | POST | Create album → redirect to `/album/:id` |
| `/album/:id` | GET | View album: name, upload form, photo timeline |
| `/album/:id/upload` | POST | Upload one or more photos to album |
| `/album/:id/photo/:photoId` | GET | Serve photo file (inline display) |
| `/album/:id/photo/:photoId/download` | GET | Download photo with original filename |

## UI Flow

### Homepage (`/`)

- Simple page with app title and a "Create New Album" form
- Form has one field: album name
- On submit, creates the album and redirects to it

### Album Page (`/album/:id`)

- Displays album name and a "Share this album" copyable link
- Upload section: file input accepting multiple images, upload button
- Photo list: vertical timeline, newest first
- Each photo shows: thumbnail/image, original filename, upload date, download link
- If album doesn't exist, show 404 page

### Sharing

- The album URL (`/album/<uuid>`) is the shareable link
- Anyone with the link can view all photos and upload new ones
- No authentication or access control

## Error Handling

- Invalid album ID → 404 page
- Invalid photo ID → 404 page
- Upload failures (file too large, invalid type) → error message on album page
- File size limit: 10MB per photo
- Accepted formats: JPEG, PNG, GIF, WebP

## Project Structure

```
photo-album/
├── package.json
├── server.js              # Express app entry point
├── db.js                  # SQLite setup and queries
├── routes/
│   ├── home.js            # GET/POST /
│   └── album.js           # Album and photo routes
├── views/
│   ├── layout.ejs         # Shared HTML layout
│   ├── home.ejs           # Homepage template
│   ├── album.ejs          # Album view template
│   └── 404.ejs            # Not found page
├── public/
│   └── style.css          # Minimal CSS
├── uploads/               # Photo storage (gitignored)
└── data/                  # SQLite DB file (gitignored)
```

## Constraints and Decisions

- **No authentication:** Simplicity over security. Anyone with the link has full access.
- **No image processing:** Photos are stored and served as-is. No thumbnails or resizing.
- **No pagination:** Suitable for small-to-medium albums (dozens to low hundreds of photos).
- **SQLite:** No need for a separate database server. Database file lives in `./data/album.db`.
- **Local storage only:** Not designed for cloud deployment without modification.

## Future Considerations (Not in Scope)

These are explicitly out of scope but noted for potential future work:

- Image thumbnails/resizing
- Album deletion / photo deletion
- Password-protected albums
- Cloud storage (S3, Azure Blob)
- Pagination for large albums
- Drag-and-drop upload UI
