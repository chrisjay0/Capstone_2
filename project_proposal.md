# Summary:

This app enables users to curate a personalized grocery list and identifies the most cost-effective one-stop grocery store to purchase their entire list.

---

# I. Core Features:

1. **User Registration and Authentication:** Implement secure account creation and management for a personalized user experience.
2. **Geolocation:** Utilize geolocation data to identify nearby grocery stores or allow the user to manually input a zip code for location specification.
3. **API Interactions:** Use API calls to fetch real-time price data from the identified nearby stores.
4. **Search and List Building:** Provide fast and accurate search functionality for grocery items, enabling users to add items to their shopping list directly from the dynamically populated results on the search bar.
5. **Real-Time Price Comparison:** Analyze real-time price data for items in the user's list to identify the most cost-effective store option, ensuring graceful handling of API rate limits and errors.

---

# II. User Flows and Interactions

## User Flows:

- **Registration and Login:** Users create an account and log in securely. Clear error messages guide users if issues arise.
- **Profile and Preferences:** Users can update their profile and set preferences, such updating their profile or saved zip code.
- **Location Setting:** Users allow geolocation tracking or manually enter a zip code to identify nearby grocery stores.
- **Empty State Handling:** Guidance is provided when the user's list is empty.
- **Item Search:** Users search for grocery items using a dynamic search bar. As they type, a dropdown menu appears with matching items
- **List Building:** Users can click on an item in the search dropdown to add it directly to their shopping list. Users can edit the list as needed.
- **Price Comparison:** Users initiate a price comparison when their list is complete. The app fetches real-time price data and displays the most cost-effective store based on the sum cost of the user's list.
- **Feedback and Support:** Users can provide feedback and access a help or FAQ section for assistance.

## Other User Interaction Items:

- **Accessibility and Responsiveness:** The app is accessible to all users and optimized for various devices.
- **Error Handling:** Informative error messages and loading indicators guide users during interactions.
- **Security:** The app protects against common web vulnerabilities and handles user data securely.

---

# III. Server, API, and Other Non-User Interactions

- **Test-driven development & Deployment:**
  - Develop tests for each feature and function to organize development, to ensure the application works as expected.
  - Set up process, and documentation, for automated testing and deployment to streamline the development process.
- **Server Setup and Security:**

  - The server will be set up using Django, with secure endpoints for user registration, login, and data retrieval.
  - Django's built-in authentication system will be used for user password hashing and session management.
  - Implement data validation and sanitization for all user inputs and API responses to prevent security vulnerabilities.

- **Database Interactions:**

  - Django's Object-Relational Mapping (ORM) system will be used to design efficient database schemas and handle data storage and retrieval.
  - PostgreSQL will be used to store user data, including profile information, preferences, shopping lists, and log live prices collected via APIs as lists are build.
  - These live prices collected via APIs as lists are build are used to determine the best price for the sum of the user's list when the user initiates the price comparison.

- **Geolocation API:**
  - A Geolocation API will be used to get the user's location data, which will be used to identify nearby grocery stores.
- **Grocery Store API Interactions:**
  - The Django server will interact with various grocery store APIs to fetch real-time price data.
  - This data fetch process will happen _as_ the user builds their list so it is available immediately when the user initiates the price comparison.
  - Implement rate limiting handling and caching strategies to optimize API interactions and improve performance.
- **Search Functionality:**
  - A search functionality will be implemented on the server-side to fetch grocery items from the APIs based on user queries.
  - List items can be added directly from the search bar.
  - As list items are added their prices will be collected with API calls and logged for reference later when the user initiates the price comparison.
- **Price Comparison Logic:**
  - Server-side logic will be implemented to analyze the price data fetched from the APIs and calculate the total cost of the shopping list at each store.
  - The results will be sent to the client-side for display when the user initiates the price comparison.
- **Frontend Development:**
  - The frontend will be developed using React.js with Next.js for directory based routing system.
  - MaterialUI will be used for designing a responsive and accessible user interface.
- **Feedback & Error Logging:**
  - Set up an endpoint for user feedback and use the data for continuous improvement.
  - Implement server-side error logging and monitoring.

---

# IV. Routes, Endpoints, and Templates

## Backend API Routes (Django)

1. **User Registration, Authentication, and Account Management:**
   - `POST /api/v1/auth/register` (Middleware: validation): Register a new user.
   - `POST /api/v1/auth/login` (Middleware: validation): Authenticate a user and return a session token.
   - `POST /api/v1/auth/logout` (Middleware: authentication): Log out a user and invalidate their session token.
   - `POST /api/v1/auth/password-reset` (Middleware: validation): Initiate a password reset for a user.
   - `PUT /api/v1/auth/password-reset` (Middleware: validation): Complete a password reset for a user.
2. **Profile, Preferences, and Locations:**
   - `GET /api/v1/user/profile` (Middleware: authentication): Retrieve the user's profile, preferences, and saved locations.
   - `PUT /api/v1/user/profile` (Middleware: authentication, validation): Update the user's profile and preferences.
   - `POST /api/v1/user/location` (Middleware: authentication, validation): Add a new saved location for the user.
   - `PUT /api/v1/user/location/{locationId}` (Middleware: authentication, validation): Update a saved location for the user.
   - `DELETE /api/v1/user/location/{locationId}` (Middleware: authentication, validation): Remove a saved location for the user.
3. **Item Search, List Building, and Price Comparison:**
   - `GET /api/v1/items?query={searchQuery}` (Middleware: validation): Retrieve a list of items based on a search query.
   - `POST /api/v1/user/list` (Middleware: authentication, validation): Add an item to the user's shopping list.
   - `GET /api/v1/user/list` (Middleware: authentication, emptyState): Retrieve the user's shopping list.
   - `PUT /api/v1/user/list/{itemId}` (Middleware: authentication, validation): Update an item in the user's shopping list.
   - `DELETE /api/v1/user/list/{itemId}` (Middleware: authentication, validation): Remove an item from the user's shopping list.
   - `GET /api/v1/compare` (Middleware: authentication, rateLimit, validation): Fetch real-time price data and return the most cost-effective store for the user's list.
4. **Feedback and Support:**
   - `POST /api/v1/feedback` (Middleware: authentication, validation): Submit user feedback.
   - `GET /api/v1/help`: Retrieve the help or FAQ content.
5. **Health Check:**
   - `GET /api/v1/health`: Check the API's status.

## Frontend Routes (Next.js)

1. `/`: The homepage. If the user is not logged in, display a welcome message and options to log in or register. If the user is logged in, redirect to their shopping list.
2. `/login`: The login page. If the user is already logged in, redirect to their shopping list.
3. `/register`: The registration page. If the user is already logged in, redirect to their shopping list.
4. `/profile`: The user's profile page. If the user is not logged in, redirect to the login page.
5. `/list`: The user's shopping list. If the user is not logged in, redirect to the login page.
6. `/compare`: The price comparison results. If the user is not logged in, redirect to the login page.
7. `/feedback`: The feedback page. If the user is not logged in, redirect to the login page.

## React Templates

1. `HomePage`: Displayed when the user visits the `/` route. Contains a welcome message and options to log in or register.
2. `LoginPage`: Displayed when the user visits the `/login` route. Contains a form for user login.
3. `RegisterPage`: Displayed when the user visits the `/register` route. Contains a form for user registration.
4. `ProfilePage`: Displayed when the user visits the `/profile` route. Contains the user's profile information, preferences, and saved locations.
5. `ListPage`: Displayed when the user visits the `/list` route. Contains the user's shopping list and a search bar with a dynamic dropdown menu for adding items.
6. `ComparePage`: Displayed when the user visits the `/compare` route. Contains the price comparison results.
7. `FeedbackPage`: Displayed when the user visits the `/feedback` route. Contains a form for submitting user feedback.

## Middleware Helpers

1. **authentication:** Ensures that the user is authenticated before accessing certain routes.
2. **validation:** Validates the input data to ensure it meets the required format and constraints.
3. **rateLimit:** Limits the number of API requests within a certain time frame to prevent abuse.
4. **emptyState:** Handles the scenario when the user's shopping list is empty, providing guidance or a default state.
5. **errorHandler:** Centralizes error handling, providing meaningful error messages and logging errors for debugging purposes.
6. **clientRateLimit:** Limits the number of client-side requests to prevent unnecessary API calls.
