# CVR/FDR Analyzer

A single-page React application for aviation investigators to review cockpit voice recorder (CVR) and flight data recorder (FDR) cases. The dashboard surfaces high-level activity, provides quick entry points to case work, and links to focused tooling for timeline review, audio playback, correlation, and reporting.

## Features

- **Investigator dashboard** – Visualizes monthly incident/accident trends, maps case locations, and highlights recently accessed cases for quick follow-up.
- **Case workspace** – Navigate through pending investigations, drill into case details, and jump directly to CVR, FDR, correlation, and reporting tools.
- **Data correlation tools** – Dedicated views for pairing CVR transcripts with FDR parameters and producing shareable reports.
- **Responsive layout** – Built with Tailwind CSS and Lucide icons for a polished experience on desktop and tablet form factors.

## Prerequisites

- Docker Desktop (Windows/macOS) or Docker Engine + Docker Compose plugin (Linux)

## Running the full stack with Docker (recommended baseline)

1. Build and start all services (frontend, backend, PostgreSQL, MinIO):

```bash
docker compose up --build
```

2. Open the apps:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:4000
   - MinIO Console: http://localhost:9001 (user: `minioadmin`, password: `minioadmin`)

3. Stop everything:

```bash
docker compose down
```

4. Reset database/object storage volumes when needed:

```bash
docker compose down -v
```

The backend connects to PostgreSQL and MinIO via internal Docker service names (`db` and `minio`). Bucket bootstrapping is handled automatically by the `minio-setup` service.

## Optional local (non-Docker) workflow

If you intentionally need local tooling, copy `server/.env.example` to `server/.env` and configure `DATABASE_URL`/MinIO values for your host machine. Docker is now the default and expected development workflow.

## Building for production

Create an optimized production bundle in the `build/` directory:

```bash
npm run build
```

The output bundle is tree-shaken, minified, and ready to be deployed to your hosting platform of choice.

## Testing

Run the Create React App test runner in watch mode:

```bash
npm test
```

## Project structure

```
├── public/                # Static assets served as-is by CRA
├── server/                # Express + PostgreSQL REST API
│   ├── db/                # SQL schema and migration helpers
│   └── src/               # Server source (routes, services, middleware)
├── src/
│   ├── api/               # REST client helpers for the React app
│   ├── components/        # Shared React components (e.g., map visualizations)
│   ├── pages/             # Feature-specific screens (dashboard, CVR, FDR, etc.)
│   ├── App.js             # Main application shell and navigation
│   └── index.js           # React entry point
├── install-requirements.sh# Dependency installation helper script
├── package.json           # npm metadata and dependency list
├── docs/                  # Markdown documentation (e.g., architecture diagrams)
└── README.md              # Project documentation (this file)
```

## Viewing the documentation

Architecture notes and diagrams live in the `docs/` folder as Markdown files. You can view them directly in GitHub or any IDE
with Markdown preview support. For Mermaid diagrams, export a shareable image or PDF with the Mermaid CLI:

```bash
npx -y @mermaid-js/mermaid-cli -i docs/cvr-fdr-workflow-diagram.md -o docs/cvr-fdr-workflow.png
```

The command above writes a rendered diagram to `docs/cvr-fdr-workflow.png`. Feel free to change the output file extension (for
example, `.pdf`) to match the format you need.

## Available scripts

| Command | Description |
| ------- | ----------- |
| `docker compose up --build` | Builds and starts frontend, backend, PostgreSQL, MinIO, and MinIO bucket bootstrap. |
| `docker compose down` | Stops the full containerized stack. |
| `docker compose down -v` | Stops the stack and removes persisted Docker volumes. |
| `npm test` | Launches the React unit test runner (for local test execution). |
| `npm run build` | Generates a production-optimized frontend bundle. |

## Troubleshooting

- **Port already in use**: The CRA dev server defaults to port 3000. Set `PORT=3001` (or another free port) before running `npm start` to use a different port.
- **Dependency issues**: Delete the `node_modules/` directory and rerun `./install-requirements.sh`.
- **Unsupported Node.js version**: Upgrade to the latest Active LTS release of Node.js.
- **"Unexpected server error" responses**: Run `npm run server` in a terminal to keep the API logs visible. The backend now prints the stack trace for unhandled errors and, when `NODE_ENV` is not set to `production`, also returns the specific error message in the JSON response (`details` field) so you can see the root cause directly in the browser network tab.

## License

This project is the intellectual property of the Aviation Center of Excellence (ACoE), developed in collaboration with the General Civil Aviation Authority (GCAA) and the University of Sharjah.

All rights are reserved. This repository is private and not open to the public.
Redistribution, reproduction, or use of the code—whether in whole or in part—is strictly prohibited without prior written permission from the project maintainers or the affiliated institutions.
