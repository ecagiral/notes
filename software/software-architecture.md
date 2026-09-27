# On Software Architecture

The idea of separating domain logic from infrastructure is a powerful way to build an application.

In the ideal case, domain logic is a coherent unit with the single responsibility of defining business rules. But business rules need to interact with infrastructure.

Hexagonal architecture seperates domain (inside) from infrastructure (outside). Ports form the boundary and belong to the inside: outbound ports define contracts the infrastructure implements, and inbound ports let the core's implementation be swapped without changing its callers.

Within that I can define three layers: "domain" defines business rules, "port" is the contract, and "adapter" is the infrastructure implementation. Dependencies flow in a single direction: the adapter depends on the port, and the port depends on the domain.

But you still need a fourth layer, also inside, that depends on both port and domain: it loads state, calls the domain, and persists the result, because the domain doesn't care about durability. Let's call it 'application'.

Finally, some business rules require isolation and atomicity for consistency. In those cases, transaction boundaries and isolation are defined by the application layer and enforced by the infrastructure.