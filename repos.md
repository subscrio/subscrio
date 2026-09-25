# Subscrio repositories

| Repository | Purpose |
| --- | --- |
| [subscrio/subscrio](https://github.com/subscrio/subscrio) | Hub, cross-cutting issues |
| [subscrio/.github](https://github.com/subscrio/.github) | Organization profile page (`profile/README.md`) |
| [subscrio/subscrio-typescript](https://github.com/subscrio/subscrio-typescript) | TypeScript core (npm `subscrio`) |
| [subscrio/subscrio-dotnet](https://github.com/subscrio/subscrio-dotnet) | .NET core (NuGet `Subscrio.Core`) |
| [subscrio/subscrio-extensions-audit-log](https://github.com/subscrio/subscrio-extensions-audit-log) | Audit log extension |
| [subscrio/subscrio-extensions-payments](https://github.com/subscrio/subscrio-extensions-payments) | Payments extension |
| [subscrio/subscrio-abp](https://github.com/subscrio/subscrio-abp) | ABP integration (NuGet `Subscrio.Abp`) |
| [subscrio/docs](https://github.com/subscrio/docs) | Documentation site |
| [subscrio/samples](https://github.com/subscrio/samples) | Public runnable examples for how-to articles; sample index tracks readiness |
| [subscrio/server](https://github.com/subscrio/server) | Web admin / server (private) |
| [subscrio/website](https://github.com/subscrio/website) | Marketing site (private) |
| [subscrio/internal](https://github.com/subscrio/internal) | Release scripts and demo data (private) |
| [subscrio/business](https://github.com/subscrio/business) | Business docs (private) |

## Local workspace layout

`core/`, `extensions/`, and `integrations/` are **local grouping folders only** — not git repos. Each row below is one git repository checked out at the path shown.

```
<workspace>/
├── .github/               # org profile (subscrio/.github)
├── subscrio/              # hub
├── core/
│   ├── typescript/        # subscrio-typescript
│   └── dotnet/            # subscrio-dotnet
├── extensions/
│   ├── audit-log/         # subscrio-extensions-audit-log
│   │   ├── typescript/
│   │   ├── dotnet/
│   │   └── README.md
│   └── payments/          # subscrio-extensions-payments
│       ├── typescript/
│       ├── dotnet/
│       └── README.md
├── integrations/
│   └── abp/               # subscrio-abp
├── docs/
├── samples/
├── website/
├── server/
├── internal/
└── business/
```

## Where to file issues

- Package-specific bugs: file in that package repository.
- Cross-cutting domain features: file in [subscrio/subscrio](https://github.com/subscrio/subscrio/issues).
- Not sure: file in the hub; maintainers will transfer.

## Local development notes

- **Server** and **extension** packages use `file:` paths to `core/typescript` and sibling extension folders. Check out the full layout above before `npm install` in `server/` or extension `typescript/` folders.
- **Extension .NET** projects reference `core/dotnet` via relative `ProjectReference` paths. Build core before extension tests when working locally.
- **ABP integration** uses the sibling `core/dotnet` project in the full workspace and the matching published `Subscrio.Core` package when cloned by itself.
- **PostgreSQL** is required for core, extension, and server tests. Set `TEST_DATABASE_URL` or use `.env` as described in each package's test README.
- **Server tests** must keep `SENDGRID_ENABLED=false` in `server/tests/.env`.

## Test commands (from each package root)

```bash
# TypeScript core
cd core/typescript && npm install && npm run typecheck && npm run build && npm test

# .NET core
cd core/dotnet && dotnet build Subscrio.Core.sln -c Release && dotnet test Subscrio.Core.sln -c Release --no-build

# Extensions (TypeScript)
cd extensions/audit-log/typescript && npm install && npm test
cd extensions/payments/typescript && npm install && npm test

# Extensions (.NET)
cd extensions/audit-log/dotnet && dotnet test
cd extensions/payments/dotnet && dotnet test

# ABP integration
cd integrations/abp && dotnet test Subscrio.Abp.Sample.slnx -c Release

# Server
cd server && npm install && npm run build && npm test

# Docs
cd docs && pip install -r requirements.txt mkdocs-material && python -m mkdocs build

# Website
cd website && npm install && npm run build
```
