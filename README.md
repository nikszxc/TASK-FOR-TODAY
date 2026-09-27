# Tasks for Today Management System

A four-page CodeIgniter 4 + MySQL task management system built for the
laboratory activity: **Welcome** (today's tasks), **Task List** (all tasks),
**Profile** (demo user), and **About** (developer info).

Developed by **Andrea Magnaye**.

## Requirements

- PHP 8.1+ (with `intl`, `mbstring`, `mysqli`, `pdo_mysql` extensions)
- MySQL / MariaDB
- No Composer required — the CodeIgniter 4 framework is included directly
  under `system/`, and the app runs on CodeIgniter's own PSR-4 autoloader.

## Setup

1. **Create the database** (or import the ready-made export):
   ```bash
   mysql -u root -p -e "CREATE DATABASE tasks_today;"
   mysql -u root -p tasks_today < database/tasks_today.sql
   ```
   This import already includes the schema and seed data (8 tasks across
   3 dates, 1 demo user) — if you use it, skip the migrate/seed step below.

   **Or**, build it from scratch with CI4's own tools instead of the SQL file:
   ```bash
   php spark migrate
   php spark db:seed DatabaseSeeder
   ```
   The seeder inserts tasks relative to whatever day you run it on
   (yesterday / today / tomorrow), so "today's tasks" is always accurate.

2. **Configure the database connection** in `.env` (already set up for a
   local `ci4user` / `ci4pass` account — edit to match your own MySQL
   credentials):
   ```
   database.default.hostname = localhost
   database.default.database = tasks_today
   database.default.username = ci4user
   database.default.password = ci4pass
   database.default.DBDriver = MySQLi
   database.default.port = 3306
   ```

3. **Run the app**:
   ```bash
   php spark serve
   ```
   Then visit `http://localhost:8080/`.

## Routes

| Route      | Page       | Description                          |
|------------|------------|---------------------------------------|
| `/`        | Welcome    | Tasks where `task_date` = today       |
| `/tasks`   | Task List  | Every task, ordered by date           |
| `/profile` | Profile    | The single demo user record           |
| `/about`   | About      | Static page identifying the developer |

## Structure

- `app/Database/Migrations/` — schema for `tasks` and `users`
- `app/Database/Seeds/` — `TaskSeeder`, `UserSeeder`, `DatabaseSeeder`
- `app/Models/` — `TaskModel`, `UserModel`
- `app/Controllers/` — `Home`, `Tasks`, `Profile`, `About`
- `app/Views/` — one view per page, sharing `templates/header.php` /
  `templates/footer.php`
- `database/tasks_today.sql` — full export of the seeded database
