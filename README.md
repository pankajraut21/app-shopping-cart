<p align="center">
    <img src="https://img.shields.io/github/stars/pankajraut21/app-shopping-cart?style=for-the-badge&color=orange">
    <img src="https://img.shields.io/github/forks/pankajraut21/app-shopping-cart?style=for-the-badge&color=purple">
    <img src="https://img.shields.io/github/license/pankajraut21/app-shopping-cart?style=for-the-badge&color=blue">
    <img src="https://img.shields.io/github/issues/pankajraut21/app-shopping-cart?style=for-the-badge&color=red">
    <img src="https://img.shields.io/github/contributors/pankajraut21/app-shopping-cart?style=for-the-badge&color=cyan">
<br>
    <img src="https://img.shields.io/badge/Author-Pankaj Raut-magenta?style=flat-square">
    <img src="https://img.shields.io/badge/Open%20Source-Yes-orange?style=flat-square">
    <img src="https://img.shields.io/badge/Maintained-Yes-cyan?style=flat-square">
    <img src="https://img.shields.io/badge/Made%20In-India-green?style=flat-square">
    <img src="https://img.shields.io/badge/Written%20In-Angular-blue?style=flat-square">
<br>
    <img src="https://github-readme-stats.vercel.app/api/pin/?username=pankajraut21&repo=app-shopping-cart&theme=synthwave">
</p>

(https://github.com/pankajraut21/app-shopping-cart)




Certainly! Here’s a polished and organized version of your technical specifications:

---

### Project Overview

**Framework**: Angular 18

### Core Files and Configuration

- **index.html**: Updated the application title to "Shopping Cart".

- **main.ts**: Configures and provides Angular's `HttpClient` service for dependency injection throughout the application using `provideHttpClient()`.

- **environments (folder)**: Contains environment-specific files with constants for API URLs (e.g., `https://dummyjson.com`).

### Application Structure

#### Root Folder: `app`

- **AppComponent**: 
  - Manages view state (shop or cart) and cart count using a `BehaviorSubject`.
  - Handles view switching via click event bindings.
  - Conditionally displays `app-shop` or `app-cart` components based on the current view.
  - Uses `cartCount$` observable to asynchronously display the cart count.
  
- **HomeComponent**:
  - Acts as a container for content projection, utilizing `ng-content` to project `app-shop` and `app-cart` components based on the current view.

#### Pages Folder: `app/pages`

- **HomeComponent**:
  - Container component demonstrating content projection.

#### Features Folder: `app/features`

- **Cart Module**:
  - **Components**: Contains the `CartComponent`.
    - **Template**:
      - Displays a "Cart" heading.
      - Shows "Your cart is empty" when there are no items.
      - Displays total price of items, formatted as currency.
      - Contains a form for each cart item with fields for image, title, price, discount, and quantity.
      
    - **Class Properties**:
      - `cartItems`: Array of items in the cart.
      - `cartForm`: Form group for managing item quantities.
      - `totalPrice$`: Observable for total price.
      - `subscriptions`: Manages subscriptions to prevent memory leaks.
      
    - **Constructor**: Initializes `CartService`, `FormBuilder`, and sets up the form group.
    
    - **Lifecycle Hooks**:
      - `ngOnInit()`: Loads cart items and subscribes to cart updates.
      - `ngOnDestroy()`: Unsubscribes from all subscriptions.
      
    - **Methods**:
      - `loadCart()`: Loads cart items and initializes form controls.
      - `updateQuantity()`: Updates quantities in the cart service.
      - `removeFromCart()`: Removes items from the cart and reloads.

  - **Directives**:
    - **IntegerOnlyDirective**: Restricts input to integer values.

- **Product Module**:
  - **Components**: Contains the `ShopComponent`.
    - **Template**:
      - Displays a "Shop" heading.
      - Shows a "Loading products..." message while fetching data.
      - Displays products with thumbnail, title, price, discount, and rating. Includes an "Add to Cart" button.
      - Displays "No products available" if no products are found.
      
    - **Class Properties**:
      - `products$`: Observable for product list.
      
    - **Constructor**: Injects `ProductService` and `CartService`.
    
    - **Lifecycle Hook**:
      - `ngOnInit()`: Initializes `products$` observable with product data.
      
    - **Methods**:
      - `addToCart()`: Adds selected products to the cart.
      - `trackByFn()`: Optimizes rendering using product `id`.

#### Core Folder: `app/core`

- **Constants**:
  - **API_ENDPOINTS**: Defines API URLs for product retrieval.

- **Models**:
  - **CartItem**: Interface extending `Product` with an additional `quantity` property.
  - **Product**: Interface representing product data.

- **Services**:
  - **CartService**:
    - Manages cart functionality with observables for cart count and total price.
    - **Methods**:
      - `addToCart(product)`: Adds or updates product in the cart.
      - `removeFromCart(productId)`: Removes product from the cart.
      - `updateCart(product)`: Updates product quantity.
    - **Observables**:
      - `cartCount$`: Total number of products in the cart.
      - `totalPrice$`: Total price of cart items.

  - **ProductService**:
    - Fetches product data from a remote API.
    - **API Configuration**:
      - `apiUrl`: Base URL for API.
      - `productsEndpoint`: Endpoint for fetching products.
    - **HTTP Operations**:
      - `getProducts()`: Fetches and returns product data.
    - **Error Handling**:
      - `handleError(error)`: Manages HTTP errors and returns empty data on failure.

---

This structure provides a clear overview of the project's architecture and functionality, organized by the primary components and services.
