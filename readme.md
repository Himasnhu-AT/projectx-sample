
# Mini SQL Engine

This project aims to create a mini SQL engine implemented in TypeScript. 

## Project Overview

The Mini SQL Engine will consist of three core modules:

1. **Parser:** Takes a SQL query as input and generates a parse tree representing the query's structure.
2. **Query Executor:** Processes the parse tree and executes the corresponding SQL query.
3. **Query Generator:** Generates valid SQL queries from data structures or user input.

## Project Steps

**Step 1: Define the SQL Grammar**

* Create a formal grammar (BNF/EBNF) to define the syntax of the supported SQL dialect.

**Step 2: Implement the Parser**

* Develop a parser using a parsing technique like recursive descent or LL(k) parsing.
* The parser should convert SQL queries into parse trees.

**Step 3: Implement the Query Executor**

* Design a query executor to process parse trees and execute queries.
* This could involve simulating a database or interacting with a real data store.

**Step 4: Implement Query Generator**

* Develop a module to generate SQL queries based on data structures or user input.

**Step 5: Integration and Testing**

* Integrate the modules into a functioning Mini SQL Engine.
* Perform thorough testing with a variety of SQL queries.

## Project Structure

* **parser:** Contains the parser module.
* **executor:** Contains the query executor module.
* **generator:** Contains the query generator module.
* **utils:** Contains utility functions used across the project.
* **test:** Contains test cases for each module.

## Project Goals

* Gain experience building a simple SQL engine.
* Understand the fundamentals of SQL parsing and execution.
* Develop skills in TypeScript programming.

## Dependencies

* TypeScript
* (Optional) A data store or mock database for testing.

## Getting Started

1. Clone this repository.
2. Install dependencies using npm or yarn.
3. Start implementing the project steps.

## Contributions

Contributions are welcome! Please open issues or submit pull requests.
