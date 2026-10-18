# Backend Architecture

The PawPrint backend follows **Clean Architecture**: the code is split into layers, and dependencies point **inward only**.

## Layers

```
  ┌─────────────────────┐          ┌─────────────────────────────┐
  │    PawPrint.Api     │─────────►│   PawPrint.Infrastructure   │
  │   (Presentation)    │          │ (Data access & ext services)│
  └──────────┬──────────┘          └──────────────┬──────────────┘
             │                                    │
             │                                    │
             ▼                                    │
  ┌─────────────────────┐                         │
  │ PawPrint.Application│◄────────────────────────┘
  │     (Use cases)     │
  └──────────┬──────────┘
             │
             ▼
  ┌─────────────────────┐
  │   PawPrint.Domain   │
  │   (Business model)  │
  └─────────────────────┘

  A ───► B  means "A references B"
```

```
        ┌──────────────────────────────────────────┐
        │  Api  (controllers, middleware, DI)      │
        │   ┌──────────────────────────────────┐   │
        │   │  Infrastructure (EF Core, repos) │   │
        │   │   ┌──────────────────────────┐   │   │
        │   │   │  Application (use cases) │   │   │
        │   │   │   ┌──────────────────┐   │   │   │
        │   │   │   │      Domain      │   │   │   │
        │   │   │   └──────────────────┘   │   │   │
        │   │   └──────────────────────────┘   │   │
        │   └──────────────────────────────────┘   │
        └──────────────────────────────────────────┘
              dependencies point inward  ──►
```

## Dependency rule

| Project | References | Must not reference |
|---|---|---|
| `PawPrint.Domain` | nothing | everything else (no EF Core, no ASP.NET) |
| `PawPrint.Application` | Domain | Infrastructure, Api |
| `PawPrint.Infrastructure` | Application | Api |
| `PawPrint.Api` | Application, Infrastructure | — |

Application **declares** interfaces it needs (e.g. `IPetRepository`, `IEmailSender`); Infrastructure **implements** them. Api is the composition root and wires implementations together in `Program.cs`.

## What goes where

| Layer | Contents | Examples |
|---|---|---|
| Domain | Entities, value objects, enums, domain rules | `Pet`, `Owner`, `Appointment`, `Species` |
| Application | Use cases, interfaces, DTOs, validation | `CreatePet`, `GetPetById`, `IPetRepository` |
| Infrastructure | Persistence, external services | `AppDbContext`, migrations, `PetRepository` |
| Api | HTTP endpoints, middleware, auth, DI wiring | `PetsController`, exception middleware |

Within Domain and Application, organise folders **by feature** (`Pets/`, `Appointments/`) rather than by type.

## Request flow

```
 Client        Api              Application        Domain        Infrastructure       Database
   │      (PetsController)      (CreatePet)         (Pet)       (PetRepository)          │
   │              │                  │                │                │                 │
   │ POST /pets   │                  │                │                │                 │
   │─────────────►│                  │                │                │                 │
   │              │ CreatePet(req)   │                │                │                 │
   │              │─────────────────►│                │                │                 │
   │              │                  │ new Pet(...)   │                │                 │
   │              │                  │───────────────►│ enforce rules  │                 │
   │              │                  │◄───────────────│                │                 │
   │              │                  │ IPetRepository.Add(pet)         │                 │
   │              │                  │────────────────────────────────►│     INSERT      │
   │              │                  │                │                │────────────────►│
   │              │                  │                │                │◄────────────────│
   │              │                  │◄────────────────────────────────│                 │
   │              │      PetDto      │                │                │                 │
   │              │◄─────────────────│                │                │                 │
   │ 201 Created  │                  │                │                │                 │
   │◄─────────────│                  │                │                │                 │
```

## Folder layout

```
src/backend/
├── PawPrint.slnx
├── src/
│   ├── PawPrint.Domain/
│   ├── PawPrint.Application/
│   ├── PawPrint.Infrastructure/
│   └── PawPrint.Api/
└── tests/
    ├── PawPrint.Domain.Tests/
    ├── PawPrint.Application.Tests/
    └── PawPrint.Api.Tests/        # integration tests
```
