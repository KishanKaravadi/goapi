# Go API Learning Project

This repository contains a simple RESTful API built with Go, primarily as a hands-on project to learn and understand core Go concepts, project structuring, and basic API development patterns. It utilizes an in-memory "mock" database for data storage, making it lightweight and easy to run without external dependencies.

## 🚀 Getting Started

Follow these steps to get a copy of the project up and running on your local machine.

### Prerequisites

Before you begin, ensure you have the following installed:

* **Go (1.24.4 recommended):** <https://go.dev/dl/>

* **Git:** <https://git-scm.com/downloads>

### Installation

1.  **Clone the repository:**

    ```bash
    git clone [https://github.com/KishanKaravadi/goapi.git](https://github.com/KishanKaravadi/goapi.git) # This is your module path
    cd goapi
    ```

2.  **Initialize Go modules and download dependencies:**

    ```bash
    go mod tidy
    ```

### Running the Application

To start the API server:

```bash
go run ./cmd/api
```
The API should now be running, typically on `http://localhost:8000` (as configured in `main.go`).

## 📂 Project Structure

This project follows a common Go project layout to help organize the code:

* **`api/`**: Contains core API type definitions (`CoinBalanceParams`, `CoinBalanceResponse`, `Error` structs) and common utility functions (`writeError`, `RequestErrorHandler`, `InternalErrorHandler`) for consistent API responses.

* **`cmd/api/`**: The main application entry point (`main.go`) which initializes the HTTP server (`chi.NewRouter()`) and registers the API handlers.

* **`internal/`**: This directory holds private application and library code. Packages within `internal` cannot be imported by other modules, enforcing encapsulation.

    * **`internal/handlers/`**: Contains HTTP handler functions that process incoming requests and send responses, such as `GetCoinBalance`. It also defines the main `Handler` function that sets up routes.

    * **`internal/middleware/`**: Houses middleware functions (e.g., `authorization.go`) that can be applied to requests before they reach the handlers (for tasks like authentication, logging, etc.).

    * **`internal/tools/`**: A utility package containing helper functions or components. This includes interface definitions (`DatabaseInterface`), concrete implementations (`mockdb.go`), and a constructor for the database (`NewDatabase`).

## 💡 API Endpoints

This is a basic API, but it demonstrates how to handle common requests.

### Get Account Coins Balance

* **Endpoint:** `GET /account/coins`

* **Description:** Retrieves a user's coin balance from the mock database.

* **Authentication:** Requires an `Authorization` header with a valid token, alongside the `username` query parameter, as handled by the `Authorization` middleware.

* **Query Parameters:**

    * `username`: (string, **required**) The username of the user whose coin balance is to be retrieved.

* **Example Request (using Postman or cURL):**

    Assuming `marie` has `789GHI` as `AuthToken` from `mockLoginDetails`:

    ```
    GET http://localhost:8000/account/coins?username=marie
    Headers:
        Authorization: 789GHI
    ```

* **Example Response (Success):**

    ```json
    {
        "Code": 200,
        "Balance": 300
    }
    ```

* **Example Response (Error - Bad Request, e.g., missing username or token, or invalid credentials):**

    ```json
    {
        "Code": 400,
        "Message": "Invalid username or token."
    }
    ```

* **Example Response (Error - Internal Server Error, e.g., issues with database setup or unexpected errors):**

    ```
    {
        "Code": 500,
        "Message": "An Unexpected Error Occured."
    }
    ```

---

## 🛠️ Technologies Used

* **Go Programming Language (1.24.4)**

* **Go Standard Library:** For HTTP server (`net/http`), JSON encoding/decoding (`encoding/json`), etc.

* **`github.com/go-chi/chi` (v1.5.5):** A lightweight, idiomatic, and composable router for building HTTP services.

* **`github.com/gorilla/schema` (v1.4.1):** For decoding URL query parameters into Go structs (`api.CoinBalanceParams`).

* **`github.com/sirupsen/logrus` (v1.9.3)::** A popular structured logger for Go.

* **In-memory Mock Database:** Simple Go maps and structs (`internal/tools/mockdb.go`) to simulate database storage and interactions.

## 📚 Learning Notes & Future Improvements

This project serves as a foundational learning tool. Here are some ideas for further exploration and improvement:

* **Implement more API endpoints:** Add functionalities like `POST /transactions`, `PUT /update-profile`, `DELETE /user`.

* **Add proper authentication and authorization:** Beyond basic middleware, integrate JWTs or OAuth.

* **Replace mock database with a real database:** Connect to PostgreSQL, MySQL, MongoDB, or a key-value store like Redis.

* **Implement unit and integration tests:** Learn how to write tests for your handlers, middleware, and business logic.

* **Error Handling Refinements:** Create custom error types and a more robust error response structure.

* **Logging:** Integrate structured logging.

* **Configuration Management:** Use environment variables or a configuration file for settings like port numbers.

* **Dependency Injection:** Explore patterns for managing dependencies more cleanly.

* **Containerization:** Learn how to containerize the application using Docker.

* **Deployment:** Deploy the application to a cloud provider (e.g., Google Cloud Run, AWS EC2, Heroku).

Feel free to fork this repository and experiment with these ideas!
