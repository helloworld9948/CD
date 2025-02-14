Symbol Table Implementation in C

```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TABLE_SIZE 10

typedef struct Symbol {
    char name[50];
    char type[50];
    int scope;
} Symbol;

Symbol symbolTable[TABLE_SIZE];

unsigned int hash(char *name) {
    unsigned int hashValue = 0;
    while (*name) {
        hashValue = (hashValue << 5) + *name++;
    }
    return hashValue % TABLE_SIZE;
}

void insertSymbol(char *name, char *type, int scope) {
    unsigned int index = hash(name);
    strcpy(symbolTable[index].name, name);
    strcpy(symbolTable[index].type, type);
    symbolTable[index].scope = scope;
}

Symbol* lookupSymbol(char *name) {
    unsigned int index = hash(name);
    if (strcmp(symbolTable[index].name, name) == 0) {
        return &symbolTable[index];
    }
    return NULL;
}

void printSymbolTable() {
    printf("\nSymbol Table:\n");
    for (int i = 0; i < TABLE_SIZE; i++) {
        if (symbolTable[i].name[0] != '\0') {
            printf("Name: %-10s Type: %-10s Scope: %-5d\n", symbolTable[i].name, symbolTable[i].type, symbolTable[i].scope);
        }
    }
}

int main() {
    for (int i = 0; i < TABLE_SIZE; i++) {
        symbolTable[i].name[0] = '\0';
    }

    insertSymbol("x", "int", 0);
    insertSymbol("y", "float", 1);
    insertSymbol("add", "function", 0);

    Symbol* found = lookupSymbol("x");
    if (found) {
        printf("\nFound symbol: %s, Type: %s, Scope: %d\n", found->name, found->type, found->scope);
    } else {
        printf("\nSymbol not found\n");
    }

    printSymbolTable();
    return 0;
}


```
Lexical Analyzer in Lex

```c

%{
#include <stdio.h>
%}

DIGIT [0-9]+
IDENT [a-zA-Z_][a-zA-Z0-9_]*
COMMENT "//".*
OPERATOR [+\-*/=<>!]+

%%
{DIGIT} { printf("Constant: %s\n", yytext); }
{IDENT} { printf("Identifier: %s\n", yytext); }
{COMMENT} { printf("Comment: %s\n", yytext); }
{OPERATOR} { printf("Operator: %s\n", yytext); }
[\t\n ]+  { }
. { printf("Unrecognized: %s\n", yytext); }
%%

int main() {
    yylex();
    return 0;
}

int yywrap() {
    return 1;
}


```
How to Compile and Run

```Bash

lex lexer.l
gcc lex.yy.c -o lexer -ll
./lexer

```
2. Lex Program (lexer.l)

```c


%{

include <stdio.h>
int lineCount = 1;
%}

DIGIT [0-9]
LETTER [a-zA-Z]
IDENTIFIER ({LETTER})({LETTER}|{DIGIT})
CONSTANT ({DIGIT})+
OPERATOR [-+/=]
COMMENT \/\/[^\n]*
WHITESPACE [ \t]+

%%
\n { lineCount++; }
{IDENTIFIER} { printf("Identifier: %s\n", yytext); }
{CONSTANT} { printf("Constant: %s\n", yytext); }
{OPERATOR} { printf("Operator: %s\n", yytext); }
{COMMENT} { printf("Comment: %s\n", yytext); }
{WHITESPACE} { / Ignore whitespace / }
. { printf("Invalid: %s\n", yytext); }
%%

int main() {
yylex();
return 0;
}

int yywrap() {
return 1;
}


```

---

