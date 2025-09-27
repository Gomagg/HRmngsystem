### 📘 Project Best Practices

#### 1. Project Purpose
This repository is a Laravel 12-based Human Resources Management System (HRMS). It centralizes HR operations: user authentication, employee and department management, attendance and leaves, payroll and salary processing, performance management, training and trainers, recruitment (jobs, applicants, interview questions), sales estimates/expenses reporting, assets management, chat, and PDF/Excel reporting.


#### 2. Project Structure
- High-level layout (Laravel defaults with notable additions):
  - app/
    - Http/Controllers: Feature-based controllers (e.g., EmployeeController, LeavesController, PayrollController, SalesController, TrainingController, JobController, etc.).
    - Models: Eloquent models (e.g., Employee, Department, StaffSalary, Leave, Training, Category, Question, etc.). Ensure file/class names follow PSR-4 (StudlyCase) and match the class name.
    - Exports: Export classes (e.g., SalaryExcel.php) for Maatwebsite/Excel.
    - Helper/helpers.php: Project-specific global helpers (note: currently autoloaded under autoload-dev; see Best Practices below).
    - Providers: Laravel service providers (AppServiceProvider, AuthServiceProvider, RouteServiceProvider, etc.).
    - Rules: Custom validation rules (e.g., MatchOldPassword.php).
  - bootstrap/, storage/: Framework runtime/cache.
  - config/: Configuration (app, auth, cache, database, filesystems, logging, mail, queue, services, session).
  - database/
    - migrations/: Schema migrations covering HR, payroll, performance, training, sales, and recruitment domains.
    - factories/: Model factories (e.g., UserFactory) for testing.
    - seeders/: Database seeders entry point.
  - public/: Web root and static assets; index.php is the HTTP entrypoint.
  - resources/
    - views/: Blade templates (primary location for UI views).
    - js/, css/: Frontend assets bundled via Vite; TailwindCSS configured.
  - routes/
    - web.php: HTTP routes; grouped by feature with auth middleware and Route::controller usage.
    - console.php: Artisan console routes/commands.
  - tests/
    - Unit/, Feature/: PHPUnit/Laravel test suites with TestCase bootstrap.
  - vite.config.js, package.json: Frontend toolchain (Vite + TailwindCSS + axios).
  - composer.json: PHP dependencies and Composer scripts (dev/test flows, post-create hooks).

Notes:
- There is a top-level views/ directory in addition to resources/views/. Prefer resources/views for Blade templates to align with Laravel defaults. Only keep a top-level views/ if explicitly configured.
- Auth scaffolding leverages laravel/ui with Auth::routes(). Consider modern alternatives (Breeze/Fortify) if refactoring.


#### 3. Test Strategy
- Frameworks/Tools:
  - PHPUnit 11 with Laravel testing helpers (artisan test).
  - Mockery for mocking.
  - Database configured for testing via phpunit.xml using in-memory SQLite (DB_CONNECTION=sqlite, DB_DATABASE=:memory:).
  - Factories under database/factories are used for model creation in tests.

- Organization:
  - Place unit tests in tests/Unit for isolated logic (helpers, validation rules, pure services, Eloquent scopes/accessors/mutators).
  - Place feature tests in tests/Feature for HTTP endpoints, controllers, middleware, views, queues, and overall integration of layers.

- Conventions & helpers:
  - Use RefreshDatabase (or DatabaseMigrations) trait to ensure clean state per test class.
  - Use model factories and database seeders only where necessary; prefer factories over manual inserts.
  - For HTTP tests, assert status codes, redirects, view names/data, JSON payloads, and authorization.
  - For queues/events/listeners, use Queue::fake(), Bus::fake(), Event::fake() to assert dispatching behavior.
  - For file exports/imports, use Storage::fake() and Maatwebsite/Excel’s testing helpers; for PDF, assert response headers/content types.

- Philosophy:
  - Unit tests: Fast, isolated, no external I/O. Cover domain logic that can break silently (salary calculations, leave balance rules, appraisal scoring, query scopes).
  - Feature/integration tests: Validate critical flows end-to-end (login, CRUD for employees/departments, applying for jobs, leave submission/approval, payroll generation, report exports).
  - Aim for high coverage on risk areas (payroll, leave approvals, data exports). Use a baseline target of 70–80%+ for critical domain modules.


#### 4. Code Style
- Standards & tooling:
  - Follow PSR-12 and Laravel conventions. Use Laravel Pint for formatting: vendor/bin/pint.
  - Keep controllers thin; business logic belongs in services/actions or model methods/scopes where appropriate.

- Naming conventions:
  - Classes: StudlyCase; filenames should match class names exactly (e.g., Department.php, RoleTypeUser.php).
  - Controllers: Suffix with Controller (e.g., TrainingTypeController).
  - Models: Singular nouns (Employee, Leave, TrainingType). Prefer App\Models namespace.
  - Migrations: Use descriptive names; keep schema in snake_case.
  - Routes: Use consistent route names. Prefer dot notation for route names (e.g., employees.index) rather than embedding slashes; keep URIs kebab-case.

- Requests & validation:
  - Use FormRequest classes for validation and authorization. Keep validation rules out of controllers when complex.
  - Centralize validation messages and reuse rules via Rule classes (see app/Rules) or custom rule objects.

- Eloquent & database:
  - Use guarded/fillable on models to prevent mass-assignment issues.
  - Use casts for dates/booleans/decimals. For monetary values, use decimal(precision, scale) and cast to string/decimal to avoid float errors.
  - Create dedicated query scopes for commonly reused filters (e.g., active employees, pending leaves).
  - Prefer route model binding for cleaner controllers and 404 handling.

- Error/exception handling:
  - Validate inputs and fail fast with descriptive errors; use abort_if/throw when appropriate.
  - Catch at boundaries (external APIs, file IO); log via report(). Return user-friendly messages; never expose stack traces in production.
  - Use authorization via policies/gates for resource actions.

- Views & frontend:
  - Keep Blade templates in resources/views; break into layout + partials (components/partials) for reuse.
  - Use Vite for asset bundling; import resources/js/app.js and resources/css/app.css via @vite.
  - Use TailwindCSS utility classes; avoid inline styles. Encapsulate JS behavior in modules where possible.

- Helpers & utilities:
  - Prefer namespaced service classes over global helper functions. If helpers are necessary at runtime, move them from autoload-dev to autoload (composer.json) under "files".


#### 5. Common Patterns
- Route grouping with middleware('auth') and Route::controller to keep routes concise.
- Feature-based controller organization (Employees, Leaves, Payroll, Performance, Training, Trainers, Jobs, Sales, Reports, Settings, Chat, Assets).
- Eloquent models representing domain entities; leverage relationships, accessors/mutators, and query scopes for reusable logic.
- Reporting/Exporting:
  - PDF via barryvdh/laravel-dompdf. Use view-based PDFs; optimize images and fonts.
  - Excel via maatwebsite/excel. Use FromCollection/FromQuery/WithHeadings exports and chunked queries for large datasets.
- Queues/Async:
  - Queue workers can be run (composer run dev script runs queue:listen). Offload heavy exports/emails to queues.
- Configuration via config/*. Use config() accessors; avoid direct env() usage outside config files.


#### 6. Do's and Don'ts
- Do
  - Keep controllers slim; extract business logic into services/actions or model methods.
  - Use policies/gates and middleware for access control; never trust request data for authorization decisions.
  - Validate all incoming requests; prefer FormRequest classes.
  - Use route model binding and implicit bindings for cleaner code.
  - Use transactions for multi-step data changes (creating payroll + salary items, leave approval state transitions).
  - Log important domain events (payroll runs, approvals, deletions) with contextual data.
  - Add tests when fixing bugs or adding features; favor Feature tests for HTTP flows.
  - Run Pint and tests locally before committing; keep CI green.
  - Document env variables and sensitive configs in .env.example.

- Don't
  - Don’t put heavy logic in routes or Blade views.
  - Don’t query in tight loops; eager-load relations and use chunking for large datasets (exports/reports).
  - Don’t use floats for money; avoid magic numbers/strings—centralize constants/enums.
  - Don’t hardcode file paths, URLs, or credentials; use config and env variables.
  - Don’t expose exception traces or sensitive data to end users; sanitize outputs.


#### 7. Tools & Dependencies
- PHP/Laravel
  - PHP >= 8.2
  - laravel/framework ^12
  - laravel/ui: Auth scaffolding (Auth::routes()). Consider Breeze/Fortify if modernizing.
  - php-flasher/flasher-laravel: Flash notifications.
  - barryvdh/laravel-dompdf: PDF generation from Blade views.
  - maatwebsite/excel: Excel imports/exports.

- Dev & Testing
  - phpunit/phpunit ^11, mockery/mockery.
  - laravel/pint for code style.
  - laravel/pail for local log tailing.
  - laravel/sail for containerized dev (optional).

- Frontend
  - Vite ^6, TailwindCSS ^4, axios.

- Local setup
  - Copy .env.example to .env and set DB/app credentials.
  - composer install && npm install
  - php artisan key:generate
  - php artisan migrate (post-create hook may run migrations; verify your database connection)
  - For development: either
    - composer run dev (runs: php artisan serve, queue:listen, pail logs, and npm run dev concurrently), or
    - Run processes individually: php artisan serve, php artisan queue:listen, npm run dev
  - Run tests: php artisan test


#### 8. Other Notes
- Legacy/structure notes:
  - Ensure all Blade templates live in resources/views unless there is deliberate configuration for a custom path. If top-level views/ exists, consolidate into resources/views to avoid confusion.
  - Some model filenames appear with non-standard casing (e.g., department.php). Align filenames to match class names (Department.php) for PSR-4 autoload stability across case-sensitive filesystems.
  - app/Helper/helpers.php is autoloaded in autoload-dev. If helpers are required in production, move them to composer.json autoload.files.

- Domain considerations:
  - Monetary calculations: use integers (cents) or precise decimals and consistent rounding (Banker’s rounding vs standard) across payroll and reports.
  - Time and leave: normalize to a project-wide timezone (APP_TIMEZONE) and store UTC; use Carbon consistently.
  - Data privacy: audit access to employee PII; use policies and logs. Mask sensitive fields in logs.

- Performance & reliability:
  - For large exports, use queued exports and chunked queries to limit memory usage.
  - Add database indexes for frequently filtered columns (employee_id, department_id, dates, statuses).
  - Use caching where beneficial (config/cache routes if appropriate; cache heavy dropdowns or lookups).

- When generating new code:
  - Follow feature-based organization and consistent route naming. Add corresponding tests for new endpoints.
  - Wire new env keys into config/*.php and document them in .env.example.
  - Prefer dependency injection and constructor-typed services; avoid facades in core logic where testability matters.
