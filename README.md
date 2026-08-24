# Subscrio

Subscrio is an open-source entitlement engine for .NET and TypeScript applications. It stores products, features, plans, customers, and subscriptions in your database, then resolves the feature value available to a customer.

The library is embedded in the application that needs the access decision. It does not require a hosted entitlement service.

## Why it exists

Plan-name checks tend to spread through application code. They become harder to maintain when plans change, a customer negotiates an exception, several subscriptions provide access, or a feature needs a limit instead of a yes-or-no result.

Subscrio keeps that logic in one model. Application code checks a stable feature key. The catalog, subscription, and any customer-specific override determine the returned toggle, number, or text value.

Subscrio is not a payment processor, user-authorization system, feature-flag service, or license-key activation service. It can work alongside those systems.

## How it works

1. Define products, features, plans, and billing cycles through the API or [configuration sync](https://docs.subscrio.com/reference/config-sync.md).
2. Create and update customers and subscriptions from your application or an integration.
3. Ask Subscrio for a feature by its stable key when the application needs to make an access decision.

Catalog and subscription data remain in your database. Plans and customer exceptions can change without rewriting checks throughout the application.

## Shared domain model

The .NET and TypeScript implementations use the same concepts:

| Entity | Purpose |
| --- | --- |
| Product | Groups the features and plans for a software product. |
| Feature | Defines a stable key, value type, and default value for something the application can check. |
| Plan | Assigns feature values within a product. |
| Billing cycle | Describes how a plan recurs, including its duration and unit. |
| Customer | Identifies the customer or account whose access is being evaluated. |
| Subscription | Connects a customer to a plan and billing cycle, with dates and lifecycle state. |
| Feature override | Replaces a plan value for one subscription, either temporarily or permanently. |

Applications refer to records by stable string keys, such as `seat-limit` or `growth-plan`. Database IDs stay inside Subscrio.

## Resolving a feature

Subscrio resolves a feature value in this order:

1. A feature override on the subscription
2. The feature value assigned by the plan
3. The feature's default value

The same path supports toggle, numeric, and text values. Customers can have multiple subscriptions, and subscription status is calculated from lifecycle dates and cancellation state. See the [feature checker reference](https://docs.subscrio.com/reference/feature-checker.md) for method-level behavior.

## Implementations

| Language | Package | Database support | Setup |
| --- | --- | --- | --- |
| TypeScript | npm [`subscrio`](https://www.npmjs.com/package/subscrio) | PostgreSQL | [TypeScript README](https://github.com/subscrio/subscrio-typescript) |
| .NET | NuGet [`Subscrio.Core`](https://www.nuget.org/packages/Subscrio.Core) | PostgreSQL and SQL Server | [.NET README](https://github.com/subscrio/subscrio-dotnet) |

A Rust implementation is planned but is not currently available.

## Extensions

First-party extensions register through Subscrio hooks and maintain their own database schema.

| Extension | Purpose | TypeScript | .NET |
| --- | --- | --- | --- |
| Audit log | Records Subscrio changes in `subscrio.transaction_logs`. | [`subscrio-audit-log`](https://github.com/subscrio/subscrio-extensions-audit-log) | [`Subscrio.AuditLog`](https://github.com/subscrio/subscrio-extensions-audit-log) |
| Payments | Records supported Stripe invoice payment data in `subscrio.payments`. | [`subscrio-payments`](https://github.com/subscrio/subscrio-extensions-payments) | [`Subscrio.Payments`](https://github.com/subscrio/subscrio-extensions-payments) |

See [How to extend Subscrio](https://docs.subscrio.com/reference/how-to-extend.md) for hook behavior and extension examples.

## Stripe integration

The optional Stripe integration turns supported subscription events into Subscrio subscription updates. When your application receives the webhook, it must verify the Stripe signature before passing the event to the library. The separate Subscrio Web Admin can receive and verify Stripe webhooks directly.

See [Integrate with Stripe](https://docs.subscrio.com/reference/how-to-integrate-with-stripe.md) for the supported events and setup details.

## Documentation

- [Documentation](https://docs.subscrio.com/)
- [TypeScript setup and API overview](https://github.com/subscrio/subscrio-typescript)
- [.NET setup and API overview](https://github.com/subscrio/subscrio-dotnet)
- [Reference source](https://docs.subscrio.com/reference/)

## Repositories and local workspace

Subscrio is split across multiple GitHub repositories. For local development, clone the repos you need into a shared workspace root so `file:` dependencies and .NET project references resolve.

See [repos.md](./repos.md) for the repository list and recommended folder layout.

## Contributing

Use the implementation README in each repository for build, test, and contribution instructions. Cross-cutting changes start in [subscrio/subscrio](https://github.com/subscrio/subscrio/issues).

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

Subscrio is available under the [MIT License](./LICENSE).
