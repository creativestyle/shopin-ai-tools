# Reference examples

Study these existing features before creating a new one.

## Cart (recommended reference — full-stack feature)

Complex feature with hooks, components, services, and providers.

- Feature dir: `apps/presentation/features/cart/`
- Components: `apps/presentation/features/cart/components/`
- Hooks: `apps/presentation/features/cart/hooks/` (use-add-to-cart, use-remove-cart-item, etc.)
- Service: `apps/presentation/features/cart/lib/cart-bff-service.ts`
- BFF module: `apps/bff/src/features/cart/`

## Product (simpler reference)

Simpler feature showing server components with data fetching.

- Feature dir: `apps/presentation/features/product/`
- Page: `apps/presentation/features/product/product-page.tsx`
- Components: `apps/presentation/features/product/components/`
- Service: `apps/presentation/features/product/lib/product-service.ts`
