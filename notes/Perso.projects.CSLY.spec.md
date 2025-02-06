---
id: Perso.projects.CSLY.spec
title: Perso.projects.CSLY.spec
desc: AI generated spec
updated: 0
created: 0
---
# **Repository Specification Document**

## **1. Overview**
### **Project Name:**
Sly - A Lightweight Lexer and Parser Framework

### **Description:**
This repository contains a modular and extensible framework for building lexers and parsers. The framework provides fluent APIs for constructing lexical analyzers and parsers, supporting internationalization, error handling, and advanced parsing techniques.

### **Key Features:**
- **Fluent API** for defining lexers and parsers.
- **Multiple Lexical Token Types** (Identifiers, Keywords, Strings, Integers, etc.).
- **Error Handling System** with categorized error levels.
- **State Machine-Based Lexer** with FSM-based token matching.
- **Parser Construction Tools** (Recursive Descent, EBNF-based, etc.).
- **Support for Multiple Languages** (i18n system for translations).

---

## **2. Architecture**
The repository is structured into multiple components, each handling a specific aspect of the lexer and parser framework.

### **2.1 Directory Structure**
```
src/
  sly/
    buildresult/      # Handles build results, errors, and error codes
    i18n/             # Internationalization support (translations, messages)
    lexer/            # Lexical analysis components
      attributes/     # Lexeme attributes (e.g., Keywords, Identifiers, Numbers)
      fluent/         # Fluent API for lexer construction
      fsm/            # Finite State Machine (FSM) based lexer implementation
    parser/           # Parser construction components
      fluent/         # Fluent API for EBNF-based parsers
      generator/      # Visitor-based AST and syntax tree generation
    EnumConverter.cs  # Utility for converting string enums
    sly.csproj        # Project configuration
```

---

## **3. Components**

### **3.1 Lexer**
**Purpose:** Converts raw input text into a structured sequence of tokens.

**Key Components:**
- **Lexeme Attributes** (`LexemeAttribute.cs`, `KeywordAttribute.cs`, `StringAttribute.cs`) – Define token types.
- **FSM-Based Lexer** (`FSMLexer.cs`, `FSMNode.cs`, `FSMTransition.cs`) – Implements finite state machine logic.
- **Fluent API** (`FluentLexerBuilder.cs`, `IFluentLexerBuilder.cs`) – Provides an intuitive way to define lexers.

### **3.2 Parser**
**Purpose:** Converts token sequences into structured syntax trees based on grammar rules.

**Key Components:**
- **Fluent Parser API** (`FluentParserBuilder.cs`, `IFluentParserBuilder.cs`) – Enables declarative parsing.
- **Grammar Rules** (`RuleParser.cs`, `SyntaxTreeVisitor.cs`) – Defines parsing rules and AST traversal.
- **Error Handling** (`ParseError.cs`, `UnexpectedTokenSyntaxError.cs`) – Handles syntax and parsing errors.

### **3.3 Build Results and Error Handling**
**Purpose:** Centralized system for reporting errors, warnings, and parsing failures.

**Key Components:**
- **Error Codes Enum** (`ErrorCodes.cs`) – Categorizes lexer/parser errors.
- **Error Levels** (`ErrorLevel.cs`) – Differentiates between fatal, error, and warning messages.
- **BuildResult** (`BuildResult.cs`) – Collects errors and validation results.

### **3.4 Internationalization (i18n)**
**Purpose:** Supports multilingual error messages and UI texts.

**Key Components:**
- **Translation Files** (`translations/en.txt`, `translations/fr.txt`, `translations/zh-Hans.txt`) – Stores error messages in different languages.
- **i18n Manager** (`I18N.cs`) – Loads and retrieves localized messages.

---

## **4. Error Handling and Logging**
**Error Reporting:**
- Each error is classified using `ErrorCodes.cs` and logged in `BuildResult.cs`.
- Errors are collected and can be retrieved for debugging and reporting.

**Logging:**
- Currently, errors are converted to string format for debugging.
- Possible enhancement: Integrate a dedicated logging system.

---

## **5. Usage Guide**
### **5.1 Creating a Lexer**
```csharp
var lexer = FluentLexerBuilder<TokenType>
    .NewBuilder()
    .IgnoreWhiteSpace(true)
    .Keyword(TokenType.IF, "if")
    .Keyword(TokenType.ELSE, "else")
    .String(TokenType.STRING)
    .Integer(TokenType.INT)
    .Build("en");
```

### **5.2 Creating a Parser**
```csharp
var parser = FluentParserBuilder<TokenType>
    .NewBuilder()
    .Rule("expression")
    .Match(TokenType.INT)
    .Or()
    .Match(TokenType.STRING)
    .Build();
```

---

## **6. Future Enhancements**
- **Improve Logging Mechanism**: Integrate structured logging for debugging.
- **Refactor Large Methods**: Split long functions into smaller, reusable parts.
- **Optimize Lexer FSM**: Improve state transition efficiency.
- **Add More Examples**: Provide sample projects demonstrating different use cases.

---

## **7. Conclusion**
This repository provides a robust and flexible lexer/parser framework with internationalization, error handling, and fluent APIs. Future improvements will focus on logging, performance optimization, and user documentation.


