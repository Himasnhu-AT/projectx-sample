## Step 3: Implement the Query Executor

## Overview

Develop the core functionality of your engine by implementing a query executor. This component will take the parse tree as input and execute the corresponding query. This might involve interacting with a data store or a simulated database.

## Expected Outcome

By the end of this step, you will have a query executor capable of processing parse trees and producing results based on the executed queries.

## Language

This step will be implemented using typescript.

```markdown
# Code implementation for Step 3: Implement the Query Executor in typescript
```

```typescript
// query_executor.ts
import { ParseTree } from './parser';
import { Table } from './database';

export class QueryExecutor {
  private tables: { [key: string]: Table };

  constructor(tables: { [key: string]: Table }) {
    this.tables = tables;
  }

  execute(tree: ParseTree): any {
    switch (tree.type) {
      case 'SELECT':
        return this.executeSelect(tree);
      case 'INSERT':
        return this.executeInsert(tree);
      case 'UPDATE':
        return this.executeUpdate(tree);
      case 'DELETE':
        return this.executeDelete(tree);
      default:
        throw new Error(`Unsupported query type: ${tree.type}`);
    }
  }

  private executeSelect(tree: ParseTree): any[] {
    const tableName = tree.tableName;
    const table = this.tables[tableName];

    if (!table) {
      throw new Error(`Table ${tableName} not found`);
    }

    // TODO: Implement filtering and projection logic based on the SELECT tree structure
    return table.data;
  }

  private executeInsert(tree: ParseTree): void {
    const tableName = tree.tableName;
    const table = this.tables[tableName];

    if (!table) {
      throw new Error(`Table ${tableName} not found`);
    }

    // TODO: Implement insert logic based on the INSERT tree structure
    // Add the new row to the table data
  }

  private executeUpdate(tree: ParseTree): void {
    const tableName = tree.tableName;
    const table = this.tables[tableName];

    if (!table) {
      throw new Error(`Table ${tableName} not found`);
    }

    // TODO: Implement update logic based on the UPDATE tree structure
    // Modify the existing rows based on the provided conditions and values
  }

  private executeDelete(tree: ParseTree): void {
    const tableName = tree.tableName;
    const table = this.tables[tableName];

    if (!table) {
      throw new Error(`Table ${tableName} not found`);
    }

    // TODO: Implement delete logic based on the DELETE tree structure
    // Remove the matching rows based on the conditions provided
  }
}
```

```markdown
# Test cases for Step 3: Implement the Query Executor in typescript
```

```typescript
// query_executor_test.ts
import { QueryExecutor } from './query_executor';
import { ParseTree } from './parser';
import { Table } from './database';

describe('QueryExecutor', () => {
  let executor: QueryExecutor;
  let table: Table;

  beforeEach(() => {
    table = new Table('users', [
      { name: 'id', type: 'INT' },
      { name: 'name', type: 'VARCHAR' },
      { name: 'age', type: 'INT' },
    ]);

    table.data = [
      { id: 1, name: 'John Doe', age: 30 },
      { id: 2, name: 'Jane Doe', age: 25 },
      { id: 3, name: 'Peter Pan', age: 20 },
    ];

    executor = new QueryExecutor({ users: table });
  });

  it('should execute SELECT query', () => {
    const selectTree: ParseTree = {
      type: 'SELECT',
      tableName: 'users',
    };

    const result = executor.execute(selectTree);
    expect(result).toEqual(table.data);
  });

  it('should throw error for non-existent table', () => {
    const selectTree: ParseTree = {
      type: 'SELECT',
      tableName: 'non_existent_table',
    };

    expect(() => executor.execute(selectTree)).toThrowError(
      'Table non_existent_table not found'
    );
  });

  // TODO: Add more test cases for INSERT, UPDATE, DELETE queries
});
```

This code provides a basic framework for the query executor. It defines functions for handling SELECT, INSERT, UPDATE, and DELETE queries.  Currently, the implementation is incomplete. You need to fill in the logic for each type of query, including filtering, projection, and data manipulation based on the provided parse tree.

The provided test cases illustrate how to test the executor using the `expect` assertions. You should add more test cases to cover various scenarios for each query type. 
