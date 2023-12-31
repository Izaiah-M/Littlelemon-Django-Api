# Littlelemon Restaurant API

This API provides functionality for managing a restaurant's menu-(In particular littlelemon, but can be adjusted to fit any), orders, and users. It is built using the Django Rest Framework (DRF).

## Models

The API uses the following models:

### Category

- `slug` (SlugField): A unique slug for the category.
- `title` (CharField): The title of the category.

### MenuItem

- `title` (CharField): The title of the menu item.
- `price` (DecimalField): The price of the menu item.
- `featured` (BooleanField): Indicates if the menu item is featured.
- `category` (ForeignKey to Category): The category to which the menu item belongs.

### Cart

- `user` (ForeignKey to User): The user who added the menu item to the cart.
- `menuitem` (ForeignKey to MenuItem): The menu item added to the cart.
- `quantity` (SmallIntegerField): The quantity of the menu item in the cart.
- `unit_price` (DecimalField): The unit price of the menu item.
- `price` (DecimalField): The total price of the menu item in the cart.

### Order

- `user` (ForeignKey to User): The user who placed the order.
- `delivery_crew` (ForeignKey to User, nullable): The user assigned as the delivery crew for the order.
- `status` (BooleanField): The status of the order.
- `total` (DecimalField): The total price of the order.
- `date` (DateField): The date of the order.

### OrderItem

- `order` (ForeignKey to Order): The order to which the order item belongs.
- `menuitem` (ForeignKey to MenuItem): The menu item included in the order.
- `quantity` (SmallIntegerField): The quantity of the menu item in the order.
- `unit_price` (DecimalField): The unit price of the menu item in the order.
- `price` (DecimalField): The total price of the menu item in the order.

## Endpoints

The API provides the following endpoints:

### User Registration and Token Generation

- `POST /api/users`: Creates a new user with name, email, and password.
- `GET /api/users/me`: Displays information about the current user.
- `POST /token/login`: Generates access tokens for authentication.

### Menu Items

- `GET /api/menu-items`: Lists all menu items (accessible by customers and delivery crew).
- `POST /api/menu-items`: Denies access and returns 403 - Unauthorized (forbidden).
- `GET /api/menu-items/{menuItem}`: Lists a single menu item (accessible by customers and delivery crew).
- `POST /api/menu-items/{menuItem}`: Denies access and returns 403 - Unauthorized.
- `GET /api/menu-items` (Manager Only): Lists all menu items (accessible by managers).
- `POST /api/menu-items` (Manager Only): Creates a new menu item and returns 201 - Created.
- `GET /api/menu-items/{menuItem}` (Manager Only): Lists a single menu item (accessible by managers).
- `PUT /api/menu-items/{menuItem}` (Manager Only): Updates a single menu item.
- `PATCH /api/menu-items/{menuItem}` (Manager Only): Updates a single menu item.
- `DELETE /api/menu-items/{menuItem}` (Manager Only): Deletes a menu item.

### User Group Management Endpoints

#### /api/groups/manager/users

- Role: Manager
- Method: GET
- Purpose: Returns all managers.

#### /api/groups/manager/users

- Role: Manager
- Method: POST
- Purpose: Assigns the user in the payload to the manager group and returns 201 - Created.

#### /api/groups/manager/users/{userId}

- Role: Manager
- Method: DELETE
- Purpose: Removes this particular user from the manager group and returns 200 - Success if everything is okay. If the user is not found, returns 404 - Not found.

#### /api/groups/delivery-crew/users

- Role: Manager
- Method: GET
- Purpose: Returns all delivery crew.

#### /api/groups/delivery-crew/users

- Role: Manager
- Method: POST
- Purpose: Assigns the user in the payload to the delivery crew group and returns 201 - Created.

#### /api/groups/delivery-crew/users/{userId}

- Role: Manager
- Method: DELETE
- Purpose: Removes this user from the delivery crew group and returns 200 - Success if everything is okay. If the user is not found, returns 404 - Not found.

### Cart Management Endpoints

#### /api/cart/menu-items

- Role: Customer
- Method: GET
- Purpose: Returns current items in the cart for the current user token.

#### /api/cart/menu-items

- Role: Customer
- Method: POST
- Purpose: Adds the menu item to the cart. Sets the authenticated user as the user ID for these cart items.

#### /api/cart/menu-items

- Role: Customer
- Method: DELETE
- Purpose: Deletes all menu items created by the current user token.

...

### Order Management Endpoints

#### /api/orders

- Role: Customer
- Method: GET
- Purpose: Returns all orders with order items created by this user.

#### /api/orders

- Role: Customer
- Method: POST
- Purpose: Creates a new order item for the current user. Gets current cart items from the cart endpoints and adds those items to the order items table. Then deletes all items from the cart for this user.

#### /api/orders/{orderId}

- Role: Customer
- Method: GET
- Purpose: Returns all items for this order ID. If the order ID doesn't belong to the current user, it displays an appropriate HTTP error status code.

#### /api/orders

- Role: Manager
- Method: GET
- Purpose: Returns all orders with order items by all users.

...

#### /api/orders/{orderId}

- Role: Manager
- Method: DELETE
- Purpose: Deletes this order.

#### /api/orders

- Role: Delivery crew
- Method: GET
- Purpose: Returns all orders with order items assigned to the delivery crew.

#### /api/orders/{orderId}

- Role: Delivery crew
- Method: PATCH
- Purpose: A delivery crew can use this endpoint to update the order status to 0 or 1. The delivery crew will not be able to update anything else in this order.

## Usage

To use this API, follow the steps below:

1. Install the necessary dependencies and set up the Django project.
2. Create the required user groups (Manager and Delivery crew) and assign users to these groups.
3. Make requests to the respective endpoints using the appropriate HTTP methods and required parameters.
4. Handle the responses and status codes returned by the API for success and error cases.
5. Ensure proper authentication by including the user token in the request headers.
6. Implement error handling for cases such as non-existing items, unauthorized requests, and invalid data in POST, PUT, or PATCH requests.
7. Refer to the specific endpoint documentation for more details on each request's purpose, required roles, and expected HTTP status codes.
