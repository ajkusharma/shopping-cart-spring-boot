# Shopping Cart — Spring Boot

Project: Online Shopping Cart (Spring Boot + Thymeleaf)

Author: Ajay Sharma

This repository contains a lightweight online shopping cart web application built with Spring Boot, Spring MVC, Spring Data JPA and Thymeleaf. It supports user registration and login, product browsing, cart operations, checkout, order history, and an admin area for managing products, categories and users.

Quick plan
- Create and edit `src/main/resources/application.properties` with your DB and mail settings.
- Build and run on Windows using PowerShell and the included `mvnw.cmd`.
- Visit the app in a browser, register a user, and try browsing/adding products to the cart.

Checklist
- [x] Project overview and tech stack
- [x] Prerequisites and setup instructions (MySQL/H2 and properties)
- [x] Windows PowerShell build & run commands
- [x] User and admin workflow descriptions
- [x] File layout and where to find templates/static/java packages
- [x] Testing and troubleshooting tips

---

## Table of Contents
1. Project Overview
2. Tech Stack
3. Prerequisites
4. Setup (Database & properties)
5. Build & Run (Windows PowerShell)
6. Application Workflow — User
7. Application Workflow — Admin
8. Password Reset Flow
9. File Layout
10. Security & Roles
11. Configuration Properties Examples
12. Testing
13. Troubleshooting
14. Next Steps / Roadmap
15. License & Contact

---

## 1) Project Overview
A simple e-commerce shopping cart application demonstrating a full-stack Spring Boot MVC web app using Thymeleaf templates. Main features:
- User registration, login, profile management
- Product listing by category, product details pages
- Shopping cart (add/update/remove) and checkout flow
- Order history for users
- Admin area for CRUD operations on products, categories and users

## 2) Tech Stack
- Java 17
- Spring Boot 3.x (Spring MVC, Spring Data JPA, Spring Security)
- Thymeleaf for server-side HTML templates
- MySQL (production) or H2 (test/dev) as the database
- Maven (wrapper included) for build

## 3) Prerequisites
- JDK 17+ installed and `JAVA_HOME` set
- PowerShell (Windows) is the demonstrated shell here
- MySQL server (or use H2 for in-memory testing)
- Optionally an SMTP server or development mail configuration

## 4) Setup (Database & properties)
1. Create a MySQL database (example):

   CREATE DATABASE shopping_cart CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

2. Update `src/main/resources/application.properties` with your connection values. Example minimal properties:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/shopping_cart?serverTimezone=UTC&useSSL=false
spring.datasource.username=your_db_user
spring.datasource.password=your_db_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false
spring.thymeleaf.cache=false
# File storage (images)
app.upload.dir=src/main/resources/static/img/product_img
# Mail (for password reset)
spring.mail.host=smtp.example.com
spring.mail.port=587
spring.mail.username=your_smtp_user
spring.mail.password=your_smtp_password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

3. (Optional) For development/testing you can use the H2 in-memory DB by adding a `test` profile or temporarily switching datasource URL to `jdbc:h2:mem:testdb`.

## 5) Build & Run (Windows PowerShell)
Recommended commands for Windows PowerShell (from project root):

Build package:

```powershell
.\mvnw.cmd clean package -DskipTests
```

Run with the wrapper (dev):

```powershell
.\mvnw.cmd spring-boot:run
```

Run the packaged jar:

```powershell
java -jar .\target\Online_Shopping_Cart-0.0.1-SNAPSHOT.jar
```

Run with a specific Spring profile (PowerShell example):

```powershell
$env:SPRING_PROFILES_ACTIVE = 'dev'; .\mvnw.cmd spring-boot:run; Remove-Item Env:\SPRING_PROFILES_ACTIVE
```

If using MySQL, ensure the DB is running and properties are correct before starting.

## 6) Application Workflow — User
- Registration & Login
  - Register via `/register` and log in via `/login`.
  - Account/profile pages available for viewing/updating user details.

- Browsing Products
  - Home and category pages list products.
  - Clicking a product goes to a detail page (e.g., `/view_product?id={id}`).

- Shopping Cart & Checkout
  - Add product(s) to the cart (session-backed or user-backed depending on auth).
  - Update quantities or remove items in the cart page.
  - Proceed to checkout, confirm order and see an order success page.

- Orders
  - After checkout, orders are stored and can be viewed in `user` profile pages (e.g., `my_orders.html`).

## 7) Application Workflow — Admin
- Admin access is provided under `/admin` pages (templates under `src/main/resources/templates/admin/`).
- Admin operations:
  - Add/Edit/Delete products (`add_product.html`, `edit_product.html`).
  - Manage categories (`category.html`, `edit_category.html`).
  - View orders and user list pages.
- Create an initial admin user by inserting a user record with role `ROLE_ADMIN` or follow any provided seeding instructions.

## 8) Password Reset Flow
- 'Forgot password' form captures user email and creates a reset token.
- App sends an email with a reset link (token) — configure SMTP in `application.properties`.
- Reset link validates token (and expiry) and allows setting a new password.
- In development, if mail is not configured, check application logs or console output for the generated reset link/token.

## 9) File Layout
- Templates: `src/main/resources/templates/` (including `admin/` and `user/` subfolders)
- Static assets: `src/main/resources/static/` (css, js, img)
- Java sources: `src/main/java/com/ecom/`
  - `controller/` — web controllers
  - `service/` — business logic
  - `repository/` — Spring Data JPA repositories
  - `model/` — JPA entities
  - `config/` — security and app configuration
  - `util/` — helpers
- Tests: `src/test/java/` and `src/test/resources/`

## 10) Security & Roles
- Roles used by the app: `ROLE_USER`, `ROLE_ADMIN` (check your security config in `config/` package).
- Spring Security handles authentication and method/URL authorization.
- Certain admin endpoints and templates are restricted to the admin role.

## 11) Configuration Properties Examples
Important properties to review/edit in `src/main/resources/application.properties`:
- Database: `spring.datasource.*`
- JPA: `spring.jpa.hibernate.ddl-auto`, `spring.jpa.show-sql`
- Thymeleaf: `spring.thymeleaf.cache` (disable in dev)
- Mail: `spring.mail.*` for password reset emails
- App-specific: `app.upload.dir`, token expiry values, pagination sizes

## 12) Testing
Run unit and integration tests with the wrapper:

```powershell
.\mvnw.cmd test
```

There is a sample application test `ShoppingCartApplicationTests` located under `src/test/java/com/ecom/`.

To run tests using H2 (in-memory) make sure your `test` profile or test `application.properties` points to H2.

## 13) Troubleshooting
- Database connection errors: verify `spring.datasource.url`, credentials, and that MySQL is running.
- Missing upload directories: create the paths referenced in `app.upload.dir` and ensure the app has write permissions.
- Email issues: check SMTP credentials and port; use a local dev SMTP or inspect logs for reset tokens.
- View errors: inspect Thymeleaf templates for mismatched model attributes.

Useful dev commands:

```powershell
.\mvnw.cmd clean package -DskipTests
.\mvnw.cmd spring-boot:run
```

To enable more logging, change the logging level in `application.properties`:

```properties
logging.level.org.springframework=DEBUG
logging.level.com.ecom=DEBUG
```

## 14) Next Steps / Roadmap
- Add a REST API and decouple frontend (React/Vue) from server-side templates
- Introduce Docker support (Dockerfile + docker-compose for DB)
- Move file uploads to cloud storage (S3) and serve images via CDN
- Add more integration tests and CI (GitHub Actions)
- Implement pagination, search, and product filtering

## 15) License & Contact
- License: (choose an appropriate license, e.g., MIT) — add `LICENSE` file if desired.
- Contact / Issues: open an issue or reach out to the repo owner for questions or contributions.

---

If you want, I can:
- Add example SQL seed data or a Docker Compose file for MySQL + app
- Add a sample `application.properties` for `dev` and `test` profiles
- Insert an initial-admin SQL snippet into the README

