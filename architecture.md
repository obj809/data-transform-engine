Browser (simple UI)
  │ 1) POST /upload (multipart)
  ▼
API (FastAPI/Express)
  - saves file to /data/in/
  - creates job_id
  - pushes {job_id, input_path, preset} to Redis queue
  - returns job_id
  │
  │ 2) GET /jobs/:id (poll)
  ▼
Redis
  - queue (list/stream)
  - job status hash/map (queued/running/done/failed, progress)
  ▲
  │ 3) Worker blocks on queue
  │
Worker (Docker)
  - reads input from shared volume
  - runs ffmpeg -> /data/out/
  - updates Redis status/progress
  ▼
Shared Docker volume (/data)
  /data/in/<job_id>.mp4
  /data/out/<job_id>_compressed.mp4

Browser
  4) GET /download/:job_id  -> streams /data/out file