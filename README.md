# Virtufin WorkManager

[![Build Status](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/actions/workflows/docs/badge.svg?branch=master)](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/actions)
[![NuGet](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/actions/workflows/nuget/badge.svg?branch=master)](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/packages/nuget/Virtufin.WorkManager.Client)
[![PyPI](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/actions/workflows/pypi/badge.svg?branch=master)](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/packages/pypi/virtufin-workmanager)
[![npm](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/actions/workflows/npm/badge.svg?branch=master)](https://git.haenerconsulting.com/virtufin/virtufin-workmanager/packages/npm/virtufin-workmanager)

📖 Documentation: [workmanager.doc.virtufin.com](https://workmanager.doc.virtufin.com)

Worker lifecycle management and code execution service with Dapr integration.

## Features

- Worker lifecycle management (create, start, stop, delete)
- Code execution engines: Python, C# Source, DotNet DLL
- Pub/sub-based worker invocation via Dapr
- State store persistence for worker configurations and history
- gRPC API with reflection

## Project Structure

```
virtufin-workmanager/
├── src/
│   ├── Virtufin.WorkManager/              # .NET service (gRPC server, worker engine)
│   ├── Virtufin.WorkManager.Client/       # .NET client NuGet package
│   ├── Virtufin.WorkManager.Protos/       # Protobuf definitions (workmanager.proto)
│   ├── Virtufin.Worker.DevKit/            # Worker base classes (WorkerBase, CommandWorker)
│   ├── Virtufin.WorkManager.Engines/      # Engine registry
│   ├── Virtufin.WorkManager.Engine.*/     # Engine implementations (Python, CSharpSource, DotNetDll)
│   ├── python/virtufin/workmanager/       # Python client library
│   └── typescript/src/                    # TypeScript client library
├── tests/
│   ├── python/                            # Python client tests
│   ├── typescript/                        # TypeScript client tests
│   └── Virtufin.WorkManager.*.Tests/      # .NET tests
├── docs/                                  # MkDocs documentation
├── deploy/                                # Deployment configs
├── versions.env                           # Version pin
└── AGENTS.md                              # Agent documentation
```

## Deployment

All deployment infrastructure is managed in dedicated repositories.

### Local (Native Dapr)

Run services natively with Dapr sidecars. No Docker containers for services. Requires Dapr CLI, .NET SDK, and Redis.

```bash
git clone https://git.haenerconsulting.com/virtufin/deploy-local.git
cd deploy-local
./scripts/deploy_workmanager.sh
```

See [deploy-local](https://git.haenerconsulting.com/virtufin/deploy-local) for full documentation.

### Docker Dev

Build and run from source via Docker Compose.

```bash
git clone https://git.haenerconsulting.com/virtufin/docker-compose.git
cd docker-compose
./scripts/deploy_workmanager.dev.sh
```

### Docker Prod

Run pre-built Harbor images via Docker Compose.

```bash
git clone https://git.haenerconsulting.com/virtufin/docker-compose.git
cd docker-compose
./scripts/deploy_workmanager.prod.sh
```

See [docker-compose](https://git.haenerconsulting.com/virtufin/docker-compose) for full documentation.

### Kubernetes

Deploy with the Virtufin Helm chart. Requires Dapr installed on the cluster.

```bash
helm repo add virtufin https://helm.haenerconsulting.com/virtufin/helm
helm install virtufin virtufin/virtufin \
  --namespace virtufin \
  --create-namespace \
  --set stateStore.host=<redis-host> \
  --set pubsub.host=<redis-host>
```

See [helm](https://helm.haenerconsulting.com/virtufin/helm) for all configurable values.

## API

| Protocol | Port | Description |
|----------|------|-------------|
| HTTP | 25001 | Health checks, management |
| gRPC | 25002 | Worker lifecycle operations |

## Development

### Prerequisites

- .NET 10.0 SDK or later
- Dapr CLI (for local development)

### Build

```bash
dotnet build
```

### Test

```bash
dotnet test
```

## License

Proprietary - Virtufin
