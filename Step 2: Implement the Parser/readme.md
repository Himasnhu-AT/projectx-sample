## Step 2: Implement the Parser

## Overview
Build a parser that can take a SQL query as input and convert it into a parse tree. This parse tree will represent the hierarchical structure of the query, making it easier to process and understand.

## Expected Outcome
By the end of this step, you will have a parser that can successfully parse SQL queries and generate a parse tree representing the query's structure.

## Language
This step will be implemented using typescript.

***

```typescript
// parser.ts
import { Token, TokenType } from "./tokenizer";

export interface Node {
  type: string;
  value?: string | number;
  children?: Node[];
}

export class Parser {
  tokens: Token[];
  currentTokenIndex: number;

  constructor(tokens: Token[]) {
    this.tokens = tokens;
    this.currentTokenIndex = 0;
  }

  parse(): Node {
    return this.parseQuery();
  }

  private parseQuery(): Node {
    const query = {
      type: "QUERY",
      children: [],
    } as Node;

    query.children.push(this.parseSelectStatement());

    return query;
  }

  private parseSelectStatement(): Node {
    const select = {
      type: "SELECT",
      children: [],
    } as Node;

    this.consumeToken(TokenType.SELECT);

    select.children.push(this.parseSelectList());

    if (this.currentToken.type === TokenType.FROM) {
      this.consumeToken(TokenType.FROM);
      select.children.push(this.parseFromClause());
    }

    return select;
  }

  private parseSelectList(): Node {
    const selectList = {
      type: "SELECT_LIST",
      children: [],
    } as Node;

    selectList.children.push(this.parseExpression());

    while (this.currentToken.type === TokenType.COMMA) {
      this.consumeToken(TokenType.COMMA);
      selectList.children.push(this.parseExpression());
    }

    return selectList;
  }

  private parseFromClause(): Node {
    const from = {
      type: "FROM",
      children: [],
    } as Node;

    from.children.push(this.parseTableName());

    return from;
  }

  private parseTableName(): Node {
    const tableName = {
      type: "TABLE_NAME",
      value: this.currentToken.value,
    } as Node;

    this.consumeToken(TokenType.IDENTIFIER);

    return tableName;
  }

  private parseExpression(): Node {
    const expression = {
      type: "EXPRESSION",
      children: [],
    } as Node;

    expression.children.push(this.parseColumn());

    return expression;
  }

  private parseColumn(): Node {
    const column = {
      type: "COLUMN",
      value: this.currentToken.value,
    } as Node;

    this.consumeToken(TokenType.IDENTIFIER);

    return column;
  }

  private consumeToken(tokenType: TokenType): Token {
    if (this.currentToken.type !== tokenType) {
      throw new Error(
        `Expected token ${tokenType}, got ${this.currentToken.type}`
      );
    }
    const consumedToken = this.currentToken;
    this.currentTokenIndex++;
    this.currentToken = this.tokens[this.currentTokenIndex];
    return consumedToken;
  }

  get currentToken(): Token {
    return this.tokens[this.currentTokenIndex];
  }
}
```

***

```typescript
// parser.test.ts
import { Parser } from "./parser";
import { Tokenizer } from "./tokenizer";

describe("Parser", () => {
  it("should parse a simple SELECT statement", () => {
    const query = "SELECT name FROM users";
    const tokenizer = new Tokenizer(query);
    const tokens = tokenizer.tokenize();
    const parser = new Parser(tokens);
    const parseTree = parser.parse();

    expect(parseTree).toEqual({
      type: "QUERY",
      children: [
        {
          type: "SELECT",
          children: [
            {
              type: "SELECT_LIST",
              children: [
                {
                  type: "EXPRESSION",
                  children: [
                    {
                      type: "COLUMN",
                      value: "name",
                    },
                  ],
                },
              ],
            },
            {
              type: "FROM",
              children: [
                {
                  type: "TABLE_NAME",
                  value: "users",
                },
              ],
            },
          ],
        },
      ],
    });
  });

  it("should parse a SELECT statement with multiple columns", () => {
    const query = "SELECT name, age FROM users";
    const tokenizer = new Tokenizer(query);
    const tokens = tokenizer.tokenize();
    const parser = new Parser(tokens);
    const parseTree = parser.parse();

    expect(parseTree).toEqual({
      type: "QUERY",
      children: [
        {
          type: "SELECT",
          children: [
            {
              type: "SELECT_LIST",
              children: [
                {
                  type: "EXPRESSION",
                  children: [
                    {
                      type: "COLUMN",
                      value: "name",
                    },
                  ],
                },
                {
                  type: "EXPRESSION",
                  children: [
                    {
                      type: "COLUMN",
                      value: "age",
                    },
                  ],
                },
              ],
            },
            {
              type: "FROM",
              children: [
                {
                  type: "TABLE_NAME",
                  value: "users",
                },
              ],
            },
          ],
        },
      ],
    });
  });
});
```

***

```typescript
// tokenizer.ts
export enum TokenType {
  SELECT = "SELECT",
  FROM = "FROM",
  WHERE = "WHERE",
  AND = "AND",
  OR = "OR",
  NOT = "NOT",
  EQUAL = "EQUAL",
  GREATER_THAN = "GREATER_THAN",
  LESS_THAN = "LESS_THAN",
  GREATER_THAN_OR_EQUAL_TO = "GREATER_THAN_OR_EQUAL_TO",
  LESS_THAN_OR_EQUAL_TO = "LESS_THAN_OR_EQUAL_TO",
  NOT_EQUAL_TO = "NOT_EQUAL_TO",
  PLUS = "PLUS",
  MINUS = "MINUS",
  MULTIPLY = "MULTIPLY",
  DIVIDE = "DIVIDE",
  LEFT_PAREN = "LEFT_PAREN",
  RIGHT_PAREN = "RIGHT_PAREN",
  COMMA = "COMMA",
  IDENTIFIER = "IDENTIFIER",
  NUMBER = "NUMBER",
  STRING = "STRING",
}

export interface Token {
  type: TokenType;
  value: string | number;
}

export class Tokenizer {
  query: string;
  currentCharacterIndex: number;

  constructor(query: string) {
    this.query = query;
    this.currentCharacterIndex = 0;
  }

  tokenize(): Token[] {
    const tokens: Token[] = [];
    while (this.currentCharacterIndex < this.query.length) {
      const currentCharacter = this.query[this.currentCharacterIndex];
      if (currentCharacter.match(/\s/)) {
        this.currentCharacterIndex++;
      } else if (currentCharacter.match(/[a-zA-Z]/)) {
        tokens.push(this.parseIdentifier());
      } else if (currentCharacter.match(/[0-9]/)) {
        tokens.push(this.parseNumber());
      } else {
        tokens.push(this.parseSymbol());
      }
    }
    return tokens;
  }

  private parseIdentifier(): Token {
    let identifier = "";
    while (
      this.currentCharacterIndex < this.query.length &&
      this.query[this.currentCharacterIndex].match(/[a-zA-Z]/)
    ) {
      identifier += this.query[this.currentCharacterIndex];
      this.currentCharacterIndex++;
    }
    return {
      type: TokenType.IDENTIFIER,
      value: identifier,
    };
  }

  private parseNumber(): Token {
    let number = "";
    while (
      this.currentCharacterIndex < this.query.length &&
      this.query[this.currentCharacterIndex].match(/[0-9]/)
    ) {
      number += this.query[this.currentCharacterIndex];
      this.currentCharacterIndex++;
    }
    return {
      type: TokenType.NUMBER,
      value: parseInt(number),
    };
  }

  private parseSymbol(): Token {
    const symbol = this.query[this.currentCharacterIndex];
    this.currentCharacterIndex++;

    switch (symbol) {
      case "SELECT":
        return {
          type: TokenType.SELECT,
          value: "SELECT",
        };
      case "FROM":
        return {
          type: TokenType.FROM,
          value: "FROM",
        };
      case "WHERE":
        return {
          type: TokenType.WHERE,
          value: "WHERE",
        };
      case "AND":
        return {
          type: TokenType.AND,
          value: "AND",
        };
      case "OR":
        return {
          type: TokenType.OR,
          value: "OR",
        };
      case "NOT":
        return {
          type: TokenType.NOT,
          value: "NOT",
        };
      case "=":
        return {
          type: TokenType.EQUAL,
          value: "=",
        };
      case ">":
        return {
          type: TokenType.GREATER_THAN,
          value: ">",
        };
      case "<":
        return {
          type: TokenType.LESS_THAN,
          value: "<",
        };
      case ">=":
        return {
          type: TokenType.GREATER_THAN_OR_EQUAL_TO,
          value: ">=",
        };
      case "<=":
        return {
          type: TokenType.LESS_THAN_OR_EQUAL_TO,
          value: "<=",
        };
      case "!=":
        return {
          type: TokenType.NOT_EQUAL_TO,
          value: "!=",
        };
      case "+":
        return {
          type: TokenType.PLUS,
          value: "+",
        };
      case "-":
        return {
          type: TokenType.MINUS,
          value: "-",
        };
      case "*":
        return {
          type: TokenType.MULTIPLY,
          value: "*",
        };
      case "/":
        return {
          type: TokenType.DIVIDE,
          value: "/",
        };
      case "(":
        return {
          type: TokenType.LEFT_PAREN,
          value: "(",
        };
      case ")":
        return {
          type: TokenType.RIGHT_PAREN,
          value: ")",
        };
      case ",":
        return {
          type: TokenType.COMMA,
          value: ",",
        };
      default:
        throw new Error(`Unknown symbol: ${symbol}`);
    }
  }
}
```