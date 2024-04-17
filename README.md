# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [V] Commit: `Create Notification model struct.`
    -   [V] Commit: `Create SubscriberRequest model struct.`
    -   [V] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [V] Commit: `Implement add function in Notification repository.`
    -   [V] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [V] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [V] Commit: `Create Notification service struct skeleton.`
    -   [V] Commit: `Implement subscribe function in Notification service.`
    -   [V] Commit: `Implement subscribe function in Notification controller.`
    -   [V] Commit: `Implement unsubscribe function in Notification service.`
    -   [V] Commit: `Implement unsubscribe function in Notification controller.`
    -   [V] Commit: `Implement receive_notification function in Notification service.`
    -   [V] Commit: `Implement receive function in Notification controller.`
    -   [V] Commit: `Implement list_messages function in Notification service.`
    -   [V] Commit: `Implement list function in Notification controller.`
    -   [V] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
*In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?*

The code relies on RwLock<Vec<Notification>> to keep things in order when multiple threads access the notification list. This fancy lock lets multiple threads read the list at once (great for efficiency) but only allows one thread to write (important to avoid data corruption). This is crucial because different threads might add or grab notifications from the same list.  A regular Mutex<Vec<Notification>> would be too strict, only allowing one thread in at a time, even for reads.  With RwLock, reads can happen concurrently while writes remain exclusive, making it the perfect fit for this situation.

The code relies on RwLock<Vec<Notification>> to keep things in order when multiple threads access the notification list. This fancy lock lets multiple threads read the list at once (great for efficiency) but only allows one thread to write (important to avoid data corruption). This is crucial because different threads might add or grab notifications from the same list.  A regular Mutex<Vec<Notification>> would be too strict, only allowing one thread in at a time, even for reads.  With RwLock, reads can happen concurrently while writes remain exclusive, making it the perfect fit for this situation.


*In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?*

Rust enforces memory safety and thread safety through its ownership and immutability rules. Unlike languages with garbage collection, Rust manages memory explicitly, preventing data races. Static variables, by default, are immutable. This means their value can't be changed after initialization. This design choice aligns with Rust's focus on safety. It prevents potential issues caused by concurrent access to static variables, which could corrupt data or lead to unpredictable behavior. By disallowing mutation, Rust forces developers to use safe and explicit mechanisms like locks (Mutexes, RwLocks) or atomic operations to control access to shared data. This approach promotes robust and predictable concurrent programming in Rust, without sacrificing performance or adding unnecessary complexity.

#### Reflection Subscriber-2

*Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.*

I haven't due to the limited time I have with other projects.

*Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?*

The Observer pattern shines in its ability to effortlessly handle growing numbers of subscribers. Subscribers simply register with the publisher to get updates, keeping them loosely coupled. Adding new subscribers doesn't require touching the publisher's code. Instead, new subscriber instances can be created and registered on the fly, smoothly fitting into the existing system. Similarly, scaling the main application by spawning multiple instances is a breeze. Each instance manages its own subscriber set and notifications independently. New subscribers can be added to any instance without disrupting others. This scalability makes the system highly adaptable and easy to maintain as it expands.

*Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).*

While writing my own tests was a good first step, it can be challenging to achieve thorough coverage, especially for edge cases.  These are the unexpected or extreme scenarios that can expose hidden bugs.