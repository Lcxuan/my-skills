---
name: CodeGraph Repository Analysis
description: Use CodeGraph MCP to analyze repository structure, symbol relationships, call chains, dependencies, implementations, and architecture before reading source files.
---

# CodeGraph Repository Analysis

## Purpose

This project has the CodeGraph MCP server configured.

Your primary goal is to use **CodeGraph** as the first choice whenever the task requires understanding the structure of the codebase.

CodeGraph provides a pre-built AST-based knowledge graph of the repository. It is significantly faster and more accurate than searching files with grep or manually reading large portions of the project.

---

# When to use CodeGraph

Always invoke the **CodeGraph MCP tool** first for requests such as:

- How does feature X work?
- Where is this class or method defined?
- What calls this method?
- Which methods are called by this function?
- Which classes implement this interface?
- Which classes inherit from this base class?
- What is the request flow?
- Trace execution from Controller to Repository.
- Find dependency relationships.
- Analyze project architecture.
- Impact analysis before refactoring.
- Cross-file analysis.
- Repository-wide exploration.
- Understanding unfamiliar modules.

Whenever a question requires information spanning multiple files, CodeGraph should be the first tool used.

---

# Preferred workflow

Follow this workflow whenever possible.

## Step 1

Use CodeGraph.

Do not start with:

- Search Files
- Grep
- Read File

if the task is structural.

---

## Step 2

Analyze the CodeGraph response.

Use the returned:

- symbols
- call paths
- dependency graph
- implementations
- references
- line-numbered source

to answer the user's question.

Trust the CodeGraph results.

Do not repeat the same work using grep.

---

## Step 3

Only use Read File when:

- implementation details are required
- CodeGraph explicitly reports stale files
- configuration files are involved
- documentation is involved
- generated files are involved
- files are not indexed

Read only the minimum number of files necessary.

---

# Tool Priority

Always follow this priority:

1. CodeGraph MCP
2. Search Files
3. Read File

Avoid using Search Files or Grep as the initial exploration tool.

---

# Best Practices

Prefer one comprehensive CodeGraph query instead of many small searches.

Use CodeGraph to:

- locate symbols
- discover relationships
- identify dependencies
- understand architecture
- inspect call chains
- find implementations
- estimate change impact

Only inspect source files after CodeGraph identifies the relevant locations.

---

# If CodeGraph reports stale indexes

If CodeGraph indicates that some files have changed since the last index update:

1. Inform the user.
2. Read only the affected files if necessary.
3. Suggest running:

```
codegraph sync
```

if a complete and up-to-date analysis is required.

---

# If CodeGraph is not initialized

If the MCP tool reports that CodeGraph has not been initialized, ask the user:

> This repository has not been indexed by CodeGraph.
> Would you like me to run:

```
codegraph init -i
```

before continuing?

---

# Examples

## Good

User:

> What calls InventoryService.Save()?

Action:

- Use CodeGraph.
- Return callers.
- Explain the call chain.

---

User:

> Explain the request flow for inventory approval.

Action:

- Use CodeGraph.
- Trace Controller → Service → Repository.
- Explain the execution flow.

---

User:

> Which classes implement IInventoryService?

Action:

- Use CodeGraph.
- Return implementations.

---

User:

> What modules depend on RecoverySummaryService?

Action:

- Use CodeGraph.
- Return dependency graph.
- Explain impact.

---

## Do NOT

Do not start by recursively searching the repository with Search Files or Grep when CodeGraph can answer the question.

Do not manually inspect dozens of files before querying CodeGraph.

Do not verify CodeGraph results using grep unless necessary.

---

# Goal

Use CodeGraph as the primary repository intelligence engine.

Treat manual file searching as a fallback rather than the default approach.