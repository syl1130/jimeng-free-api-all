# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/claude-code) when working with code in this repository.

## Project Overview

Jimeng AI Free API Server - A reverse-engineered API server that provides OpenAI-compatible endpoints for Jimeng AI (即梦 AI) image and video generation capabilities.

**Key Features:**
- Text-to-Image generation (multiple models: jimeng-4.5, jimeng-4.1, etc.)
- Image-to-Image composition
- Video generation (jimeng-video-3.5-pro, jimeng-video-3.0, etc.)
- Seedance 2.0 multi-image intelligent video generation
- OpenAI API compatible endpoints

## Build and Development Commands

```bash
# Install dependencies
npm install

# Development mode with hot reload
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## Docker Commands

```bash
# Build Docker image
docker build -t jimeng-free-api-all:latest .

# Run container
docker run -it -d --init --name jimeng-free-api-all -p 8000:8000 -e TZ=Asia/Shanghai jimeng-free-api-all:latest

# Using pre-built image from Docker Hub
docker pull wwwzhouhui569/jimeng-free-api-all:latest
docker run -it -d --init --name jimeng-free-api-all -p 8000:8000 -e TZ=Asia/Shanghai wwwzhouhui569/jimeng-free-api-all:latest
```

## Project Architecture

```
src/
├── index.ts                    # Application entry point
├── daemon.ts                   # Daemon process management
├── api/
│   ├── controllers/            # Business logic controllers
│   │   ├── core.ts            # Core utilities (token handling, etc.)
│   │   ├── images.ts          # Image generation logic
│   │   ├── videos.ts          # Video generation logic (including Seedance 2.0)
│   │   └── chat.ts            # Chat completion logic
│   ├── routes/                 # API route definitions
│   │   ├── index.ts           # Route aggregator
│   │   ├── images.ts          # /v1/images/* endpoints
│   │   ├── videos.ts          # /v1/videos/* endpoints
│   │   ├── chat.ts            # /v1/chat/* endpoints
│   │   ├── models.ts          # /v1/models endpoint
│   │   └── ...
│   └── consts/                 # API constants and exceptions
└── lib/
    ├── server.ts              # Koa server setup with middleware
    ├── config.ts              # Configuration management
    ├── logger.ts              # Logging utilities
    ├── util.ts                # Helper utilities
    ├── request/               # Request handling classes
    ├── response/              # Response handling classes
    ├── exceptions/            # Exception classes
    └── configs/               # Configuration schemas
```

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/v1/chat/completions` | POST | OpenAI-compatible chat completions (for image/video generation) |
| `/v1/images/generations` | POST | Text-to-image generation |
| `/v1/images/compositions` | POST | Image-to-image composition |
| `/v1/videos/generations` | POST | Video generation (including Seedance 2.0) |
| `/v1/models` | GET | List available models |

## Key Technical Details

### Authentication
- Uses `sessionid` cookie from Jimeng website as Bearer token
- Multi-account support via comma-separated sessionids: `Authorization: Bearer sessionid1,sessionid2`

### Model Mappings
- **Image Models**: jimeng-4.5, jimeng-4.1, jimeng-4.0, jimeng-3.x, jimeng-2.x, jimeng-1.4, jimeng-xl-pro
- **Video Models**: jimeng-video-3.5-pro, jimeng-video-3.0, jimeng-video-3.0-pro, jimeng-video-2.0, jimeng-video-2.0-pro
- **Seedance Models**: seedance-2.0, seedance-2.0-pro (multi-image video generation)

### File Upload
- Supports multipart/form-data for file uploads
- koa-body configured with 100MB file size limit
- Files field can be object or array format (normalized in Request.ts)

### Seedance 2.0 Specifics
- Uses `unified_edit_input` structure with `material_list` and `meta_list`
- Internal model: `dreamina_seedance_40_pro`
- Draft version: 3.3.9
- Supports prompt placeholders: @1, @2, @图1, @image1 to reference uploaded images

## Development Guidelines

1. **TypeScript**: Project uses TypeScript with ESM modules
2. **Path Aliases**: Uses `@/` alias pointing to `src/` directory
3. **Logging**: Use the logger from `@/lib/logger.ts` for consistent output
4. **Configuration**: Environment configs in `configs/` directory, loaded via `@/lib/config.ts`
5. **API Compatibility**: Maintain OpenAI API compatibility for client integration

## Testing API Calls

```bash
# Text-to-image
curl -X POST http://localhost:8000/v1/images/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid" \
  -d '{"model": "jimeng-4.5", "prompt": "beautiful sunset", "ratio": "16:9", "resolution": "2k"}'

# Video generation
curl -X POST http://localhost:8000/v1/videos/generations \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_sessionid" \
  -d '{"model": "jimeng-video-3.5-pro", "prompt": "cat playing", "ratio": "16:9", "resolution": "720p"}'

# Seedance 2.0 multi-image video (file upload)
curl -X POST http://localhost:8000/v1/videos/generations \
  -H "Authorization: Bearer your_sessionid" \
  -F "model=seedance-2.0" \
  -F "prompt=@1 and @2 start dancing" \
  -F "files=@/path/to/image1.jpg" \
  -F "files=@/path/to/image2.jpg"
```

## Configuration

Default port: 8000
Configuration files in `configs/` directory with YAML format.
