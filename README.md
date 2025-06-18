Overview
The Shopping Cart Module is a vital part of the E-Commerce application. It empowers users to manage their potential purchases by:

Adding products to their cart.
Viewing the items currently in their cart.
Removing items from their cart.
Calculating the total price of all items in their cart.
This module seamlessly integrates with the Product and User modules to retrieve product specifics and link cart items to individual users.

Architecture
The Shopping Cart Module adheres to a layered architecture, promoting separation of concerns and maintainability:

Controller Layer: This layer is the entry point for all HTTP requests, handling incoming requests and crafting appropriate HTTP responses.
Service Layer: This is where the core business logic for all cart operations resides. It orchestrates interactions between the repository and other services.
Repository Layer: Responsible for direct interactions with the database using JPA (Java Persistence API), abstracting data access details.
Model Layer: Defines the data structures and relationships for the application's entities, such as CartItem, Product, and User.

+---------------------+    HTTP Request    +-------------------+    Service Call    +-----------------+    Repository Call    +---------------------+    JPA Mapping    +------------+
| Client (Browser/App)|------------------->| CartController    |------------------->| CartService     |---------------------->| CartItemRepository  |----------------->| Database   |
+---------------------+                    +-------------------+                    +-----------------+                      +---------------------+                  | (product,  |
          ^                                        |                                        |                                          |                        SQL Queries   |  user,     |
          |                                        | Delegates Request                      | Business Logic                           | Data Access Logic      <------------->|  cart_item)|
          |                                        |                                        | Orchestrates Services                    | ORM (JPA)              Object Mapping |            |
          | HTTP Response                          |                                        |                                          |                        <------------- |            |
          +----------------------------------------+----------------------------------------+------------------------------------------+------------------------------------------+
                                                   |                                        |                                          |
                                                   V                                        V                                          V
                                          Processes Request                       Retrieves/Manages Data                    Performs CRUD Operations
                                          Prepares Response                       Interacts with Repositories               Translates Objects to SQL
                                                                                  Transforms Data                           Interacts with Database Driver

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

