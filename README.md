
### [Technical Specifications](#technical-specifications)

1. [Core Files](#core-files)
    - [index.html](#indexhtml)
    - [main.ts](#maints)
    - [Environments](#environments-folder)
2. [Application Structure](#application-structure)
    - [Core](#core-folder)
    - [Features](#features-folder)
    - [Pages](#pages-folder)
3. [Modules](#modules)
    - [AppComponent](#appcomponent)
    - [Cart Module](#cart-module)
        - [Components](#cart-module-components)
        - [Directives](#cart-module-directives)
        - [Services](#cart-module-services)
    - [Product Module](#product-module)
        - [Components](#product-module-components)
        - [Services](#product-module-services)
4. [Core Functionality](#core-functionality)
    - [Constants](#constants)
    - [Models](#models)

---

### Technical Specifications

#### [Core Files](#core-files)

- **[index.html](#indexhtml)**: Updated the application title to "Shopping Cart".
- **[main.ts](#maints)**: Configures Angular's `HttpClient` service using the `provideHttpClient()` function, making it available for dependency injection throughout the application.
- **[Environments](#environments-folder)**: Contains environment-specific configuration files with constants for API URLs, such as `https://dummyjson.com`.

#### [Application Structure](#application-structure)

- **[Core](#core-folder)**: Contains essential application-wide functionality.
  - **[Constants](#constants)**: Defines constants for API endpoints.
  - **[Models](#models)**: Provides data models like `CartItem` and `Product`.

- **[Features](#features-folder)**: Contains feature-specific modules and components.
  - **[Cart Module](#cart-module)**: Manages cart-related functionality.
    - **[Services](#services)**: Includes services such as `CartService`.
  - **[Product Module](#product-module)**: Handles product-related functionality.
    - **[Services](#services)**: Includes services such as `ProductService`.

- **[Pages](#pages-folder)**: Contains page-level components for content projection and layout.
  - **HomeComponent**: Acts as a container for projecting other components.

#### [Modules](#modules)

- **[AppComponent](#appcomponent)** (`app` folder):
  - **Description**: The root component of the application.
  - **Purpose**:
    - Manages the current view state (shop or cart) using a `BehaviorSubject`.
    - Handles cart count using an observable (`cartCount$`).
    - Uses click event bindings to switch views between `app-shop` and `app-cart` components.
    - Conditionally displays the `app-shop` or `app-cart` component based on the current view.
    - Projects content using Angular's content projection (`ng-content`).
  - **Component Structure**:
    - **HTML Template**:
      - **Title**: Displays the application title.
      - **Navigation**: Uses a navigation bar with buttons to switch between views.
      - **Content Projection**: Includes content projection using `ng-content` to display either `app-shop` or `app-cart` based on the view state.
    - **TypeScript Code**:
      - **Class Properties**:
        - `view`: Tracks the current view state (shop or cart).
        - `cartCount$`: Observable for the cart count.
      - **Constructor**: Initializes services and sets up view management.
      - **Methods**:
        - `displayShop(): void`: Set the current view to `shop` and displays `shop`.
        - `displayCart(): void`: Set the current view to `cart` and displays `cart`.

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
        - `ngOnInit()`: Loads cart items, `totalPrice$`: Observable for fetching the total price. and subscribes to updates.
        - `ngOnDestroy()`: Unsubscribes from all active subscriptions.
      - **Methods**:
        - `loadCart()`: Loads items from `CartService` and initializes form controls for item quantities.
        - `updateQuantity()`: Updates item quantities in the cart service when changed.
        - `removeFromCart()`: Removes an item from the cart and reloads the cart items.
  - **[Directives](#cart-module-directives)**:
      - **IntegerOnlyDirective**: Restricts input to integer values only, cart component uses this with minimum quantity value `1`.
  - **[Services](#cart-module-services)**:
    - **[CartService](#cartService)**:
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

- **[Product Module](#product-module)** (`app/features/product`):
  - **[Components](#product-module-components)**:
    - **ShopComponent**:
      - **Template**:
        - **Title**: Displays "Shop".
        - **Product List**:
          - **Loading State**: Shows "Loading products..." while fetching data.
          - **Product Display**:
            - Displays each product with its thumbnail, title, price, discount badge, rating, and an "Add to Cart" button if available.
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
  - **[Services](#product-module-services)**:
    - **[ProductService](#productservice)**:
      - **Purpose**: Fetches product data from a remote API.
      - **API Configuration**:
        - `apiUrl`: Base URL for the API.
        - `productsEndpoint`: Endpoint for fetching product data.
      - **HTTP Operations**:
        - `getProducts(): Observable<Product[]>`: Retrieves a list of products, limited to 50 items. Returns an observable of product arrays, maps the response to extract data, and handles errors.
      - **Error Handling**:
        - `handleError(error: any): Observable<Product[]>`: Handles HTTP errors, displays an alert with the error message, and returns an empty array to ensure continued application functionality.


#### [Core Functionality](#core-functionality)

- **[Constants](#constants)**:
  - **API_ENDPOINTS**: Contains constants for API URLs used in fetching product data.

- **[Models](#models)**:
  - **CartItem**: Interface extending `Product` with an additional `quantity` property.
  - **Product**: Interface representing product data structure.




