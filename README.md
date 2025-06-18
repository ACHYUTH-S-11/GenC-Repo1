# Overview
The Shopping Cart Module is a core component of the E-Commerce application, enabling users to manage their potential purchases.

The Shopping Cart Module facilitates the following key functionalities:

* **Adding products:** Users can add desired products to their cart.
* **Viewing items:** Users can see the products currently in their cart.
* **Removing items:** Users can remove products they no longer wish to purchase.
* **Calculating total price:** The module calculates the cumulative price of all items in the cart.

This module integrates with the **Product** and **User** modules to retrieve product details and associate cart items with specific users.

---

# Architecture
The Shopping Cart Module employs a layered architecture, ensuring separation of concerns and maintainability:

* **Controller Layer:** Handles incoming HTTP requests and generates appropriate responses.
* **Service Layer:** Contains the core business logic for all cart operations, coordinating interactions between the repository and other services.
* **Repository Layer:** Manages direct database interactions using JPA (Java Persistence API), abstracting data access details.
* **Model Layer:** Defines the data structures and relationships for application entities such as `CartItem`, `Product`, and `User`.


= The Shopping Cart Module

Overview
--------
The Shopping Cart Module is a vital part of the E-Commerce application. It empowers users to manage their potential purchases by:

* Adding products to their cart.
* Viewing the items currently in their cart.
* Removing items from their cart.
* Calculating the total price of all items in their cart.

This module seamlessly integrates with the Product and User modules to retrieve product specifics and link cart items to individual users.

Architecture
------------
The Shopping Cart Module adheres to a layered architecture, promoting separation of concerns and maintainability:

* **Controller Layer:** This layer is the entry point for all HTTP requests, handling incoming requests and crafting appropriate HTTP responses.
* **Service Layer:** This is where the core business logic for all cart operations resides. It orchestrates interactions between the repository and other services.
* **Repository Layer:** Responsible for direct interactions with the database using JPA (Java Persistence API), abstracting data access details.
* **Model Layer:** Defines the data structures and relationships for the application's entities, such as CartItem, Product, and User.

[plantuml, plantuml_architecture, svg]
----
@startuml
participant "Client (Browser/App)" as Client
actor CartController as Controller
boundary CartService as Service
collections CartItemRepository as Repository
database Database as DB

Client -> Controller: HTTP Request
Controller --> Service: Delegates Request
Service --> Repository: Business Logic\nOrchestrates Services
Repository --> DB: Data Access Logic\nORM (JPA) Object Mapping

DB -- SQL Queries --> DB
DB -- JPA Mapping --> Repository
Repository -- Repository Call --> Service
Service -- Service Call --> Controller
Controller -- HTTP Response --> Client

note right of DB: (product, user, cart_item)

rectangle "Layers" {
    Controller -right-> F[Processes Request\nPrepares Response]
    Service -right-> G[Retrieves/Manages Data\nInteracts with Repositories]
    Repository -right-> H[Performs CRUD Operations\nTranslates Objects to SQL]
    DB -right-> I[Transforms Data\nInteracts with Database Driver]
}

@enduml
----

Endpoints
---------
Base URL: `http://<hostname>:<port>/api/cart`

.API Endpoints
|===
| Endpoint | Method | Description

| `/add` | `POST` | Adds a specified quantity of a product to a user's cart.
| `/remove/{cartItemId}` | `DELETE` | Removes a specific item from the cart using its cartItemId.
| `/view/{userId}` | `GET` | Retrieves all items currently in a user's cart.
| `/total/{userId}` | `GET` | Calculates and returns the total price of all items in a user's cart.
| `/products` (Product Module) | `GET` | Retrieves a list of all available products (While part of the Product module, often accessed for cart operations).
|===

Database Tables
---------------

### `product` Table

.Product Table Schema
|===
| Column Name | Data Type | Description

| `product_id`| `BIGINT` | Primary key
| `name` | `VARCHAR` | Name of the product
| `price` | `DOUBLE` | Price of the product
|===

### `app_user` Table

.App User Table Schema
|===
| Column Name | Data Type | Description

| `user_id` | `BIGINT` | Primary key
| `name` | `VARCHAR` | Name of the user
|===

### `cart_item` Table (Implicitly suggested by the architecture)
This table would likely hold the details of items within a user's cart. While not explicitly defined in your text, based on the functionalities, it would typically have columns like:

.Cart Item Table Schema
|===
| Column Name | Data Type | Description

| `cart_item_id` | `BIGINT` | Primary key
| `user_id` | `BIGINT` | Foreign key referencing `app_user`
| `product_id` | `BIGINT` | Foreign key referencing `product`
| `quantity` | `INT` | Quantity of the product in the cart
|===
