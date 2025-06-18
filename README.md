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

```mermaid
graph TD
    A[Client (Browser/App)] -->|HTTP Request| B(CartController)
    B -->|Delegates Request| C(CartService)
    C -->|Business Logic Orchestrates Services| D(CartItemRepository)
    D -->|Data Access Logic ORM (JPA) Object Mapping| E(Database)

    subgraph Layers
        B --- F[Processes Request Prepares Response]
        C --- G[Retrieves/Manages Data Interacts with Repositories]
        D --- H[Performs CRUD Operations Translates Objects to SQL]
        E --- I[Transforms Data Interacts with Database Driver]
    end

    E -- SQL Queries --> E(Database)
    E -- JPA Mapping --> D
    D -- Repository Call --> C
    C -- Service Call --> B
    B -- HTTP Response --> A

    note right of E: (product, user, cart_item)

Endpoints
Base URL: http://<host>:<port>/api/cart

Add to Cart
Endpoint: /add
Method: POST
Description: Adds a specified quantity of a product to a user's cart.

Remove from Cart
Endpoint: /remove/{cartItemId}
Method: DELETE
Description: Removes a specific item from the cart using its cartItemId.

View Cart
Endpoint: /view/{userId}
Method: GET
Description: Retrieves all items currently in a user's cart.

Get Total Price
Endpoint: /total/{userId}
Method: GET
Description: Calculates and returns the total price of all items in a user's cart.

Get All Products
Endpoint: /products
Method: GET
Description: Retrieves a list of all available products. (While part of the Product module, often accessed for cart operations).


Database Tables
1. product
Column Name	Data Type	Description
product_id	BIGINT	Primary key
name	VARCHAR	Name of the product
price	DOUBLE	Price of the product

2. app_user
Column Name	Data Type	Description
user_id	BIGINT	Primary key
name	VARCHAR	Name of the user

2. app_user
Column Name	Data Type	Description
user_id	BIGINT	Primary key
name	VARCHAR	Name of the user

