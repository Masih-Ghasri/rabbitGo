# RabbitMQ "Work Queues" Tutorial in Go

This project is an implementation of the [second official RabbitMQ tutorial using Go](https://www.rabbitmq.com/tutorials/tutorial-two-go), focusing on **Work Queues**. Its purpose is to demonstrate how to distribute time-consuming tasks among multiple workers, using concepts like **Message Acknowledgment** and **Durability**.

## What is RabbitMQ?

RabbitMQ is a powerful, open-source **Message Broker**. Simply put, it acts like a post office for software applications; it receives messages from sending applications (Producers) and delivers them to receiving applications (Consumers). This approach helps in decoupling different parts of a system, thereby increasing flexibility and scalability.

## About This Project

This project demonstrates the "Work Queue" pattern (also known as a "Task Queue"). It involves two programs:

1.  **`new_task.go` (Producer/Task Sender):** This program sends tasks (messages) to a shared queue named `task_queue`. The tasks are read from command-line arguments and are marked as **persistent**, meaning they will survive a RabbitMQ restart.
2.  **`worker.go` (Consumer/Worker):** This program (you can run multiple instances) connects to the `task_queue`, receives tasks one by one, simulates work based on the message content (each `.` represents one second of work), and sends back a **manual acknowledgment** only after the task is completed. This ensures that if a worker dies, the task isn't lost.

Key concepts covered:
* **Work Queues:** Distributing tasks among multiple consumers.
* **Durability:** Ensuring queues and messages survive broker restarts.
* **Message Acknowledgment:** Making sure a message is only removed from the queue after being successfully processed.
* **Fair Dispatch (QoS):** Ensuring workers only receive one message at a time.

## Prerequisites

To run this project, you will need:

* **[Go](https://golang.org/dl/)**: The Go programming language must be installed.
* **[RabbitMQ Server](https://www.rabbitmq.com/download.html)**: A RabbitMQ server instance must be installed and running.
    * Ensure the server is accessible on the default port (`5672`).
    * (Optional but recommended) Enable the [RabbitMQ Management Plugin](https://www.rabbitmq.com/management.html) (`rabbitmq-plugins enable rabbitmq_management`) to visualize queues (`http://localhost:15672`).

## Installation & Setup

1.  **Clone the project (if you haven't already):**
    ```bash
    git clone [https://github.com/Masih-Ghasri/rabbitGo.git](https://github.com/Masih-Ghasri/rabbitGo.git)
    cd rabbitGo
    ```

2.  **Install dependencies:**
    ```bash
    go mod tidy
    ```

## How to Run

You will need **at least two terminal windows**. It's recommended to open three or more: two for running `worker.go` instances and one for `new_task.go`.

1.  **Run the Workers (`worker.go`):**
    * Open one (or more) terminal windows.
    * Navigate to the `WorkQueues` directory:
        ```bash
        cd WorkQueues
        ```
    * Run the following command:
        ```bash
        go run worker.go
        ```
    * You should see:
        ```
        [*] Waiting for messages. To exit press CTRL+C
        ```
    * Repeat this in a second terminal to start another worker.

2.  **Run the Task Sender (`new_task.go`):**
    * Open a **new** terminal window.
    * Navigate to the `WorkQueues` directory:
        ```bash
        cd WorkQueues
        ```
    * Run the command, adding your task message as an argument. Each `.` adds a second of "work":
        ```bash
        go run new_task.go "First task."
        go run new_task.go "Second task.."
        go run new_task.go "A much longer task....."
        ```
    * For each command, you will see output like:
        ```
        [x] Sent First task.
        ```

3.  **Observe the Result:**
    * Go back to the worker terminals. You will see how the tasks are distributed between them and processed:
        ```
        [*] Waiting for messages. To exit press CTRL+C
        Received a message: First task.
        Done
        Received a message: A much longer task.....
        Done
        ```
      *(The other worker will process "Second task..")*

Notice how RabbitMQ distributes the tasks among the available workers. If you stop a worker mid-task (before "Done" is printed), RabbitMQ will re-queue that task and give it to another available worker once it starts up again.

## Contributing

If you'd like to contribute or improve this project, please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License.