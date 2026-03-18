<p align="center">
  <kbd>
    <img src="https://raw.githubusercontent.com/phpfasty/.github/refs/heads/main/profile/php-fasty.png" width="256" alt="PHP FastY" />
  </kbd>
</p>

<h2 align="center">PHP FastY</h2>

<p align="center">
  They said the elephant was too heavy to fly.<br>
  Ours never looked back.
</p>



## What this is

PHP FastY is a collection of production-ready example applications that demonstrate how to extract serious performance from PHP without the weight of a full framework.

Each repository is a self-contained, deployable unit — `git clone`, `docker compose up`, done.



## How the speed is achieved

Performance here is not accidental. It is the result of deliberate architectural choices applied consistently at every layer:

**Micro-framework routing**  
A tiny dispatcher resolves routes in microseconds. No middleware stack loaded on every request. Only what the route needs.

**PSR-11 service container with lazy bindings**  
Every dependency is registered as a factory closure and instantiated only on first use. Boot time is near zero. `Closure::bind` is used where reflection overhead would otherwise be unavoidable.

**Shared memory cache**  
Frequently accessed data — structured content, rendered fragments, configuration — is stored in process-shared memory. A cache-aside pattern keeps reads at the speed of a hash lookup.

**Hybrid static + dynamic delivery**  
Nginx serves pre-rendered static files directly, bypassing PHP entirely for known, cacheable responses. PHP only handles the long tail — paginated content, dynamic queries, API endpoints that cannot be pre-generated.

**Twelve-Factor configuration**  
All environment-specific values are injected at runtime via environment variables. No hardcoded config. The same container image runs in development, staging, and production without modification.

**PHP configuration as code**  
`php.ini` and cache configuration live in the repository root and are mounted into the container. Tuning memory limits, TTLs, and opcode cache settings is a commit, not a server session.

---

## Architecture

```
Browser
  │
  ├── static files (HTML, CSS, JS) → served by Nginx directly
  │
  └── /api/* requests → FastCGI → PHP-FPM
                                      │
                              Service Container
                                      │
                             ┌────────┴────────┐
                          APCu cache       Fixtures / Data
                        (shared memory)    (JSON, DB)
```



## Stack

| Layer | Choice |
|---|---|
| Runtime | PHP 8.4 on Alpine |
| Web server | Nginx |
| Routing | FlightPHP core |
| DI Container | flightphp/container (PSR-11) |
| In-memory cache | APCu |
| Config | phpdotenv + environment variables |
| Deployment | Docker Compose, git pull |



## Repositories

Each repository in this organisation is a standalone example application following the same architecture. Clone any of them, bring up Docker Compose, and you have a working, instrumented baseline to build from.



*All the memory of an elephant. None of the weight.*
