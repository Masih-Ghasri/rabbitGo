# RabbitMQ "Hello World" Tutorial in Go

This project is a simple implementation of the [first official RabbitMQ tutorial using Go](https://www.rabbitmq.com/tutorials/tutorial-one-go). Its purpose is to demonstrate the basic concepts of publishing and consuming messages using RabbitMQ and its Go client library.

## What is RabbitMQ?

RabbitMQ is a powerful, open-source **Message Broker**. Simply put, it acts like a post office for software applications; it receives messages from sending applications (Producers) and delivers them to receiving applications (Consumers). This approach helps in decoupling different parts of a system, thereby increasing flexibility and scalability.

## About This Project

This project consists of two small Go programs:

1.  **`send.go` (Producer):** This program connects to RabbitMQ, sends a single, simple message ("Hello World!") to a queue named `hello`, and then exits.
2.  **`receive.go` (Consumer):** This program connects to RabbitMQ, listens to the `hello` queue, and prints any message it receives to the console. It continues running until manually stopped (with `CTRL+C`).

## Prerequisites

To run this project, you will need:

* **[Go](https://golang.org/dl/)**: The Go programming language must be installed on your system.
* **[RabbitMQ Server](https://www.rabbitmq.com/download.html)**: A RabbitMQ server instance must be installed and running.
    * Ensure the RabbitMQ server is accessible on the default port (`5672`).
    * (Optional but recommended) Enable the [RabbitMQ Management Plugin](https://www.rabbitmq.com/management.html) (`rabbitmq-plugins enable rabbitmq_management`) to visualize queues and messages via the web UI (`http://localhost:15672`).

## Installation & Setup

1.  **Clone the project:**
    ```bash
    git clone https://github.com/Masih-Ghasri/rabbitGo.git
    cd rabbitGo/HelloWorld
    ```

2.  **Install dependencies:**
    The `go run` command usually handles dependencies automatically. However, you can also fetch them using:
    ```bash
    go mod tidy
    ```
    Or:
    ```bash
    go get [github.com/rabbitmq/amqp091-go](https://github.com/rabbitmq/amqp091-go)
    ```

## How to Run

You will need **two separate terminal windows** because the consumer needs to run continuously to listen for messages.

1.  **Run the Consumer (`receive.go`):**
    * Open a terminal window.
    * Navigate to the `HelloWorld` directory.
    * Run the following command:
        ```bash
        go run receive.go
        ```
    * You should see a message like this, and the program will wait for messages:
        ```
        [*] Waiting for messages. To exit press CTRL+C
        ```

2.  **Run the Producer (`send.go`):**
    * Open a **new** terminal window.
    * Navigate to the `HelloWorld` directory.
    * Run the following command:
        ```bash
        go run send.go
        ```
    * You should see a message like this, and the program will exit:
        ```
        [x] Sent Hello World!
        ```

3.  **Observe the Result:**
    * Go back to the first terminal (running `receive.go`). You should now see that the message has been received and printed:
        ```
        [*] Waiting for messages. To exit press CTRL+C
        Received a message: Hello World!
        ```

You can run `send.go` multiple times, and you will see each new message being printed in the `receive.go` terminal.

## Contributing

If you'd like to contribute or improve this project, please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License. (Replace this with your actual license)