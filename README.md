# Pathfinder

An interactive pathfinding visualizer with a Go API and a Svelte 5 / SvelteKit frontend. Compare A*, Dijkstra, and breadth-first search as they explore a grid and find a route around walls.

[Source](https://github.com/cassiokiyoshi/pathfinder)

## Features

- Draw and erase walls, and move the start and goal cells.
- Stream visited cells and the final path from the API using newline-delimited JSON (NDJSON).
- Adjust animation speed, pause/resume, or stop a search.
- Compare visited-cell counts, path length, and server search duration.
- Clear a path, clear walls, or reset the grid.

## Requirements

- Go 1.27.1 or newer, as declared in `api/go.mod`.
- Node.js 22.12+ and npm (the frontend uses Vite 8).

## Run locally

From the project root, start the API:

```sh
cd api
go run ./cmd/server
```

In a second terminal, from the project root:

```sh
cd frontend
npm ci
npm run dev
```

Open the URL printed by Vite, normally http://localhost:5173. The API listens on port 8080; Vite proxies `/api` requests to it. Keep both processes running.

Draw walls, choose an algorithm, and click **Run search**. Use the animation slider to change playback speed. The reported duration measures server search time, not animation playback time.

## Checks and build

From `api/`:

```sh
go test ./...
go build ./cmd/server
```

From `frontend/`:

```sh
npm run check
npm run build
```

The frontend uses `adapter-auto`. Production hosting needs an appropriate SvelteKit adapter and routing of `/api/search` to the Go service on the same origin. The development proxy alone does not configure production routing. GitHub Pages cannot run the Go API.

## API

- `GET /health`: returns `ok`.
- `POST /api/search`: accepts a grid and algorithm; streams `visited`, `path` (when found), and `complete` events. Errors use an `error` event.

Example, with the API running:

```sh
curl -N http://localhost:8080/api/search \
  -H 'Content-Type: application/json' \
  -d '{"algorithm":"astar","rows":5,"cols":5,"start":{"row":0,"col":0},"goal":{"row":4,"col":4},"walls":[{"row":1,"col":1}]}'
```

Positions use zero-based `row` and `col` coordinates. Supported algorithm values are `astar`, `dijkstra`, and `bfs`.

## Structure

- `api/cmd/server/`: HTTP server entry point.
- `api/internal/httpapi/`: request decoding and streaming responses.
- `api/internal/pathfinder/`: algorithms, grid validation, and Go tests.
- `frontend/src/lib/`: grid helpers, streaming client, and UI components.
- `frontend/src/routes/`: page and layout.
