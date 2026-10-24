# User Guide

## Introduction

MapServer is the backend component of the system developed for building and querying regression trees. The server loads datasets from a MySQL database, builds prediction models, and allows users to navigate the generated trees to obtain predictions for new data.

Once started, the server remains active and waits for incoming connections. Each connected client is handled independently, allowing multiple users to work simultaneously without interfering with one another.

## System Requirements

Java 21 or later is required to run the server. The JAR file is already compiled and includes all the required dependencies, so no additional external libraries need to be installed separately.

The server must be able to connect to an existing MySQL database that has been configured and populated with at least one data table. The last column of each table must be a numeric attribute representing the value that the model is expected to learn to predict.

## Installation and Configuration

The working directory must contain the following two files:

- `Server.jar` — the server executable.
- `RunServer.bat` — a startup script for Windows.

Before starting the server, it is advisable to verify that the MySQL database is reachable. The default connection parameters are:

| Parameter | Value |
|---|---|
| Server | `localhost` |
| Port | `3306` |
| Database | `MapDB` |
| User | `MapUser` |
| Password | `map` |

These parameters are fixed and cannot be changed through command-line arguments or an external configuration file. If the database is hosted at a different address or uses different credentials, the server will be unable to connect and will return an error when the first data operation is attempted.

## Startup

To start the server, navigate to the directory containing the JAR file and run the following command in a terminal:

```bash
java -jar MapServer.jar
```

On Windows, you can also double-click `EseguiServer.bat`, which executes the same command.

The following output is displayed at startup:

```text
[SERVER] Inizializzazione...
[SERVER] Tentativo di avvio sulla porta 8080
Server in ascolto sulla porta 8080
```

When the message `Server in ascolto sulla porta 8080` appears, the service is ready to accept connections. Port `8080` is the default port and, like the database parameters, cannot be changed.

## Operation

The server continues running until it is manually stopped. While running, it prints messages about incoming connections to the console, making it easier to monitor server activity.

When a client connects, a message similar to the following appears:

```text
Nuova connessione da: /192.168.1.50
```

The IP address identifies the connected client. From that point onward, the server can receive requests from that client to load tables, build regression trees, or make predictions.

If a client's connection is interrupted because the client application is closed or a network problem occurs, the server prints:

```text
Client disconnected: /192.168.1.50
```

This message simply indicates that the client's session has ended. The server continues operating normally and remains ready to accept new connections, including reconnections from the same client.

In less frequent cases, error messages may appear in the console. If a problem occurs while handling a new connection, the following message is displayed:

```text
Errore avvio client
```

In this case, the connection is immediately closed, and the client must try to connect again. For more serious network-related errors, the message is instead:

```text
Errore di connessione sul server: <dettaglio_errore>
```

## Communication Protocol with the Client

Each request sent by a client to the server is identified by a command code. Depending on the code, the server expects specific arguments and returns a corresponding response.

| Code | Requested Operation | Arguments Sent by the Client | Server Response |
|---|---|---|---|
| `0` | Load data from the database |