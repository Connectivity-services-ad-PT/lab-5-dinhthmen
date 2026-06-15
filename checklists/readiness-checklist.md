# Readiness Checklist - Lab 05

- [x] Database ready: PostgreSQL container responds to `pg_isready`.
- [x] AI service ready: AI container returns `200` for `/health` and `/predict` works.
- [x] API ready: API container returns `200` for `/health`.
- [x] API dependency readiness: API `/readiness` confirms DB and AI are reachable on `team-internal`.
- [x] Environment variables: `.env.example` defines API, DB, AI, token, port, and version settings with no real secrets.
- [x] Network and ports: `team-internal` and `class-net` are defined; ports 8000, 9000, and 5432 are mapped.
- [x] Healthchecks: API, AI, and DB have Compose healthchecks.
- [x] Newman evidence: `npm run test:compose` generates XML/HTML reports in `reports/`.

## Evidence

Verified locally on 2026-06-15 with the same API, AI, and DB images/ports:

```text
docker build -t fit4110/iot-ingestion:lab05 .
docker build -f Dockerfile.ai -t fit4110/ai-service:lab05 .
docker run -d --rm --name fit4110-db-lab05 -p 5432:5432 postgres:15-alpine
docker run -d --rm --name fit4110-ai-lab05 -p 9000:9000 fit4110/ai-service:lab05
docker run -d --rm --name fit4110-api-lab05 -p 8000:8000 fit4110/iot-ingestion:lab05
curl http://localhost:8000/health
curl http://localhost:8000/readiness
curl http://localhost:9000/health
npm run test:compose
```

GitHub Actions validates the actual `docker compose up -d --build --wait` path.

Image tag used locally: `fit4110/iot-ingestion:lab05`.
Registry push is left for the selected registry/owner.
