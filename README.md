# 🎵 AI Music Generation Platform

A cutting-edge full-stack application that leverages AI to generate original music, album covers, and lyrics. Users can create songs in multiple modes, manage credits, and share their creations with the community.

![React](https://img.shields.io/badge/React-19.2.3-blue?logo=react)
![Next.js](https://img.shields.io/badge/Next.js-16.1.1-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5.8.2-blue?logo=typescript)
![Python](https://img.shields.io/badge/Python-3-blue?logo=python)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-blue?logo=postgresql)
![License](https://img.shields.io/badge/License-MIT-green)

## 🚀 Features

- **AI-Powered Music Generation**: Create original music using ACE-Step with AI-generated prompts and lyrics
- **Smart Lyric Generation**: Use Qwen2-7B LLM to generate structured lyrics from descriptions
- **Album Art Generation**: Automatic cover art creation using Stable Diffusion XL Turbo
- **Multiple Generation Modes**:
  - 📝 Simple Description Mode - Describe your song and AI handles the rest
  - ⚙️ Custom Mode - Provide prompts and lyrics for fine-tuned control
  - 🎤 Hybrid Mode - Mix descriptions with custom prompts
- **Credit System**: User-friendly credit-based generation with flexible purchase options
- **Music Discovery**: Browse trending songs and categorized music collections
- **Social Features**: Like songs, track play counts, and build your profile
- **Publish & Share**: Share your creations publicly with the community
- **Async Processing**: Queue-based music generation using Inngest for reliability
- **Secure Storage**: S3-backed storage with presigned URLs for secure access

## 🏗️ Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Frontend (Next.js 16)                    │
│  - React 19 Components  - TypeScript  - Tailwind CSS            │
│  - Better Auth (SSO)    - Zustand    - Inngest Events           │
└─────────────────┬───────────────────────────────────────────────┘
                  │
        ┌─────────┴──────────────┬──────────────┐
        │                        │              │
   ┌────▼────┐            ┌──────▼──────┐   ┌──▼────┐
   │PostgreSQL│            │ Inngest     │   │Polar  │
   │Database  │            │ (Queue)     │   │(Billing)
   └──────────┘            └──────┬──────┘   └───────┘
                                  │
                          ┌───────▼──────────┐
                          │  Modal Labs      │
                          │  (Python/GPU)    │
                          ├─ ACE-Step        │
                          ├─ Qwen2-7B        │
                          └─ SDXL Turbo      │
                                  │
                          ┌───────▼──────────┐
                          │  AWS S3          │
                          │  (Storage)       │
                          └──────────────────┘
```

### Key Components

| Component          | Purpose                         | Tech                       |
| ------------------ | ------------------------------- | -------------------------- |
| **Frontend**       | User interface and interactions | Next.js, React, TypeScript |
| **Backend API**    | Music generation endpoints      | Python, Modal, FastAPI     |
| **Database**       | User, song, and credit tracking | PostgreSQL, Prisma         |
| **Task Queue**     | Async generation workflow       | Inngest                    |
| **Storage**        | Audio and image files           | AWS S3                     |
| **Payment**        | Credit purchasing               | Polar                      |
| **Authentication** | User sessions and auth          | Better Auth                |

## 📋 Tech Stack

### Frontend

- **Framework**: Next.js 16.1.1
- **UI Library**: React 19.2.3
- **Language**: TypeScript 5.8.2
- **Styling**: Tailwind CSS 4.0.15 + PostCSS
- **UI Components**: Radix UI
- **State Management**: Zustand 5.0.10
- **Form Validation**: React Hook Form + Zod
- **Notifications**: Sonner
- **Database ORM**: Prisma 6.6.0
- **Authentication**: Better Auth 1.4.11
- **Task Queue**: Inngest 3.49.3
- **Cloud Storage**: AWS S3 SDK
- **Code Quality**: ESLint, Prettier, TypeScript

### Backend

- **Language**: Python 3
- **AI Models**:
  - **Music Generation**: ACE-Step
  - **Text-to-Speech/Prompt**: Qwen2-7B-Instruct
  - **Image Generation**: Stable Diffusion XL Turbo
- **Infrastructure**: Modal Labs (GPU Compute)
- **Audio Processing**: FFmpeg
- **Libraries**: transformers, diffusers, torch, torchaudio, boto3, pydantic

### Infrastructure

- **Database**: PostgreSQL
- **Storage**: AWS S3
- **Payment**: Polar
- **Compute**: Modal Labs (L40S GPU)
- **Deployment**: Vercel (Frontend), Modal (Backend)

## 🛠️ Setup & Installation

### Prerequisites

- Node.js 18+ and npm 11+
- Python 3.9+
- PostgreSQL 12+
- AWS Account (S3 access)
- Polar Account (payment processing)
- Modal Account (GPU compute)

### Frontend Setup

1. **Install Dependencies**

```bash
cd frontend
npm install
```

2. **Environment Configuration**

```bash
cp .env.example .env.local
```

Update `.env.local` with your credentials:

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/ai-music-gen

# Authentication
BETTER_AUTH_SECRET=your-secret-key

# Modal (AI Backend)
MODAL_KEY=your-modal-key
MODAL_SECRET=your-modal-secret

# AWS S3
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=us-east-1
S3_BUCKET_NAME=your-bucket-name

# Modal API Endpoints
GENERATE_FROM_DESCRIPTION=https://your-modal-app--generate-from-description.modal.run
GENERATE_FROM_DESCRIBED_LYRICS=https://your-modal-app--generate-with-described-lyrics.modal.run
GENERATE_WITH_LYRICS=https://your-modal-app--generate-with-lyrics.modal.run

# Polar (Payment)
POLAR_ACCESS_TOKEN=your-polar-token
POLAR_WEBHOOK_SECRET=your-webhook-secret
```

3. **Setup Database**

```bash
npm run db:push
npm run db:generate
```

4. **Start Development Server**

```bash
npm run dev
```

Visit `http://localhost:3000`

### Backend Setup

1. **Install Dependencies**

```bash
cd backend
pip install -r requirements.txt
```

2. **Set Environment Variables**

```bash
export MODAL_TOKEN_ID=your-modal-token-id
export MODAL_TOKEN_SECRET=your-modal-token-secret
export S3_BUCKET_NAME=your-bucket-name
export AWS_ACCESS_KEY_ID=your-access-key
export AWS_SECRET_ACCESS_KEY=your-secret-key
```

3. **Deploy to Modal**

```bash
modal deploy main.py
```

This will create FastAPI endpoints for:

- `POST /generate_from_description`
- `POST /generate_with_lyrics`
- `POST /generate_with_described_lyrics`

## 📖 Usage Guide

### Creating Music

#### Mode 1: Simple Description

1. Go to `/create`
2. Write a full song description (e.g., "Melancholic piano ballad with emotional vocals")
3. Toggle instrumental if needed
4. Click "Create"
5. AI generates prompt, lyrics, music, and cover art
6. Track generation status in real-time

#### Mode 2: Custom Generation

1. Go to `/create` → Switch to "Custom"
2. Write a music prompt (e.g., "Lo-fi hip hop, 90 BPM, jazzy chords")
3. Choose: Write lyrics yourself OR describe lyrics for AI to generate
4. Click "Create"
5. Music generates with your custom parameters

#### Mode 3: Instrumental

- Uncheck the vocal option in either mode
- AI generates music without lyrics

### Managing Credits

- **Starting Credits**: 100 per user
- **Cost per Song**: 1 credit (generates 2 versions: guidance scale 7.5 and 15.0)
- **Purchasing**: Go to Settings → Upgrade → Select plan
- **Plans**: Small, Medium, Large tiers via Polar

### Publishing & Sharing

1. Go to Dashboard (your songs list)
2. Click the menu on a song
3. Toggle "Publish"
4. Published songs appear on home page
5. Share the public link with others
6. Track plays and likes in real-time

### Browsing Music

- **Home**: Trending songs (last 2 days) + categorized collections
- **Categories**: Auto-generated based on music content
- **Trending**: Most recent quality songs
- **Like**: Save favorite songs to your library
- **Play**: Listen to any published song with play count tracking

## 🔄 Generation Pipeline

### Full Workflow

```
1. User submits generation request
   ↓
2. Server creates Song record (status: "queued")
   ↓
3. Inngest picks up event
   ↓
4. Credit check (must have ≥1 credit)
   ↓
5. Status: "processing"
   ↓
6. Modal Backend:
   a) If simple mode: Qwen2-7B generates music prompt
   b) If simple mode: Qwen2-7B generates lyrics (if not instrumental)
   c) ACE-Step generates audio (180 seconds)
   d) SDXL Turbo generates album cover
   e) FFmpeg converts WAV → MP4 (192 kbps AAC)
   ↓
7. Upload to S3
   ↓
8. Categories auto-generated by Qwen2-7B
   ↓
9. Update database:
   - S3 keys stored
   - Categories linked
   - Status: "processed"
   - Credits decremented by 1
   ↓
10. Available for play/sharing
```

### Generation Parameters

```
Audio Duration: 180 seconds
Inference Steps: 60
Guidance Scale: 7.5 and 15.0 (dual generation)
GPU: L40S (Modal)
Seed: Random or user-specified
Output Format: MP4 (AAC, 192 kbps)
Cover Art: PNG, 1024x1024
```

## 🗄️ Database Schema

### Core Models

**User**

- Authentication and credit tracking
- Relations: sessions, accounts, songs, likes

**Song**

- Music metadata and generation parameters
- S3 keys for audio and thumbnail
- Status tracking (queued, processing, processed, failed)
- Publication and engagement metrics

**Category**

- Auto-generated music genres/moods
- Automatic categorization based on content analysis

**Like**

- User-song relationship for favorites
- Composite primary key: (userId, songId)

**Session & Account**

- Better Auth models for authentication
- Support for email/password and OAuth providers

**Verification**

- Email verification tokens

See [prisma/schema.prisma](frontend/prisma/schema.prisma) for full schema.

## 🔐 Security

- **Authentication**: Session-based with Better Auth
- **Authorization**:
  - Users can only access their own songs
  - Published songs accessible to all
  - Play URLs expire in 1 hour
- **API Security**:
  - Modal endpoint authentication via MODAL_KEY and MODAL_SECRET
  - Polar webhook signature verification
- **Data Protection**:
  - Passwords hashed by Better Auth
  - S3 presigned URLs (time-limited access)
  - No direct S3 exposure

## 📊 Monitoring & Logging

- **Inngest Dashboard**: Monitor generation events and failures
- **Modal Logs**: GPU compute and model loading details
- **Database Logging**: PostgreSQL query logging in development
- **Error Tracking**: Failed generations marked in database
- **Analytics**: Play counts and engagement tracking

## 🚀 Deployment

### Frontend (Vercel)

```bash
# Push to main branch
git push origin main
# Vercel auto-deploys
```

### Backend (Modal)

```bash
# Deploy Python backend
modal deploy backend/main.py
# Get endpoint URLs
modal objects list
```

### Database Migrations

```bash
npm run db:migrate
```

## 📚 API Documentation

### Song Generation Endpoints (Modal)

**POST** `/generate_from_description`

```json
{
  "full_described_song": "Upbeat summer pop song",
  "instrumental": false,
  "audio_duration": 180,
  "seed": -1,
  "guidance_scale": 15.0,
  "infer_step": 60
}
```

**POST** `/generate_with_lyrics`

```json
{
  "prompt": "Lo-fi, chill, jazz",
  "lyrics": "[verse]\nLyric lines...\n[chorus]\n...",
  "audio_duration": 180,
  "guidance_scale": 15.0
}
```

**POST** `/generate_with_described_lyrics`

```json
{
  "prompt": "Electronic, upbeat",
  "described_lyrics": "Song about summer love",
  "instrumental": false,
  "audio_duration": 180
}
```

**Response** (All endpoints)

```json
{
  "s3_key": "audio-file-uuid.mp4",
  "cover_image_s3_key": "cover-uuid.png",
  "categories": ["Electronic", "Upbeat", "Summer"]
}
```

### Server Actions (Next.js)

**generateSong(request)**

- Creates song records and triggers async generation
- Queues both 7.5 and 15.0 guidance scales
- Returns immediately, processes in background

**getPlayUrl(songId)**

- Returns presigned S3 URL (1 hour expiry)
- Requires permission (owner or published)
- Increments play count

**getPresignedUrl(key)**

- Generic presigned URL generator
- Used for thumbnails and audio

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

### Code Standards

- Use TypeScript with strict mode
- Follow ESLint and Prettier configurations
- Write tests for new features
- Update documentation

## 🐛 Troubleshooting

### Common Issues

**Songs not generating?**

- Check Inngest dashboard for failures
- Verify Modal API endpoints in env variables
- Ensure S3 credentials are correct
- Check user credit balance

**Database connection errors?**

- Verify DATABASE_URL format
- Ensure PostgreSQL is running
- Check network connectivity to database

**S3 access issues?**

- Verify AWS credentials
- Check S3 bucket permissions
- Ensure bucket name matches env variable

**Modal API timeouts?**

- GPU might be loading large models (first time only)
- Check Modal dashboard for compute limits
- Increase Modal request timeout

## 📝 Environment Variables Reference

| Variable                         | Required | Description                          |
| -------------------------------- | -------- | ------------------------------------ |
| `DATABASE_URL`                   | ✓        | PostgreSQL connection string         |
| `BETTER_AUTH_SECRET`             | ✓        | Session encryption key               |
| `MODAL_KEY`                      | ✓        | Modal API key                        |
| `MODAL_SECRET`                   | ✓        | Modal API secret                     |
| `AWS_ACCESS_KEY_ID`              | ✓        | AWS S3 access key                    |
| `AWS_SECRET_ACCESS_KEY`          | ✓        | AWS S3 secret key                    |
| `AWS_REGION`                     | ✓        | AWS region (e.g., us-east-1)         |
| `S3_BUCKET_NAME`                 | ✓        | S3 bucket name                       |
| `GENERATE_FROM_DESCRIPTION`      | ✓        | Modal endpoint URL                   |
| `GENERATE_FROM_DESCRIBED_LYRICS` | ✓        | Modal endpoint URL                   |
| `GENERATE_WITH_LYRICS`           | ✓        | Modal endpoint URL                   |
| `POLAR_ACCESS_TOKEN`             | ✓        | Polar API token                      |
| `POLAR_WEBHOOK_SECRET`           | ✓        | Polar webhook signature secret       |
| `NODE_ENV`                       |          | Environment (development/production) |

## 📈 Performance Metrics

- **Music Generation**: 2-5 minutes per song
- **Cover Art Generation**: 5-10 seconds per image
- **Database Queries**: Sub-100ms (with indexes)
- **S3 Upload Speed**: Depends on file size and network
- **Generation Concurrency**: 1 song per user at a time

## 🎓 Learning Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Inngest Documentation](https://www.inngest.com/docs)
- [Better Auth Documentation](https://www.better-auth.com)
- [Modal Documentation](https://modal.com/docs)
- [Tailwind CSS](https://tailwindcss.com/docs)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Support

For issues and feature requests:

- Open an issue on GitHub
- Check existing documentation
- Review troubleshooting section
- Contact the maintainers

## 🙏 Acknowledgments

- ACE-Step team for the music generation model
- Alibaba Qwen team for the language model
- Stability AI for Stable Diffusion XL
- Modal Labs for GPU infrastructure
- Better Auth for authentication framework
- Polar for payment processing

---

**Made with ❤️ for music creators**
