# Regression Tree Client/Server

The system implements a regression tree using a client/server architecture. The server handles data acquisition from a MySQL database, regression tree construction, and the loading of previously saved models. The client allows users to perform interactive predictions by navigating the resulting tree.

The project is organized into two independent modules, `Server` and `Client`, which communicate through TCP sockets. Detailed documentation, including user guides, Javadoc, UML diagrams, and test documentation, is available within the respective modules.

## General Architecture

The system follows a client/server architecture based on TCP sockets. The client and server exchange requests and responses in the form of serialized Java objects.

The server manages the logic related to regression trees and is the only component that accesses the MySQL database to retrieve training data. Alternatively, it can load a previously constructed tree saved in an archive file, avoiding database access and the need to repeat the training phase.

The client sends requests to the server and uses the received responses to allow users to interact with the model and perform predictions.

## File and Directory Structure

The project is divided into the `Server` and `Client` modules, each dedicated to a specific responsibility within the architecture.

The modules contain source code, tests, documentation, and the artifacts required for execution. The structure includes the following resources, where applicable:

- `src/`: source code;
- `test/`: module tests;
- `doc/`: technical documentation;
- executable JAR files;
- startup scripts;
- UML diagrams;
- user guides and test documentation.

## Client/Server Communication

The client and server communicate by exchanging serialized Java objects. Each request is identified by an integer code that specifies the operation to be performed.

| Code | Operation |
| --- | --- |
| 0 | Retrieve data from a MySQL table |
| 1 | Build the regression tree |
| 2 | Load a tree from a `.dmp` archive file |
| 3 | Perform interactive prediction by navigating the tree |

Requests are processed by the server, which returns the results of the requested operation to the client.

## The Regression Tree

A regression tree is a model constructed from a set of training examples and used to predict the value of a numerical variable, called the target variable, based on the observed values of the other attributes.

Each internal node of the tree represents a test on an independent attribute. The type of test depends on the nature of the attribute:

- **Discrete attribute:** the test provides one branch for each possible value of the attribute.
- **Continuous attribute:** the test is binary and compares the attribute value against a threshold, distinguishing between cases where \(A_i \leq threshold\) and cases where \(A_i > threshold\).

Each leaf node represents the predicted value, calculated as the mean of the target variable values for the training examples that reach that leaf. The path from the root to a leaf corresponds to a conjunction of conditions on the attributes.

### Tree Construction

The tree is built recursively. At each step, the algorithm selects, from the candidate tests on the available attributes, the one that best separates the examples with respect to the target variable.

The selection criterion is based on variance reduction, measured through the sum of squared errors (*Sum of Squared Errors*, SSE). For a set of examples \(S\), the SSE is calculated with respect to the mean of the target values. For each possible split, the SSE values of the resulting subsets are calculated, and the test that minimizes their sum is selected.

The recursion terminates when the subset of examples to be split satisfies the stopping criterion defined by the algorithm, for example, when it contains a sufficiently small number of elements.

### Model Saving and Loading

Trained trees can be saved to archive files with the `.dmp` extension and subsequently reloaded. This functionality makes it possible to reuse an already constructed model without repeating the training process or accessing the database again.

## Database Configuration

The server accesses the MySQL database through JDBC. The default configuration is shown in the following table.

| Parameter | Value |
| --- | --- |
| JDBC Driver | `com.mysql.cj.jdbc.Driver` |
| Host | `localhost` |
| Port | `3306` |
| Database | `MapDB` |
| Username | `MapUser` |
| Password | `map` |

The JDBC library used is `mysql-connector-java-8.0.17.jar`, located in the `Server/lib/` directory.

The connection parameters can be modified according to the configuration of the environment in which the server runs.