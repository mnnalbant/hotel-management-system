# Hotel Management System (JDBC & MySQL)

A **Hotel Management System** developed with a focus on database integrity, relational theory, and robust backend architecture. This application utilizes **Java JDBC** to interface with a **MySQL** database, strictly adhering to **3rd Normal Form (3NF)** to ensure data consistency and eliminate redundancy.

---

## 🏛️ System Architecture & Logic

### 1. Database Normalization (3NF)

The core of this project is its relational design. The database is structured to eliminate partial and transitive dependencies:

* **Room Management:** Functional dependencies are enforced so that `RoomID` uniquely determines `RoomType`, `Price`, and `Status`.
* **User Hierarchy:** Implements an **ISA Relationship** (Inheritance). A base `User` table stores common credentials, while specialized tables for `Guest`, `Administrator`, `Receptionist`, and `Housekeeping` store role-specific data.
* **Integrity Constraints:** Utilizes `FOREIGN KEY` constraints with strategic `ON DELETE` rules (Restrict/Cascade) to prevent orphaned records in bookings or housekeeping schedules.

### 2. Role-Based Access Control (RBAC)

The system features four distinct interfaces, ensuring that business logic is isolated by user responsibility:

* **Administrator:** * Full CRUD control over Room inventory.
* User account management (exclusive rights to add staff).
* **Analytical Reporting:** Generates revenue reports and identifies the "Most Booked Room Types" using complex SQL aggregations.


* **Receptionist:**
* Acts as the operational bridge.
* Manages the booking lifecycle: confirming/denying requests based on real-time availability.
* Triggers the housekeeping workflow by assigning cleaning tasks upon check-out.


* **Guest:**
* Self-service booking engine.
* Dynamic availability search based on guest count and date ranges.
* Financial tracking for advanced or check-out payments.


* **Housekeeping:**
* Focused operational view: manages room cleanliness without access to sensitive guest PII (Personally Identifiable Information).



---

## 🚀 Key Features

### 📅 Advanced Booking Engine

Unlike basic systems, this engine prevents **Overbooking** entirely through SQL-side validation. It checks temporal overlaps to ensure that for any given `RoomID`, no two reservations occupy the same time frame. It also supports capacity-based queries (e.g., matching a party of 4 to either two double rooms or one family suite).

### 🧹 Housekeeping Workflow

Integrated scheduling ensures rooms are never double-booked while "Dirty."

1. **Check-out:** Room status automatically updates to `Pending Cleaning`.
2. **Assignment:** Receptionist assigns the task to a specific Housekeeper.
3. **Completion:** Once the Housekeeper marks the task as `Completed`, the room status reverts to `Available` in the global inventory.

### 📊 Business Intelligence & Reporting

The Administrator dashboard leverages the power of SQL for high-level insights:

* **Revenue Tracking:** Calculates income across different hotels in the chain.
* **Employee Logs:** Monitors housekeeping efficiency and receptionist booking volumes.

---

## 🛠️ Technical Implementation

* **Logic-Heavy SQL:** Designed to avoid "Hacky Java" solutions. Calculations, availability checks, and data validation are handled within the MySQL layer to maintain a single source of truth.
* **Robust Error Handling:** The application implements comprehensive `try-catch-finally` blocks for JDBC operations, ensuring that database connections are always closed and SQL exceptions provide meaningful feedback to the user rather than crashing the program.
* **Security:** Administrator creation is restricted to the Database Manager level, preventing unauthorized privilege escalation through the UI.

---

## 🖥️ User Interface

The system utilizes a **Terminal Menu-Driven Interface** designed for speed and clarity.

* **Main Menu:** Role selection with input validation.
* **Secondary Menus:** Context-aware options specific to the logged-in user type.
* **State Persistence:** Users can perform multiple actions and return to sub-menus until they explicitly choose to exit.
