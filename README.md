# Advanced TTS Reader (FastAPI + Flutter)

## Project Structure
- `backend/`: FastAPI API, PostgreSQL models, Redis queue worker, OCR/PDF/TTS pipeline.
- `frontend/`: Flutter mobile client with upload, recent files, and streaming audio player.

## Backend Setup
```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```

Run worker:
```bash
python -m app.workers.queue_worker
```

## Required environment variables
- `DATABASE_URL=postgresql://...`
- `REDIS_URL=redis://...`
- `OPENAI_API_KEY=...`
- `UPLOAD_DIR=uploads`
- `AUDIO_DIR=audio`

## Railway Deployment
1. Create a Railway project and link this repo.
2. Add PostgreSQL service and set `DATABASE_URL` from Railway reference.
3. Add Redis service and set `REDIS_URL`.
4. Set `OPENAI_API_KEY`.
5. Deploy `backend` service with start command:
   - `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
6. Create a second Railway service for worker using:
   - `python -m app.workers.queue_worker`
7. Configure persistent volume if you need durable uploaded/audio files.

## Frontend Setup
```bash
cd frontend
flutter pub get
flutter run
```
Set `baseUrl` in `lib/main.dart` to your deployed backend URL.
