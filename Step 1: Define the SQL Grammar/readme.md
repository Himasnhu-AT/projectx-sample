## Step 1: Define the SQL Grammar

## Overview
Create a formal grammar to define the syntax of the SQL dialect supported by the engine. This can be done using a grammar definition language like BNF (Backus-Naur Form) or EBNF (Extended Backus-Naur Form).

## Expected Outcome
By the end of this step, you will have a well-defined grammar that specifies the structure of SQL queries your engine will understand.

## Language
This step will be implemented using typescript.

## Code implementation for Step 1: Define the SQL Grammar in typescript
```typescript
// Define the grammar using EBNF notation
// This is a basic example, you will need to extend it for your specific SQL dialect

export const sqlGrammar = {
  // <query> ::= <select_statement> | <insert_statement> | <update_statement> | <delete_statement> | <create_table_statement> | <drop_table_statement>
  query: "<select_statement> | <insert_statement> | <update_statement> | <delete_statement> | <create_table_statement> | <drop_table_statement>",

  // <select_statement> ::= SELECT <select_list> FROM <table_name> [WHERE <condition>]
  select_statement: "SELECT <select_list> FROM <table_name> [WHERE <condition>]",

  // <select_list> ::= <column_name> | <column_name> , <select_list>
  select_list: "<column_name> | <column_name> , <select_list>",

  // <table_name> ::= <identifier>
  table_name: "<identifier>",

  // <condition> ::= <expression> <comparison_operator> <expression>
  condition: "<expression> <comparison_operator> <expression>",

  // <expression> ::= <column_name> | <literal> | <function> | <expression> <operator> <expression>
  expression: "<column_name> | <literal> | <function> | <expression> <operator> <expression>",

  // <comparison_operator> ::= = | != | > | >= | < | <=
  comparison_operator: "= | != | > | >= | < | <=",

  // <operator> ::= + | - | * | /
  operator: "+ | - | * | /",

  // <column_name> ::= <identifier>
  column_name: "<identifier>",

  // <literal> ::= <string_literal> | <numeric_literal>
  literal: "<string_literal> | <numeric_literal>",

  // <string_literal> ::= '' <string> ''
  string_literal: "'' <string> ''",

  // <numeric_literal> ::= <integer> | <float>
  numeric_literal: "<integer> | <float>",

  // <function> ::= <function_name> ( <expression> )
  function: "<function_name> ( <expression> )",

  // <function_name> ::= <identifier>
  function_name: "<identifier>",

  // <identifier> ::= [A-Za-z0-9_]+
  identifier: "[A-Za-z0-9_]+",

  // <string> ::= [A-Za-z0-9\s]+
  string: "[A-Za-z0-9\s]+",

  // <integer> ::= [0-9]+
  integer: "[0-9]+",

  // <float> ::= [0-9]+\.[0-9]+
  float: "[0-9]+\.[0-9]+"
};
```

## Test cases for Step 1: Define the SQL Grammar in typescript
```typescript
// This is a simple test case to validate the grammar definition
// You will need to create more comprehensive test cases to cover all aspects of the grammar
import { sqlGrammar } from './sql_grammar';

describe('SQL Grammar', () => {
  it('should parse a simple select statement', () => {
    const query = "SELECT name, age FROM users WHERE age > 18";
    // You would use a parser library (e.g., antlr, nearley) to parse the query based on the defined grammar
    // ...
    // The parser should successfully parse the query based on the grammar rules
    // ...
  });
});
``` 
