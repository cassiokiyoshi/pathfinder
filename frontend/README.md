# Pathfinder frontend

Svelte 5, SvelteKit, and TypeScript interface for the Pathfinder Go API.

See the [project README](../README.md) for requirements, API details, controls, and full setup.

## Development

Start the Go API in a separate terminal with `cd ../api && go run ./cmd/server`, then run from this directory:

```sh
npm ci
npm run dev
```

Open the URL printed by Vite. Requests to `/api` are proxied to http://localhost:8080 during development.

## Validate and build

```sh
npm run check
npm run build
```

`npm run preview` previews the production frontend build. Production requires a suitable SvelteKit adapter and same-origin routing to the Go API; see the project README.
