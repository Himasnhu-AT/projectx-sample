## Step 4: Implement Query Generator

## Overview
This module will be responsible for generating SQL queries from a given data structure or object. This is useful for creating queries dynamically or for generating queries based on user input.

## Expected Outcome
By the end of this step, you will have a query generator that can produce valid SQL queries based on input data or specifications.

## Language
This step will be implemented using typescript.

---
# Code implementation for Step 4: Implement Query Generator in typescript

```typescript
// queryGenerator.ts
export class QueryGenerator {
  private tableName: string;
  private columns: string[];

  constructor(tableName: string, columns: string[]) {
    this.tableName = tableName;
    this.columns = columns;
  }

  generateSelectQuery(whereClause: string = ''): string {
    const selectColumns = this.columns.join(', ');
    const query = `SELECT ${selectColumns} FROM ${this.tableName}`;
    if (whereClause) {
      return `${query} WHERE ${whereClause}`;
    }
    return query;
  }

  generateInsertQuery(data: Record<string, any>): string {
    const columns = this.columns.join(', ');
    const values = this.columns.map(col => `'${data[col]}'`).join(', ');
    return `INSERT INTO ${this.tableName} (${columns}) VALUES (${values})`;
  }

  generateUpdateQuery(data: Record<string, any>, whereClause: string): string {
    const setStatements = this.columns.map(col => `${col} = '${data[col]}'`).join(', ');
    return `UPDATE ${this.tableName} SET ${setStatements} WHERE ${whereClause}`;
  }

  generateDeleteQuery(whereClause: string): string {
    return `DELETE FROM ${this.tableName} WHERE ${whereClause}`;
  }
}
```

---
# Test cases for Step 4: Implement Query Generator in typescript

```typescript
// queryGenerator.test.ts
import { QueryGenerator } from './queryGenerator';

describe('QueryGenerator', () => {
  const tableName = 'users';
  const columns = ['id', 'name', 'email'];

  const queryGenerator = new QueryGenerator(tableName, columns);

  it('should generate a select query', () => {
    const query = queryGenerator.generateSelectQuery();
    expect(query).toBe(`SELECT id, name, email FROM users`);
  });

  it('should generate a select query with a where clause', () => {
    const query = queryGenerator.generateSelectQuery('id = 1');
    expect(query).toBe(`SELECT id, name, email FROM users WHERE id = 1`);
  });

  it('should generate an insert query', () => {
    const data = { id: 1, name: 'John Doe', email: 'john.doe@example.com' };
    const query = queryGenerator.generateInsertQuery(data);
    expect(query).toBe(`INSERT INTO users (id, name, email) VALUES ('1', 'John Doe', 'john.doe@example.com')`);
  });

  it('should generate an update query', () => {
    const data = { name: 'Jane Doe', email: 'jane.doe@example.com' };
    const query = queryGenerator.generateUpdateQuery(data, 'id = 1');
    expect(query).toBe(`UPDATE users SET name = 'Jane Doe', email = 'jane.doe@example.com' WHERE id = 1`);
  });

  it('should generate a delete query', () => {
    const query = queryGenerator.generateDeleteQuery('id = 1');
    expect(query).toBe(`DELETE FROM users WHERE id = 1`);
  });
});
```