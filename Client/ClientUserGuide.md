# Client User Guide

## Introduction

`Client` is the frontend component of the system for building and querying regression trees. It connects to the server over the network and provides a command-line interface for user interaction.

The client allows users to train a regression tree using data from a database table, load a previously saved tree from the server, and navigate the tree to make predictions on new data.

Once started, the client connects to the specified server and displays a text-based menu for selecting the operation to perform. All processing takes place on the server side: the client collects user input, sends requests to the server, and displays the results.

## System Requirements

- **Java 21 or later** is required to run the client.
- The precompiled JAR includes all required dependencies, so no additional libraries need to be installed.
- A running and reachable instance of `Server` is required.

Before starting the client, ensure that the server is running and listening on the appropriate port.

## Installation and Configuration

The working directory must contain the following files:

- `client.jar` — the executable client application.
- `RunClient.bat` — a startup script for Windows.

The client requires two command-line arguments to establish a connection. These are already configured in the command included in `RunClient.bat`.

| Parameter | Description | Example |
| --- | --- | --- |
| Host | IP address or hostname of the server | `127.0.0.1` or `localhost` |
| Port | Port on which the server is listening | `8080` |

These parameters must match the configuration of the server to which the client is connecting.

## Starting the Client

To launch the client, open a terminal in the directory containing the JAR file and execute:

```bash
java -jar client.jar <host> <port>
```

For example, to connect to a server running locally on the default port:

```bash
java -jar client.jar localhost 8080
```

On Windows, you can also double-click `RunClient.bat`, which executes the command with the default parameters `localhost` and `8080`.

The initial output displays information about the established connection:

```text
Socket[addr=localhost/127.0.0.1,port=8080,localport=xxxxx]
```

When this message appears, the connection is active and the client is ready to receive user commands.

## How It Works

After connecting, the client displays a menu for selecting the operation to perform:

```text
Learn Regression Tree from data [1]
Load Regression Tree from archive [2]
```

Enter `1` or `2` and press Enter. Other inputs, including non-numeric text, are rejected without interrupting execution. The menu is displayed again until a valid option is entered.

### Training from a Database (Option 1)

When option `1` is selected, the client asks for the name of the table to use for training the regression tree:

```text
File name:
```

Enter the table name (for example, `Customers`) and press Enter.

The client sends the request to the server and waits while the operation is processed remotely. If the operation succeeds, the following messages are displayed:

```text
Starting data acquisition phase!
Starting learning phase!
```

If an error occurs, such as a missing table or a database connection problem, the server returns an error message, which is displayed directly by the client.

### Loading a Tree from an Archive (Option 2)

When option `2` is selected, the client asks for the name of the archive file saved on