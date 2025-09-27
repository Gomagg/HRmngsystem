<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

You may also try the [Laravel Bootcamp](https://bootcamp.laravel.com), where you will be guided through building a modern Laravel application from scratch.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains thousands of video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the [Laravel Partners program](https://partners.laravel.com).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel/)**
- **[DevSquad](https://devsquad.com/hire-laravel-developers)**
- **[Redberry](https://redberry.international/laravel-development/)**
- **[Active Logic](https://activelogic.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).




Prerequisites
- PHP: 8.2+
  - Enable extensions in php.ini: OpenSSL, PDO (pdo_mysql for MySQL), Mbstring, Tokenizer, XML, Ctype, JSON, BCMath, Fileinfo, Zip, GD
- Composer: 2.x
- Node.js: 18+ (LTS recommended) and npm 9+
- Database: SQLite (simplest) or MySQL/MariaDB
- Git (optional, if cloning)

1) Get the source and install dependencies
- Open a terminal in c:\Users\Emmannate\Downloads\HR-Management-System-Built-on-Laravel-12-main
- Install PHP dependencies:
  composer install
- Install frontend/tooling dependencies:
  npm install

2) Configure environment
- Copy the example env file and generate app key:
  copy .env.example .env
  php artisan key:generate
- Set APP_URL to match where you’ll serve the app (e.g., APP_URL=http://127.0.0.1:8000)
- Configure mail for local dev to avoid sending real emails:
  MAIL_MAILER=log

3) Choose and configure database

Option A: SQLite (quickest)
- Create the SQLite database file:
  type NUL > database\database.sqlite
- In .env set:
  DB_CONNECTION=sqlite
  DB_DATABASE=database/database.sqlite
  (Clear other DB_* keys or leave them unused)

Option B: MySQL/MariaDB
- Create a database and user (example)
  CREATE DATABASE hrms CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
  CREATE USER 'hrms'@'localhost' IDENTIFIED BY 'password';
  GRANT ALL PRIVILEGES ON hrms.* TO 'hrms'@'localhost';
  FLUSH PRIVILEGES;
- In .env set:
  DB_CONNECTION=mysql
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=hrms
  DB_USERNAME=hrms
  DB_PASSWORD=password

4) Migrate the database
- Run all migrations:
  php artisan migrate
- If you plan to use queued jobs in dev (recommended), switch queue driver to database and add queue tables:
  In .env:
    QUEUE_CONNECTION=database
  Then:
    php artisan queue:table
    php artisan migrate

5) Storage and assets
- Link the storage folder for public file access:
  php artisan storage:link
- Start Vite in dev mode for assets (JS/CSS, TailwindCSS):
  npm run dev
  (Leave running while developing. For production build: npm run build)

6) Start the application

Option A: All-in-one dev script (concurrently runs server, queue worker, logs, and Vite)
- Requires Node.js for npx concurrently
  composer run dev
  This runs:
  - php artisan serve
  - php artisan queue:listen --tries=1
  - php artisan pail --timeout=0 (logs)
  - npm run dev (Vite)

Option B: Run processes individually in separate terminals
- Serve the app:
  php artisan serve
- Start the queue worker (if QUEUE_CONNECTION=database):
  php artisan queue:listen --tries=1
- Start Vite:
  npm run dev

7) Access the app
- Visit: http://127.0.0.1:8000
- You should see the login page.
- Registration is enabled at /register based on routes; create the first user there, then manage roles/permissions inside the app.

8) Running tests
- phpunit.xml is configured to use in-memory SQLite
- Execute:
  php artisan test
  or
  ./vendor/bin/phpunit

9) Common troubleshooting
- Port already in use:
  - Change the port: php artisan serve --host=127.0.0.1 --port=8001
- Node/Vite/Tailwind not building:
  - Ensure Node 18+; delete node_modules and reinstall if needed.
  - Clear Vite cache: stop all processes, then rerun npm run dev.
- Class/file not found on case-sensitive systems:
  - Ensure model/controller filenames exactly match class names (StudlyCase).
- Queue not processing:
  - Ensure QUEUE_CONNECTION=database (or redis) and a worker is running (queue:listen).
  - Ensure queue tables exist (php artisan queue:table && php artisan migrate).
- Environment/config cache:
  - php artisan optimize:clear
  - php artisan config:clear
  - php artisan route:clear
  - php artisan view:clear
- PDF/Excel features:
  - Ensure PHP extensions: fileinfo, gd, zip are enabled.

10) Recommended local .env baseline
- Example for SQLite:
  APP_NAME="HR Management System"
  APP_ENV=local
  APP_KEY=base64:generated-by-key-generate
  APP_DEBUG=true
  APP_URL=http://127.0.0.1:8000

  LOG_CHANNEL=stack
  LOG_LEVEL=debug

  DB_CONNECTION=sqlite
  DB_DATABASE=database/database.sqlite

  BROADCAST_DRIVER=log
  CACHE_DRIVER=file
  FILESYSTEM_DISK=public
  QUEUE_CONNECTION=database
  SESSION_DRIVER=file
  SESSION_LIFETIME=120

  MAIL_MAILER=log
  MAIL_HOST=127.0.0.1
  MAIL_PORT=1025
  MAIL_FROM_ADDRESS="no-reply@example.test"
  MAIL_FROM_NAME="${APP_NAME}"

11) Production notes (summary)
- Use a real database service (MySQL/PostgreSQL), run migrations, and configure a proper queue (database or Redis) with a supervisor.
- Set APP_DEBUG=false, proper APP_URL, and secure session/cookie settings.
- Build assets: npm ci && npm run build; ensure @vite tags are served from the built files with proper Vite/Laravel configuration.
- Configure mail (SMTP) and storage permissions; run php artisan storage:link.

All commands above are intended to be run from:
c:\Users\Emmannate\Downloads\HR-Management-System-Built-on-Laravel-12-main
