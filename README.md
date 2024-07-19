
### [Technical Specifications](#technical-specifications)

1. [Core Files](#core-files)
    - [index.html](#indexhtml)
    - [main.ts](#maints)
    - [Environments (folder)](#environments-folder)
2. [Application Structure](#application-structure)
    - [Core (folder)](#core-folder)
    - [Features (folder)](#features-folder)
    - [Pages (folder)](#pages-folder)
3. [Features](#features)
    - [Cart Module](#cart-module)
        - [Components](#cart-module-components)
        - [Directives](#cart-module-directives)
    - [Product Module](#product-module)
        - [Components](#product-module-components)
4. [Core Functionality](#core-functionality)
    - [Constants](#constants)
    - [Models](#models)
    - [Services](#services)
        - [CartService](#cartservice)
        - [ProductService](#productservice)

---

### Technical Specifications

#### [Core Files](#core-files)

- **[index.html](#indexhtml)**: Updated the application title to "Shopping Cart".
- **[main.ts](#maints)**: Configures Angular's `HttpClient` service using the `provideHttpClient()` function, making it available for dependency injection throughout the application.
- **[Environments (folder)](#environments-folder)**: Contains environment-specific configuration files with constants for API URLs, such as `https://dummyjson.com`.

#### [Application Structure](#application-structure)

- **[Core (folder)](#core-folder)**: Contains essential application-wide functionality.
  - **[Constants (folder)](#constants)**: Defines constants for API endpoints.
  - **[Models (folder)](#models)**: Provides data models like `CartItem` and `Product`.
  - **[Services (folder)](#services)**: Includes services such as `CartService` and `ProductService`.

- **[Features (folder)](#features-folder)**: Contains feature-specific modules and components.
  - **[Cart Module](#cart-module)**: Manages cart-related functionality.
  - **[Product Module](#product-module)**: Handles product-related functionality.

- **[Pages (folder)](#pages-folder)**: Contains page-level components for content projection and layout.
  - **HomeComponent**: Acts as a container for projecting other components.

#### [Features](#features)

- **[Cart Module](#cart-module)** (`app/features/cart`):
  - **[Components](#cart-module-components)**:
    - **CartComponent**:
      - **Template**:
        - **Title**: Displays "Cart".
        - **Empty Cart Message**: Shows "Your cart is empty" when no items are present.
        - **Total Price**: Displays the total price of cart items formatted as currency, defaulting to "0.00" if no total price is available.
        - **Cart Form**: Features a form for each cart item, including fields for image, title, price, discount, and quantity.
      - **Class Properties**:
        - `cartItems`: Array of items currently in the cart.
        - `cartForm`: Form group for managing quantities of cart items.
        - `totalPrice$`: Observable for the total price of items in the cart.
        - `subscriptions`: Manages subscriptions to prevent memory leaks.
      - **Constructor**: Initializes `CartService` and `FormBuilder`, setting up the form group.
      - **Lifecycle Hooks**:
        - `ngOnInit()`: Loads cart items and subscribes to updates.
        - `ngOnDestroy()`: Unsubscribes from all active subscriptions.
      - **Methods**:
        - `loadCart()`: Loads items from `CartService` and initializes form controls for item quantities.
        - `updateQuantity()`: Updates item quantities in the cart service when changed.
        - `removeFromCart()`: Removes an item from the cart and reloads the cart items.
    - **[Directives](#cart-module-directives)**:
      - **IntegerOnlyDirective**: Restricts input to integer values only.

  - **[Product Module](#product-module)** (`app/features/product`):
    - **[Components](#product-module-components)**:
      - **ShopComponent**:
        - **Template**:
          - **Title**: Displays "Shop".
          - **Product List**:
            - **Loading State**: Shows "Loading products..." while fetching data.
            - **Product Display**:
              - Displays each product with its thumbnail, title, price, discount, rating, and an "Add to Cart" button if available.
              - Shows "No products available" if no products are found.
          - **Track By Function**: Uses `trackByFn` to uniquely identify products by `id`, optimizing rendering performance.
        - **Class Properties**:
          - `products$`: Observable for fetching the list of products.
        - **Constructor**: Injects `ProductService` for product data and `CartService` for cart management.
        - **Lifecycle Hook**:
          - `ngOnInit()`: Assigns the observable from `ProductService.getProducts()` to `products$`.
        - **Methods**:
          - `addToCart()`: Adds a selected product to the cart using `CartService`.
          - `trackByFn()`: Improves rendering efficiency by tracking products by their `id`.

#### [Core Functionality](#core-functionality)

- **[Constants](#constants)**:
  - **API_ENDPOINTS**: Contains constants for API URLs used in fetching product data.

- **[Models](#models)**:
  - **CartItem**: Interface extending `Product` with an additional `quantity` property.
  - **Product**: Interface representing product data structure.

- **[Services](#services)**:
  - **[CartService](#cartservice)**:
    - **Purpose**: Manages shopping cart functionality, providing observables for cart count and total price.
    - **Methods**:
      - `addToCart(product: Product): void`: Adds a product to the cart, increasing its quantity if it already exists.
      - `removeFromCart(productId: number): void`: Removes a product from the cart by its ID.
      - `updateCart(product: CartItem): void`: Updates the quantity of an existing cart item.
    - **Observables**:
      - `cartCount$`: Observable for tracking the total number of items in the cart.
      - `totalPrice$`: Observable for tracking the total price of cart items.
    - **Internal Methods**:
      - `updateCartCount()`: Updates the cart count based on total product quantity or unique product count.
      - `updateTotalPrice()`: Calculates and updates the total price of the items in the cart.

  - **[ProductService](#productservice)**:
    - **Purpose**: Fetches product data from a remote API.
    - **API Configuration**:
      - `apiUrl`: Base URL for the API.
      - `productsEndpoint`: Endpoint for fetching product data.
    - **HTTP Operations**:
      - `getProducts(): Observable<Product[]>`: Retrieves a list of products, limited to 50 items. Returns an observable of product arrays, maps the response to extract data, and handles errors.
    - **Error Handling**:
      - `handleError(error: any): Observable<Product[]>`: Handles HTTP errors, displays an alert with the error message, and returns an empty array to ensure continued application functionality.

---
