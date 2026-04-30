# RapidGo — Serverless Backend Architecture on Azure

The architecture follows **Clean Architecture** principles with strict separation of concerns across four layers:

```
src/
├── RapidGo.Domain/          # Enterprise business rules (entities, interfaces, exceptions)
├── RapidGo.Application/     # Application business rules (handlers, DTOs, commands/queries)
├── RapidGo.Infrastructure/  # External concerns (Azure Table Storage repositories)
└── RapidGo.Functions/       # Delivery mechanism (HTTP-triggered Azure Functions)

tests/
└── RapidGo.Tests/           # xUnit unit tests (domain + application layers)
```

---

## Architecture Layers

### 1. Domain (`RapidGo.Domain`)
Pure .NET 8 class library with **zero external dependencies**.

| Component | Description |
|-----------|-------------|
| `Entities/Order` | Aggregate root — encapsulates order lifecycle state machine |
| `Entities/Trip` | Represents an active or completed delivery trip |
| `Entities/Customer` | Customer value object |
| `Interfaces/IOrderRepository` | Repository contract (no implementation detail leaks in) |
| `Interfaces/ITripRepository` | Repository contract for trips |
| `Exceptions/DomainException` | Base exception type for domain rule violations |
| `Exceptions/OrderNotFoundException` | Thrown when a requested order doesn't exist |

**Key design decision:** Entities use private constructors and static factory methods (`Order.Create(...)`) to enforce invariants at creation time.

### 2. Application (`RapidGo.Application`)
Orchestrates use cases. Depends only on `Domain`. Uses CQRS-lite pattern (Commands / Queries without a mediator library).

| Component | Description |
|-----------|-------------|
| `Orders/Commands/CreateOrderCommand` | DTO for creating a new order |
| `Orders/Commands/CreateOrderHandler` | Creates and persists a new order |
| `Orders/Commands/UpdateOrderStatusCommand` | DTO for accept/complete/cancel transitions |
| `Orders/Commands/UpdateOrderStatusHandler` | Applies status transitions with business rule validation |
| `Orders/Queries/GetOrderQuery` | Query for a single order |
| `Orders/Queries/GetOrderHandler` | Retrieves one order or throws `OrderNotFoundException` |
| `Orders/Queries/ListOrdersByCustomerQuery` | Query for all orders by customer |
| `Orders/Queries/ListOrdersByCustomerHandler` | Returns all orders for a given customer |
| `DTOs/OrderDto` | Read-model returned to the caller |
| `DTOs/TripDto` | Read-model for trips |

### 3. Infrastructure (`RapidGo.Infrastructure`)
Implements domain interfaces using Azure services.

| Component | Description |
|-----------|-------------|
| `Repositories/OrderRepository` | Azure Table Storage implementation of `IOrderRepository` |
| `Repositories/TripRepository` | Azure Table Storage implementation of `ITripRepository` |
| `Extensions/ServiceCollectionExtensions` | `AddInfrastructure(IConfiguration)` DI registration helper |

**Partition strategy:**
- Orders table: `PartitionKey = CustomerId`, `RowKey = OrderId` — efficient per-customer queries.
- Trips table: `PartitionKey = OrderId`, `RowKey = TripId` — efficient per-order queries.

### 4. Functions (`RapidGo.Functions`)
Entry point — Azure Functions Isolated Worker (.NET 8).

| Function | Method | Route | Description |
|----------|--------|-------|-------------|
| `CreateOrder` | POST | `/api/orders` | Create a new delivery order |
| `GetOrder` | GET | `/api/orders/{orderId}` | Get a specific order by ID |
| `ListOrdersByCustomer` | GET | `/api/customers/{customerId}/orders` | List all orders for a customer |
| `UpdateOrderStatus` | PATCH | `/api/orders/{orderId}/status` | Transition order status (accept/complete/cancel) |

---

## Getting Started

### Prerequisites
- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8)
- [Azure Functions Core Tools v4](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local)
- [Azurite](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azurite) (local Azure Storage emulator)

### Build

```bash
dotnet build RapidGo.sln --configuration Release
```

### Run tests

```bash
dotnet test tests/RapidGo.Tests/RapidGo.Tests.csproj
```

### Run locally

```bash
# Start Azurite in another terminal
azurite --silent

# Start Functions host
cd src/RapidGo.Functions
func start
```

---

## API Examples

### Create Order
```http
POST /api/orders
Content-Type: application/json
x-functions-key: <your-function-key>

{
  "customerId": "cust-abc123",
  "pickupAddress": "Carrera 50 #70-20, Medellín",
  "destinationAddress": "Calle 80 #45-10, Medellín",
  "estimatedPrice": 12500.00
}
```

### Get Order
```http
GET /api/orders/{orderId}
x-functions-key: <your-function-key>
```

### Update Status
```http
PATCH /api/orders/{orderId}/status
Content-Type: application/json
x-functions-key: <your-function-key>

{ "action": "accept" }
```
Valid actions: `accept` → `complete` → `cancel`

---

## Configuration

| Key | Description |
|-----|-------------|
| `AzureWebJobsStorage` | Azure Storage connection string (use `UseDevelopmentStorage=true` locally) |
| `FUNCTIONS_WORKER_RUNTIME` | Must be `dotnet-isolated` |

See `src/RapidGo.Functions/local.settings.json` for local development defaults.

---

## Azure Services Used

| Service | Role |
|---------|------|
| **Azure Functions (v4, Isolated)** | HTTP-triggered serverless compute |
| **Azure Table Storage** | NoSQL persistence for orders and trips |
| **Azure API Management** *(recommended)* | Gateway for auth, rate-limiting, and routing |
| **Azure Application Insights** *(recommended)* | Telemetry, distributed tracing, and alerting |

---

## Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Clean Architecture** | Clear separation of business rules from delivery and infrastructure concerns |
| **Isolated Worker model** | Full control over DI, middleware, and the host process; avoids dependency conflicts |
| **CQRS-lite (no mediator)** | Explicit handler types are easy to navigate and test without an extra library |
| **Azure Table Storage** | Serverless, cost-effective, highly scalable; fits the university project budget |
| **Nullable enabled** | Eliminates null-reference surprises at compile time |
| **Private constructors + static factories** | Entities self-validate on creation, preventing invalid domain objects |

---

## CI

GitHub Actions workflow at `.github/workflows/ci.yml` runs on every push/PR:

1. `dotnet restore`
2. `dotnet build --configuration Release`
3. `dotnet test` with TRX report artifact
