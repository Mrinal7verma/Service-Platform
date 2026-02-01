# Service Platform Backend

A scalable Node.js backend service for managing categories, items, pricing,
availability, bookings, and add-ons — similar to what a real restaurant,
booking, or SaaS product would use.

This project focuses on **business logic, system design, and maintainability**
rather than simple CRUD APIs.

---

## 🚀 Features

### Catalog Management
- Categories and optional subcategories
- Items can belong to **either a category or subcategory**
- Soft deletes using `is_active`
- Automatic inactive propagation for child entities

### Pricing Engine
Each item supports **exactly one pricing strategy**:
- Static pricing
- Tiered pricing
- Complimentary items
- Discounted pricing (flat / percentage)
- Dynamic time-based pricing

A dedicated pricing resolver dynamically calculates:
- Base price
- Discount (if applicable)
- Tax
- Final payable amount

### Tax Inheritance
- Items inherit tax from subcategory
- Subcategory inherits tax from category
- Category tax updates automatically reflect across dependent items  
(no redundant updates required)

### Availability & Booking
- Optional bookable items
- Configurable available days and time slots
- Slot availability API
- Booking with double-booking prevention

### Add-ons System
- Optional and mandatory add-ons
- Add-on groups (e.g. choose 1 of many)
- Add-ons dynamically affect final price

### Search & Listing
- Pagination and sorting
- Text search
- Filters by price, category, tax applicability, and active status

---

## 🧠 Architecture Overview

The project follows a layered architecture:

- **Routes** – HTTP request handling
- **Controllers** – Input validation and response shaping
- **Services** – Core business logic (pricing, tax, booking)
- **Models** – Data schema and relationships
- **Utils** – Shared helpers and pricing resolvers

This separation keeps business logic isolated and testable.

---

## 🗃️ Data Modeling Decisions

- Categories, subcategories, and items are separate entities
- Items reference their direct parent only (category OR subcategory)
- Tax fields are optional at lower levels to support inheritance
- Pricing configuration is stored as structured data, not computed values

This avoids data duplication and keeps pricing flexible.

---

## 🧮 Pricing Engine Design

Pricing is **resolved at request time**, not stored in the database.

A single pricing resolver:
1. Identifies the pricing type
2. Applies the correct pricing rules
3. Calculates tax based on inheritance
4. Returns a detailed price breakdown

This approach ensures:
- Correct dynamic pricing
- Easy extensibility
- No stale pricing data

---

## ⚖️ Tradeoffs & Simplifications

- Authentication and authorization were intentionally excluded
- Focused on backend correctness over UI or deployment
- Some validations were simplified to prioritize core logic clarity

---

## 🛠️ Tech Stack

- Node.js
- Express.js
- MongoDB (can be swapped easily)
- REST APIs

**Optional/Bonus:**
- TypeScript
- Schema validation
- Service-layer separation

---

## ▶️ Running Locally

```bash
git clone <repo-url>
cd service-platform
npm install
npm run dev
