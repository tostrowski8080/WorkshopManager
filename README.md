# WorkshopManager

A web application built with **ASP.NET Core** for managing a car repair shop. It facilitates team coordination, repair order management, parts inventory tracking, and customer relations.

*(Note: While this documentation is in English, the application's user interface is primarily in Polish).*

## Technologies Used
- **Framework:** ASP.NET Core (MVC / Razor Pages)
- **Object Mapping:** [Mapperly](https://mapperly.github.io/mapperly/)
- **Document Generation:** QuestPDF (PDF reports)
- **Background Processes:** Hosted Services / BackgroundService
- **CI/CD:** GitHub Actions

## Features

The application is built on a role-based architecture, restricting access to specific modules depending on the logged-in user's permissions.

### Role & Permission System

* **Customer (Klient)** * View personal repair history.
    * Upload photos of their vehicles.
    * Add comments to ongoing repair orders.
* **Receptionist (Recepcjonista)**
    * Register new customers and add their vehicles to the system.
    * Create new repair orders.
    * Link physical customer profiles with their online application accounts.
    * Manage inventory (update parts stock levels).
    * Generate individual PDF repair history reports for customers.
* **Mechanic (Mechanik)**
    * View and update the status of assigned repair orders.
    * Log performed repair tasks and services.
    * Declare specific parts used for a repair order.
* **Admin**
    * Access to all features available to other roles.
    * Manage user accounts and assign roles.
    * Manage the main parts catalog (add new items to the database).
    * Generate global, monthly PDF service reports.

## Project Structure

The project follows standard ASP.NET architectural patterns to maintain a clean codebase:

```text
/WorkshopManager
├── Controllers/         # Controllers handling requests and business logic
├── DTOs/                # Data Transfer Objects
├── Models/              # Domain models and database entities
├── Services/            # Business logic and services (including Background Services)
├── Mappers/             # Object mapping configurations (Mapperly)
├── PdfRaports/          # Classes responsible for generating documents using QuestPDF
├── Pages/               # Razor Pages views for dedicated features
├── Views/               # Razor views for MVC controllers
├── wwwroot/             # Static files (CSS, JS, images)
│   └── uploads/         # Directory for storing uploaded vehicle photos
├── Data/                # Database context (DbContext)
└── Program.cs           # Application pipeline and Dependency Injection (DI) configuration
```

## Background Services

The application features a built-in automated notification system. Utilizing the `OpenOrderReportBackgroundService`, the system automatically generates and sends an aggregated email report (e.g., a summary of all open repair orders) at a specified time interval (hourly). 
*(Note: The target email address can be configured within the system settings).*

## CI/CD - GitHub Actions

The repository includes a fully automated CI/CD pipeline. The workflow triggers automatically on every `push` or `pull request` to the `main` branch.

Pipeline steps:
1. `dotnet restore` – Restores required NuGet packages.
2. `dotnet build` – Compiles the application, ensuring the codebase is free of build errors.
3. `dotnet test` – Runs the suite of unit tests to verify system integrity.

---

## Local Setup

To run the project on your local machine, follow the instructions below:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/tostrowski8080/ProjektNet.git
   ```

2. **Configure the database:**
   Update the connection string in the `appsettings.json` file, pointing it to your local SQL server instance:
   ```json
   "ConnectionStrings": {
     "DefaultConnection": "Server=YOUR_SERVER;Database=WorkshopManagerDb;Trusted_Connection=True;MultipleActiveResultSets=true"
   }
   ```

3. **Apply database migrations:**
   ```bash
   dotnet ef database update
   ```

4. **Run the application:**
   ```bash
   dotnet run
   ```
