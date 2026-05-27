# GitHub Copilot Instructions
# Setup Done Right — .NET · Angular · React · Azure
# Shared by: https://www.youtube.com/@AIWithSandeep | github.com/[your-repo]
# ─────────────────────────────────────────────────────────

## Project Overview

This is a full-stack enterprise application. GitHub Copilot must understand
the complete tech stack and apply the correct conventions per context.

**Stack at a glance:**
| Layer        | Technology                              |
|-------------|------------------------------------------|
| Backend API  | ASP.NET Core Web API (.NET 8 / .NET 9)  |
| Frontend 1   | Angular 17+ (Standalone Components)     |
| Frontend 2   | React 18+ (TypeScript + Hooks)          |
| Cloud        | Microsoft Azure                         |
| IDE          | Visual Studio 2026 / VS Code            |
| Auth         | Azure AD / Microsoft Entra ID           |

---

## 1. .NET CORE API — RULES & CONVENTIONS

### Architecture
- Follow **Clean Architecture**: Api → Application → Domain → Infrastructure
- Use **Repository Pattern** with **Unit of Work**
- **CQRS with MediatR** for commands and queries
- **Dependency Injection** always — never use `new` for services

### C# Code Style
- Use **C# 12+** features: primary constructors, collection expressions, required members
- Always use `async/await` for every I/O operation — no `.Result` or `.Wait()`
- Return `ActionResult<T>` or `IResult` (Minimal API) from controllers
- Use `record` types for DTOs and value objects
- Use `sealed` on classes that should not be inherited
- Prefer `IReadOnlyList<T>` over `List<T>` in return types
- Use **guard clauses** at the top of methods — fail fast
- Never use `var` unless the type is obvious from the right-hand side
- No magic numbers — use `const` or configuration values

### Error Handling
- Use **Problem Details (RFC 7807)** for all error responses
- Use a global exception middleware — never catch and swallow exceptions in controllers
- Use `FluentValidation` for all request model validation
- Return `Result<T>` pattern from Application layer (do not throw for business errors)

### Data Access (Entity Framework Core)
- **Code First** migrations — always
- Use `AsNoTracking()` for read-only queries
- Use `IQueryable` inside repositories, never expose it to Application layer
- Never use `Include()` chains more than 2 levels deep — use projections instead
- Index foreign keys and frequently filtered columns
- Use `ValueConverter` for domain enums stored as strings

### Testing (.NET)
- **xUnit** as the test framework
- **Moq** for mocking
- **FluentAssertions** for assertions
- **WebApplicationFactory** for integration tests
- Test naming: `MethodName_Scenario_ExpectedBehavior`
- Aim for **80% coverage** on Application and Domain layers

### Naming Conventions
```
Controllers     → ProductsController (plural noun + Controller)
Services        → IProductService / ProductService
Repositories    → IProductRepository / ProductRepository
DTOs            → CreateProductRequest / ProductResponse
Commands        → CreateProductCommand / CreateProductCommandHandler
Queries         → GetProductByIdQuery / GetProductByIdQueryHandler
Entities        → Product, Order (singular, no suffix)
DbContext       → AppDbContext
```

### Folder Structure
```
src/
├── Api/
│   ├── Controllers/
│   ├── Middleware/
│   ├── Extensions/
│   └── Program.cs
├── Application/
│   ├── Features/
│   │   └── Products/
│   │       ├── Commands/
│   │       ├── Queries/
│   │       └── Validators/
│   ├── Interfaces/
│   └── Common/
├── Domain/
│   ├── Entities/
│   ├── Enums/
│   ├── Events/
│   └── Exceptions/
└── Infrastructure/
    ├── Persistence/
    │   ├── Configurations/
    │   ├── Migrations/
    │   └── AppDbContext.cs
    ├── Repositories/
    └── Services/
```

---

## 2. ANGULAR — RULES & CONVENTIONS

### Architecture
- Use **Standalone Components** (Angular 17+) — no NgModules unless legacy
- Use **Signals** for component-level state (Angular 17+)
- Use **NgRx** (with createFeature + createReducer) for global/shared state
- Use **Angular Router** with lazy loading for all feature routes
- Use **Functional Route Guards** — not class-based

### TypeScript Style
- **Strict mode** always (`"strict": true` in tsconfig)
- No `any` — use `unknown` and narrow the type
- Use `readonly` for properties that should not be mutated
- Use TypeScript `interface` for data shapes, `type` for unions/aliases
- Use `inject()` function instead of constructor injection

### Component Rules
- **OnPush** change detection on every component — no exceptions
- Suffix: `ProductListComponent`, `ProductCardComponent`
- Smart (container) components handle data; Dumb (presentational) handle display only
- Max component template: 100 lines — extract child components if exceeded
- Use `@defer` for heavy or below-fold content blocks

### Services
- Provided at root level (`providedIn: 'root'`) unless feature-scoped
- Use `HttpClient` with typed responses — `http.get<Product[]>(...)`
- Use `takeUntilDestroyed()` to auto-unsubscribe (Angular 16+)
- Centralize API base URLs in environment files — never hardcode

### RxJS
- Prefer `switchMap` for search/autocomplete (cancel previous)
- Prefer `concatMap` for sequential operations (order matters)
- Prefer `mergeMap` for parallel fire-and-forget
- Use `catchError` inside inner observables — never at the root stream
- Always complete streams in ngOnDestroy or use `takeUntilDestroyed()`

### Testing (Angular)
- **Angular Testing Library** + **Jest** (preferred) or Jasmine
- Test components through user behavior, not internals
- Use `MockProvider` or `provideHttpClientTesting` for unit tests
- E2E: **Playwright** (preferred over Cypress)

### Naming Conventions
```
Components  → product-list.component.ts → ProductListComponent
Services    → product.service.ts → ProductService
Guards      → auth.guard.ts → authGuard (functional)
Pipes       → currency-format.pipe.ts → CurrencyFormatPipe
Signals     → productCount = signal(0)
Computed    → totalPrice = computed(() => ...)
```

### Folder Structure
```
src/
└── app/
    ├── core/
    │   ├── interceptors/
    │   ├── guards/
    │   └── services/
    ├── shared/
    │   ├── components/
    │   ├── directives/
    │   └── pipes/
    ├── features/
    │   ├── products/
    │   │   ├── components/
    │   │   ├── services/
    │   │   ├── store/
    │   │   └── products.routes.ts
    │   └── orders/
    └── models/
        └── product.model.ts
```

---

## 3. REACT — RULES & CONVENTIONS

### Architecture
- **Feature-based folder structure** — co-locate everything per feature
- **Functional components + Hooks only** — no class components ever
- Use **TanStack Query (React Query)** for all server state
- Use **Zustand** for client/global state (prefer over Redux for simplicity)
- Use **React Router v6** with nested routes

### TypeScript Style
- **Strict mode** always
- No `any` — use proper generics or `unknown`
- Type all props with `interface`, not inline types
- Use `FC<Props>` sparingly — prefer plain function with typed params
- Use `as const` for static config/enum-like objects

### Component Rules
- One component per file — always
- Use **named exports** for components (not default)
- Props interface named `[ComponentName]Props`
- Keep components under 150 lines — extract if larger
- Use `React.memo()` for expensive pure components
- Avoid prop drilling beyond 2 levels — use context or Zustand

### Hooks
- Custom hooks prefixed with `use` — `useProductList`, `useAuth`
- Extract all reusable logic into custom hooks
- Use `useCallback` and `useMemo` only when profiler confirms benefit
- Use `useRef` for mutable values that don't need re-renders

### State Management
```typescript
// Server state → TanStack Query
const { data, isLoading } = useQuery({
  queryKey: ['products', filters],
  queryFn: () => productService.getAll(filters),
})

// Client state → Zustand
const useCartStore = create<CartState>((set) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
}))
```

### API / Services
- Centralize all HTTP calls in service files (`services/product.service.ts`)
- Use `axios` with a configured instance (base URL, interceptors, auth header)
- Always type request and response: `axios.get<Product[]>('/api/products')`
- Handle errors in the service layer — return typed error results

### Testing (React)
- **Jest** + **React Testing Library** — always
- Test user behavior, not implementation details
- Use `msw` (Mock Service Worker) for API mocking in tests
- Snapshot tests only for stable UI — not for logic-heavy components

### Naming Conventions
```
Components    → ProductList.tsx (PascalCase)
Hooks         → useProductList.ts (camelCase with 'use' prefix)
Services      → product.service.ts (camelCase)
Types/Models  → product.types.ts
Store         → cart.store.ts
Utils         → format-currency.ts (kebab-case)
```

### Folder Structure
```
src/
├── components/        ← Truly shared UI (Button, Modal, Input)
├── features/
│   ├── products/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── store/
│   │   └── types/
│   └── orders/
├── hooks/             ← App-wide hooks (useAuth, useTheme)
├── lib/               ← Third-party config (axios instance, queryClient)
├── types/             ← Global TypeScript types
└── utils/             ← Pure helper functions
```

---

## 4. AZURE CLOUD — RULES & CONVENTIONS

### Authentication & Security
- **NEVER hardcode secrets, keys, or connection strings** — use Azure Key Vault
- Use **Managed Identity** for service-to-service authentication
- Use `DefaultAzureCredential` from `Azure.Identity` SDK
- Use **Azure AD / Microsoft Entra ID** for user authentication (MSAL)

### Azure Services in Use
| Service              | Purpose                              |
|---------------------|---------------------------------------|
| Azure App Service   | Host .NET API and static frontends   |
| Azure Functions     | Serverless event-driven processing   |
| Azure SQL Database  | Relational data with EF Core          |
| Azure Blob Storage  | File and media storage               |
| Azure Service Bus   | Async messaging between services     |
| Azure Key Vault     | Secrets and certificates             |
| Application Insights| Telemetry, logging, APM              |
| Azure API Management| API gateway, rate limiting, docs     |

### Logging & Monitoring
- Use `ILogger<T>` — never `Console.WriteLine` in production code
- Add structured logging properties: `_logger.LogInformation("Processing {OrderId}", orderId)`
- Configure **Application Insights** with `TelemetryClient` for custom events
- Use **correlation IDs** across service calls for distributed tracing

### Azure Functions
- Use **isolated worker model** for all new Functions (.NET)
- Use **timer triggers** for scheduled jobs, **service bus triggers** for async work
- Keep Functions stateless — use **Durable Functions** for stateful workflows
- Bind secrets via Key Vault references in `local.settings.json`

### Infrastructure as Code
- Use **Bicep** or **Terraform** — no manual portal configuration for infrastructure
- Tag all resources: `environment`, `project`, `owner`, `cost-center`
- Use **Azure Resource Naming Conventions** (e.g., `rg-myapp-prod-eus`)

### Cost Optimization
- Use `AsNoTracking()` on all EF read queries (reduces memory on DB server)
- Cache Azure Blob SAS URLs in Redis/memory — don't regenerate per request
- Use Service Bus batch receives — not one message at a time
- Set appropriate App Service plan and scale rules

---

## 5. GENERAL RULES — ALL TECH STACKS

### What Copilot Must ALWAYS Do
- Write **production-quality code** — not demo/prototype shortcuts
- Add **XML doc comments** on all public .NET methods and interfaces
- Add **JSDoc comments** on exported TypeScript functions and interfaces
- Follow the **folder structure** defined above for each stack
- Write code that is **unit-testable** — avoid static dependencies
- Use **meaningful names** — no single-letter variables except loop counters

### What Copilot Must NEVER Do
- ❌ Hardcode URLs, secrets, API keys, or connection strings
- ❌ Use `any` in TypeScript
- ❌ Leave `TODO` or `FIXME` comments in suggestions without context
- ❌ Use synchronous HTTP calls or blocking operations
- ❌ Generate God classes or methods over 40 lines
- ❌ Use deprecated APIs — always suggest the latest stable approach
- ❌ Generate commented-out code blocks

### Git & PR Conventions
- Branch names: `feature/TICKET-123-short-description` or `fix/TICKET-456-bug-name`
- Commit style: **Conventional Commits**
  - `feat:` new features
  - `fix:` bug fixes
  - `refactor:` code restructuring
  - `test:` adding/updating tests
  - `docs:` documentation only
  - `chore:` build/tooling changes
- Every PR must include tests for new logic

### Code Review Checklist (for Copilot PR reviews)
- [ ] No secrets or hardcoded values
- [ ] async/await used consistently
- [ ] Input validation present
- [ ] Error handling covered
- [ ] Unit tests written
- [ ] No `any` or suppressed type errors
- [ ] Follows naming conventions above

---

## How to Use This File

Place this file at: **`.github/copilot-instructions.md`** in your repo root.

GitHub Copilot will automatically pick up these instructions for:
- Inline code completions
- Copilot Chat (`@workspace` context)
- `/fix`, `/explain`, `/tests`, `/doc` slash commands

> **Shared by the community** — feel free to fork and adapt for your project.
> Star the repo if this helped you! 🌟
