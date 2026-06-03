# Reference examples

Study these existing integrations before creating a new one.

## Commercetools API (recommended reference — full integration)

Mature integration with services, mappers, and client abstraction.

- Module: `integrations/commercetools-api/src/commercetools-api.module.ts`
- Service provider: `integrations/commercetools-api/src/commercetools-service-provider.ts`
- Services: `integrations/commercetools-api/src/services/` (product, cart, customer, etc.)
- Mappers: `integrations/commercetools-api/src/mappers/` (product-card, cart, price, etc.)
- Client: `integrations/commercetools-api/src/client/`

## Mock API (simpler reference)

Minimal integration useful for understanding the contract interface without external API complexity.

- Module: `integrations/mock-api/src/`
