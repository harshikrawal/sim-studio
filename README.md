

<p align="center">
  <a href="https://sim.ai" target="_blank" rel="noopener noreferrer">
    <img src="apps/sim/public/static/readme-banner.png" alt="Sim — Integrate, Context, Build, and Monitor AI agents" width="100%"/>
  </a>
</p>

<p align="center">A workspace to build, deploy and manage AI agents and workflows.</p>

## Quickstart

```bash
npx simstudio
```

Open [http://localhost:3000](http://localhost:3000)

Docker must be installed and running. Use `-p, --port <port>` to run Sim on a different port, or `--no-pull` to skip pulling the latest Docker images.

<p align="center">
  <img src="apps/sim/public/static/readme-platform.png" alt="The Sim platform — chat on the left, the visual workflow builder on the right" width="100%"/>
</p>

## Capabilities

- Connect 1,000+ integrations and every major LLM
- Add Slack, Notion, HubSpot, Salesforce, databases, and more
- Build agents visually, conversationally, or with code
- Ingest files, knowledge bases, and structured table data
- Monitor runs, logs, schedules, and workflow activity

## One workspace, every surface

<p align="center">Chat and workflows are just the start — tables, files, knowledge, and scheduled tasks all live in the same workspace.</p>

<table>
  <tr>
    <td width="50%" valign="top">
      <img src="apps/sim/public/static/readme-tables.png" alt="Tables in Sim — structured data your agents can query" width="100%"/>
      <p align="center"><b>Tables</b> — a database, built in</p>
    </td>
    <td width="50%" valign="top">
      <img src="apps/sim/public/static/readme-files.png" alt="Files in Sim — documents for your team and every agent" width="100%"/>
      <p align="center"><b>Files</b> — one store for your team and every agent</p>
    </td>
  </tr>
  <tr>
    <td width="50%" valign="top">
      <img src="apps/sim/public/static/readme-knowledge.png" alt="Knowledge bases in Sim — synced docs your agents can search" width="100%"/>
      <p align="center"><b>Knowledge</b> — your agents' memory</p>
    </td>
    <td width="50%" valign="top">
      <img src="apps/sim/public/static/readme-scheduled-tasks.png" alt="Scheduled tasks in Sim — recurring agent runs on a calendar" width="100%"/>
      <p align="center"><b>Scheduled tasks</b> — runs on your schedule</p>
    </td>
  </tr>
</table>

## Self-hosting

### Docker Compose

```bash
git clone https://github.com/harshikrawal/sim-studio && cd sim
docker compose -f docker-compose.prod.yml up -d
```

Open [http://localhost:3000](http://localhost:3000)

Sim also supports local models via [Ollama](https://ollama.ai) and [vLLM](https://docs.vllm.ai/). See the [Docker self-hosting docs](https://docs.sim.ai/self-hosting/docker) for setup details.

### Manual Setup

**Requirements:** [Bun](https://bun.sh/), [Node.js](https://nodejs.org/) v20+, PostgreSQL 12+ with [pgvector](https://github.com/pgvector/pgvector)

1. Clone and install:

```bash
git clone https://github.com/harshikrawal/sim-studio
cd sim
bun install
bun run prepare  # Set up pre-commit hooks
```

2. Set up PostgreSQL with pgvector:

```bash
docker run --name simstudio-db -e POSTGRES_PASSWORD=your_password -e POSTGRES_DB=simstudio -p 5432:5432 -d pgvector/pgvector:pg17
```

Or install manually via the [pgvector guide](https://github.com/pgvector/pgvector#installation).

3. Configure environment:

```bash
cp apps/sim/.env.example apps/sim/.env
# Create your secrets
perl -i -pe "s/your_encryption_key/$(openssl rand -hex 32)/" apps/sim/.env
perl -i -pe "s/your_internal_api_secret/$(openssl rand -hex 32)/" apps/sim/.env
perl -i -pe "s/your_api_encryption_key/$(openssl rand -hex 32)/" apps/sim/.env
# DB configs for migration
cp packages/db/.env.example packages/db/.env
# Edit both .env files to set DATABASE_URL="postgresql://postgres:your_password@localhost:5432/simstudio"
```

4. Run migrations:

```bash
cd packages/db && bun run db:migrate
```

5. Start development servers:

```bash
bun run dev:full  # Starts Next.js app and realtime socket server
```

Or run separately: `bun run dev` (Next.js) and `cd apps/sim && bun run dev:sockets` (realtime).

## Chat API Keys

Chat is a Sim-managed service. To use Chat on a self-hosted instance:

- Set `COPILOT_API_KEY` environment variable in your self-hosted apps/sim/.env file to that value

## Environment Variables

See the [environment variables reference](https://docs.sim.ai/self-hosting/environment-variables) for the full list, or [`apps/sim/.env.example`](apps/sim/.env.example) for defaults.

## Tech Stack

<details>
<summary>Next.js · Bun · PostgreSQL · Drizzle · Better Auth · Tailwind — and the rest of the stack</summary>

- **Framework**: [Next.js](https://nextjs.org/) (App Router)
- **Runtime**: [Bun](https://bun.sh/)
- **Database**: PostgreSQL with [Drizzle ORM](https://orm.drizzle.team)
- **Authentication**: [Better Auth](https://better-auth.com)
- **Schema Validation**: [Zod](https://zod.dev)
- **UI**: [Shadcn](https://ui.shadcn.com/), [Tailwind CSS](https://tailwindcss.com)
- **Streaming Markdown**: [Streamdown](https://github.com/vercel/streamdown)
- **State Management**: [Zustand](https://zustand-demo.pmnd.rs/), [TanStack Query](https://tanstack.com/query)
- **Flow Editor**: [ReactFlow](https://reactflow.dev/)
- **Docs**: [Fumadocs](https://fumadocs.vercel.app/)
- **Monorepo**: [Turborepo](https://turborepo.org/)
- **Realtime**: [Socket.io](https://socket.io/)
- **Background Jobs**: [Trigger.dev](https://trigger.dev/)
- **Remote Code Execution**: [E2B](https://www.e2b.dev/)
- **Isolated Code Execution**: [isolated-vm](https://github.com/laverdet/isolated-vm)

</details>

## Contributing

We welcome contributions! Please see our [Contributing Guide](.github/CONTRIBUTING.md) for details.


