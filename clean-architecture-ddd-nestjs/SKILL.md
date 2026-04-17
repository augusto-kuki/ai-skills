---

name: clean-architecture-ddd-nestjs
description: Enforces Clean Architecture with DDD-inspired modeling for NestJS backends
alwaysApply: true
tags: [backend, nestjs, architecture, ddd, clean-architecture]
--------------------------------------------------------------

# 🧱 Core Architectural Foundation

This backend uses **Clean Architecture** with **DDD-inspired modeling**.

Dependency direction is **MANDATORY**:

Infrastructure → Domain → Core

Violating this rule is unacceptable.

---

# 📦 Layer Responsibilities

## Core Layer (`src/core`)

Purpose: Shared primitives and cross-cutting foundations.

Allowed:

* Base entities (Entity, AggregateRoot, ValueObject)
* Either pattern
* Base error interfaces
* Utility types

Forbidden:

* Business logic
* Framework imports (NestJS, Prisma)
* Infrastructure details

Core must remain framework-agnostic forever.

---

## Domain Layer (`src/domain`)

Purpose: Pure business logic.

Allowed:

* Entities and value objects
* Use cases (application services)
* Repository interfaces (ports)
* Domain services
* Domain errors

Forbidden:

* NestJS decorators
* Prisma imports
* HTTP concepts
* Infrastructure concerns

The domain MUST be independently testable.

---

## Infrastructure Layer (`src/infra`)

Purpose: Frameworks, I/O, and adapters.

Allowed:

* Controllers
* ORM implementations (Prisma)
* HTTP presenters
* Validation pipes
* Authentication, cache, queues
* External integrations

Forbidden:

* Business logic
* Decision-making rules
* Domain mutations outside use cases

Infrastructure adapts — it does not decide.

---

# 🧬 Domain Modeling Rules

Entities:

* Must extend Entity<Props> or AggregateRoot<Props>
* Must have immutable identity (UniqueEntityID)
* Must expose behavior via methods
* Must NOT expose mutable props directly
* To update props: use setters on the entity; the use case calls them

Value Objects:

* Use only for fields with business rules
* Immutable
* Compared by value
* No identity
* Encapsulate validation

---

# 🎯 Use Case Rules (EXTREMELY STRICT)

* One class per use case
* One public execute() method
* Explicit request interface
* Output MUST be Either<Error, Success>
* No exceptions for business flow
* Dependencies injected via constructor

Follow Red-Green-Refactor:

1. Write failing test
2. Make it pass
3. Refactor

---

# 🚫 Controller Rules

Controllers must be THIN.

Controllers MAY:

* Validate input (Zod)
* Extract auth context
* Call a use case
* Map domain errors to HTTP
* Use presenters

Controllers MUST NOT:

* Contain business logic
* Access repositories directly
* Mutate domain entities

---

# 🧾 Validation Rules (Zod)

* Single source of truth for HTTP validation
* Use ZodValidationPipe
* Infer types from schemas
* Do NOT revalidate inside use cases

---

# 🗄️ Repository Rules

* Interfaces live in Domain
* Implementations live in Infrastructure
* Domain never imports implementations
* Persistence only

---

# 🧩 Prisma Governance

* Domain never imports Prisma
* Mappers convert Prisma ↔ Domain
* One mapper per entity
* Explicit null handling

---

# 🔁 Error Handling (Either)

* Use cases return Either
* No exceptions for control flow
* Controllers map errors to HTTP

---

# 🧪 Testing Constitution

* Vitest
* In-memory repositories
* Test use cases in isolation
* Tests live next to use cases

Unit test setup:

* Do NOT call other use cases
* Use factories (makeUser, etc.)
* Persist directly in in-memory repo

---

# 🧼 Anti-Entropy Rules

Never generate:

* Business logic in controllers
* God services
* Repository logic beyond persistence
* DTO duplication
* Framework leakage into domain
* Generic utils folders

---

# 🧠 Pattern Protection

Existing patterns are sacred.

Follow them strictly.

---

# 🎯 Goal

The domain is the product.

Everything else is replaceable.
