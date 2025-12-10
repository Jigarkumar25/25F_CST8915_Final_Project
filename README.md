```mermaid
flowchart LR

User -->|Uses Web App| StoreFront
Admin -->|Uses Admin App| StoreAdmin

StoreFront -->|Fetch Products| ProductService
StoreFront -->|Place Order| OrderService

OrderService -->|Send Order| RabbitMQ

RabbitMQ -->|Consume Orders| MakelineService
MakelineService -->|Store Data| MongoDB

StoreAdmin -->|View Orders| MongoDB
StoreAdmin -->|Manage Products| ProductService
StoreAdmin -->|Trigger Processing| MakelineService

```