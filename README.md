# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is degined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or `trait` in Rust) in this BambangShop case, or a single Model `struct` is enough?

In this BambangShop case, using an interface or `trait` for the Subscriber in the Observer pattern remains beneficial. Such an approach allows for abstraction and decoupling between the Subject and its observers, promoting flexibility, scalability, and easier testing. By defining a contract for subscribers, interfaces or traits ensure consistency and enforce necessary behaviors, enhancing maintainability and extensibility in the system design. Therefore, even in a simplified structure like BambangShop, utilizing an interface or trait for subscribers aids in achieving loosely coupled and adaptable code architecture.

2. `id` in `Program` and `url` in `Subscriber` is intended to be unique. Explain based on your understanding, is using `Vec` (list) sufficient or using `DashMap` (map/dictionary) like we currently use is necessary for this case?

In a scenario where unique identifiers (`id` in `Program` and `url` in `Subscriber`) are essential, choosing between a `Vec` (list) and a `DashMap` (map/dictionary) depends on the requirements. While a `Vec` could suffice for merely maintaining a collection of elements, ensuring uniqueness would require inefficient iterations over the entire list. In contrast, `DashMap` offers efficient lookup operations based on keys and guarantees uniqueness within the data structure. **Given the necessity for unique identifiers and the potential need for efficient lookup operations, employing a `DashMap` would be more suitable**, providing both integrity and performance to the system.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of List of Subscribers (`SUBSCRIBERS`) static variable, we used the `DashMap` external library for **thread safe HashMap**. Explain based on your understanding of design patterns, do we still need `DashMap` or we can implement Singleton pattern instead?

In the context of managing a list of subscribers in a thread-safe manner in Rust, using `DashMap` from an external library offers a straightforward solution, providing built-in concurrency support for simultaneous access and modification. While the Singleton pattern could be considered for managing a single instance of the subscriber list, implementing it in Rust introduces challenges due to the language's ownership and borrowing rules, necessitating additional synchronization mechanisms like `Mutex` or `RwLock` to ensure thread safety. Ultimately, `DashMap`'s dedicated concurrency handling makes it a more practical choice for managing the subscriber list in a thread-safe manner, offering simplicity and efficiency without the need for manual synchronization efforts.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no "Service" and "Repository". Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to seperate "Service" and "Repository" from a Model?

Separating the "Service" and "Repository" from the Model in MVC ensures a clear division of responsibilities, enhancing code organization and maintainability. The Model, in MVC, primarily focuses on representing the application's domain logic and data structures. Including data storage and business logic within the Model would violate the principle of separation of concerns, making the codebase harder to understand and modify. By introducing separate components for the "Service" and "Repository," we adhere to best practices, enabling better code organization and facilitating code reuse. This separation also promotes modularity and testability, as each component can be independently tested and modified without affecting others.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we only use the Model without separating concerns into "Service" and "Repository" components, things could get messy. Each model (like Program, Subscriber, Notification) would have to handle its own data storage and business logic, which could lead to a tangled mess of code. Plus, as the program gets bigger, keeping track of all these interactions becomes a headache.

3. Have you explored more about **Postman**? Tell us how this tool help you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Postman is like a Swiss Army knife for testing APIs – it's super handy!😁 You can use it to send requests to your API and see how it responds, just like sending messages back and forth. One cool thing is that you can organize your requests into groups, kind of like sorting your emails into folders. And if you need to change something in your tests, you can do it easily using environment variables – it's like having magic placeholders! Plus, Postman can run tests automatically for you, which saves a ton of time and effort. It's like having a robot helper that does all the boring stuff for you!
#### Reflection Publisher-3
