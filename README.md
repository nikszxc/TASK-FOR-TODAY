# Tasks for Today Management System

## What is this?

**Tasks for Today** is a task management system built on [CodeIgniter 4](https://codeigniter.com/), a PHP full-stack web framework that is light, fast, flexible and secure. It was built as a laboratory activity to practice wiring real Models, Controllers, and Views to a MySQL database.

The system has four pages:

- **Welcome** (`/`) — shows only today's tasks
- **Task List** (`/tasks`) — shows every task, ordered by date
- **Profile** (`/profile`) — shows the single demo user record
- **About** (`/about`) — static page identifying the developer

Developed by **Andrea Magnaye**.

## Installation

This copy of the project already includes the CodeIgniter 4 framework itself under `system/`, so **no Composer install is required** — just clone or unzip it and it runs. (If you'd rather manage the framework via Composer instead, `composer create-project codeigniter4/appstarter` gives you the same starting point, and you'd copy the `app/`, `database/`, and `public/assets/` folders from this project into it.)

## Setup

Copy `env` to `.env` if you don't already have one, and tailor it for your machine — specifically the `baseURL` and the database settings.

1. **Create the database.** Either import the ready-made export, which already includes the schema and seed data (8 tasks across 3 dates, 1 demo user):
   ```bash
   mysql -u root -p -e "CREATE DATABASE tasks_today;"
   mysql -u root -p tasks_today < database/tasks_today.sql
   ```
   or build it from scratch with CodeIgniter's own tools:
   ```bash
   php spark migrate
   php spark db:seed DatabaseSeeder
   ```
   The seeder inserts tasks relative to whatever day you run it on (yesterday / today / tomorrow), so "today's tasks" is always accurate no matter when you seed it.

2. **Point `.env` at your database** (defaults shown are for a local `ci4user` account — edit to match your own credentials):
   ```
   database.default.hostname = localhost
   database.default.database = tasks_today
   database.default.username = ci4user
   database.default.password = ci4pass
   database.default.DBDriver = MySQLi
   database.default.port = 3306
   ```

3. **Run it:**
   ```bash
   php spark serve
   ```
   then visit `http://localhost:8080/`.

## Important Change with index.php

As with any CodeIgniter 4 app, `index.php` is not in the project root — it lives inside `public/`. Point your web server (or virtual host) at the `public` folder, not the project root; pointing at the root and expecting to enter `public/...` exposes the rest of the framework and app logic.

## Server Requirements

PHP version 8.1 or higher is required, with the following extensions installed:

* [intl](http://php.net/manual/en/intl.requirements.php)
* [mbstring](http://php.net/manual/en/mbstring.installation.php)

Additionally, make sure the following extensions are enabled:

* json (enabled by default — don't turn it off)
* [mysqlnd](http://php.net/manual/en/mysqlnd.install.php) or `pdo_mysql` — this app talks to MySQL
* [libcurl](http://php.net/manual/en/curl.requirements.php) — only needed if you extend the app to use `HTTP\CURLRequest`

## Project Structure

- `app/Database/Migrations/` — schema for `tasks` and `users`
- `app/Database/Seeds/` — `TaskSeeder`, `UserSeeder`, `DatabaseSeeder`
- `app/Models/` — `TaskModel`, `UserModel`
- `app/Controllers/` — `Home`, `Tasks`, `Profile`, `About`
- `app/Views/` — one view per page, sharing `templates/header.php` / `templates/footer.php`
- `database/tasks_today.sql` — full export of the seeded database

## Routes

| Route      | Page       | Description                            |
|------------|------------|-----------------------------------------|
| `/`        | Welcome    | Tasks where `task_date` = today         |
| `/tasks`   | Task List  | Every task, ordered by date             |
| `/profile` | Profile    | The single demo user record             |
| `/about`   | About      | Static page identifying the developer   |
