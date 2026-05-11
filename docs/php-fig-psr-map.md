# PHP-FIG PSR Map for Laravel 13 Skills

This guide maps PHP-FIG standards to Laravel 13 work. Treat accepted PSRs as interoperability defaults, while still following the target project's installed packages and local conventions first.

Source baseline: [PHP-FIG PSR index](https://www.php-fig.org/psr/).

## Accepted PSRs

| PSR | Title | Laravel 13 guidance |
| --- | --- | --- |
| PSR-1 | Basic Coding Standard | New PHP code must use normal PHP tags, UTF-8 without BOM, namespaces, StudlyCaps classes, uppercase constants, and camelCase methods. |
| PSR-3 | Logger Interface | Type against `Psr\Log\LoggerInterface` when a service needs an injectable logger; use Laravel logging channels through the container. |
| PSR-4 | Autoloading Standard | Keep namespaces, Composer autoload paths, and file locations aligned. Do not add manual includes for application classes. |
| PSR-6 | Caching Interface | Use `Psr\Cache\CacheItemPoolInterface` only when a package or boundary needs cache-pool interoperability; prefer Laravel cache for ordinary app code. |
| PSR-7 | HTTP Message Interface | Use PSR-7 request/response objects at integration boundaries that require them; keep ordinary controllers on Laravel `Request`, responses, and resources. |
| PSR-11 | Container Interface | Type against `Psr\Container\ContainerInterface` only for framework-agnostic services; prefer constructor injection and Laravel container bindings in app code. |
| PSR-12 | Extended Coding Style Guide | Use Laravel Pint as the formatter. Do not hand-format code against a conflicting style when Pint or the project preset is configured. |
| PSR-13 | Hypermedia Links | Use for package or API boundaries that expose typed link relations; ordinary Laravel resources can use arrays/URLs unless interoperability is required. |
| PSR-14 | Event Dispatcher | Use Laravel events/listeners for application behavior; use PSR-14 only for framework-agnostic packages or third-party event integrations. |
| PSR-15 | HTTP Handlers | Use when implementing middleware/request handlers for PSR-7 ecosystems; ordinary Laravel middleware should remain Laravel-native. |
| PSR-16 | Simple Cache | Use `Psr\SimpleCache\CacheInterface` for simple interoperable cache dependencies; prefer Laravel cache facade/contracts for application code. |
| PSR-17 | HTTP Factories | Pair with PSR-7 when an integration must create PSR request/response/stream objects. |
| PSR-18 | HTTP Client | Type against `Psr\Http\Client\ClientInterface` for portable HTTP clients in packages or SDK-style boundaries; prefer Laravel `Http` client for app workflows. |
| PSR-20 | Clock | Use `Psr\Clock\ClockInterface` when time must be injectable and portable; Laravel tests may still use Laravel's time helpers where local style prefers them. |

## Draft PSRs

Draft PSRs can change and should not be treated as mandatory.

| PSR | Title | Guidance |
| --- | --- | --- |
| PSR-5 | PHPDoc Standard | Use PHPDoc where it helps static analysis and developer understanding, but prefer native PHP types first. |
| PSR-19 | PHPDoc Tags | Follow project/static-analysis conventions for tags until accepted. |
| PSR-21 | Internationalization | Do not require it; use Laravel localization conventions unless a project explicitly adopts this draft. |
| PSR-22 | Application Tracing | Do not require it; use the project's tracing/observability stack. |

## Deprecated or Abandoned PSRs

- PSR-0 and PSR-2 are deprecated. Prefer PSR-4 and PSR-12/Pint.
- PSR-8, PSR-9, and PSR-10 are abandoned. Do not apply them as requirements.

## Skill Mapping

- `laravel-13-core`: PSR-1, PSR-4, PSR-11, PSR-12, PSR-14, PSR-20.
- `laravel-13-api`: PSR-7, PSR-13, PSR-15, PSR-17, PSR-18 when API or integration boundaries require interoperable HTTP objects.
- `laravel-13-data-performance`: PSR-6, PSR-16, PSR-20 for cache and time abstractions at package or boundary layers.
- `laravel-13-testing-quality`: PSR-12 through Pint, plus test seams for PSR-3, PSR-18, and PSR-20 dependencies.
- `laravel-13-security-auth`: PSR-3 redaction-safe logging and PSR-7/15 review when interoperable middleware is present.
- `laravel-13-frontend-inertia`: PSR-1/4/12 for PHP-side code; frontend code follows the project's TypeScript and formatter setup.
- `laravel-13-ai-boost-mcp`: PSR-3 logging, PSR-18 HTTP clients, PSR-20 clocks, and PSR-11 container boundaries for SDK/package-style integrations.
- `laravel-13-upgrade-rector`: Replace legacy PSR-0/2 assumptions with PSR-4 and PSR-12/Pint during upgrades.
