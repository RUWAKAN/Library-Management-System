🏛️ LibraVault: Full-Stack Library Management System
LibraVault is a highly structured, enterprise-grade application designed to bridge theoretical data models with practical engineering execution. It implements a decoupled Client-Server architecture, utilizing a robust Spring Boot backend (REST API) and a responsive, zero-dependency HTML/CSS/JS frontend.

⚙️ Core Architecture Outline
Presentation Layer: A stateless, dark-themed UI featuring asynchronous fetch integration and dynamic DOM manipulation.

Application Layer: A Spring Boot environment managing business logic invariants, global exception handling, and strict Data Transfer Object (DTO) encapsulation.

Persistence Layer: An embedded H2 database executing relational mapping via Spring Data JPA, pre-seeded with sample inventory and member data for immediate testing.

🛠️ Technology Stack & Configuration
Runtime Engine: Java 17

Framework: Spring Boot 3.2.0

Data Persistence: Spring Data JPA / Hibernate (ddl-auto=create-drop)

Database: H2 In-Memory Database

Validation: Jakarta Bean Validation (@NotBlank, @Email, @Min)

Boilerplate Reduction: Lombok (@Data, @Builder, @NoArgsConstructor)

The system utilizes application.properties to configure an ephemeral environment perfect for local development:

Server Port: 8080

H2 Web Console: Enabled at http://localhost:8080/h2-console (Username: sa, Password: [blank])

Data Seeding: The DataInitializer implements CommandLineRunner to automatically populate the database on startup with:

10 distinct Book titles (e.g., Clean Code, 1984, The Great Gatsby).

4 active Members (Standard, Premium, and Student tiers).

4 historical and active Transactions (including overdue logic).

🚀 Execution Flow & Deployment Steps
To initialize and deploy the local development server, execute the following commands in your terminal.

Initialize the Workspace: Ensure all source files (pom.xml, Java controllers, and the frontend index.html) are located in their respective directories.

Compile and Run the Java Engine:

Bash
mvn spring-boot:run
Verify Server Lifecycle: The terminal will output the initialization sequence and the DataInitializer log: ✔ Sample data seeded....

Launch the Client: Open index.html directly in any modern web browser. The frontend will ping http://localhost:8080/api/dashboard and populate the UI using the pre-seeded H2 database.

📡 RESTful API Documentation
The backend enforces meticulous validation and error handling (400 Bad Request, 404 Not Found, 409 Conflict) managed by the @RestControllerAdvice global exception interceptor. Data is strictly passed via request/response DTOs to protect database entity structures.

📖 Book Management API (/api/books)
Retrieves the library's inventory. Supports query parameters for targeted data fetching.

Parameters: * ?search={query} (Searches Title or Author)

?available=true (Filters for availableCopies > 0)

Response: 200 OK (Returns List<BookResponse>)

Fetches a specific book's data object by its primary key.

Response: 200 OK or 404 Not Found

Creates a new book entity.

Payload (BookRequest): title, author, totalCopies (Required). genre, year, isbn, publisher (Optional).

Response: 201 Created

Updates an existing book's information. Dynamically recalculates availableCopies if totalCopies is altered.

Response: 200 OK

Removes a book from the database.

Response: 200 OK

Constraint: Deletion will block and throw an exception if availableCopies is less than totalCopies (meaning copies are currently issued to members).

👤 Member Management API (/api/members)
Retrieves a complete ledger of registered members or filters them via query parameters.

Parameters: ?search={query} (Optional)

Response: 200 OK (Returns List<MemberResponse>)

Creates a new member entity.

Payload (MemberRequest): name, email (Required).

Response: 201 Created

Constraint: Throws 409 Conflict if the email is already registered.

Removes a member from the database.

Response: 200 OK

Constraint: Deletion will block and throw a 400 Bad Request / Conflict if the member currently possesses active or overdue book transactions.
