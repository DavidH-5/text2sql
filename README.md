# %%
"""
# 🛠️ Customer-Scoped SQL Agent with DeepAgents

This project implements a **customer-scoped SQL assistant** using:

- **LangChain Deep Agent (`deepagents`)** for agent orchestration  
- **LangChain Community SQL tools (`SQLDatabaseToolkit`)** for database querying  
- **LangGraph middleware (`before_agent`, `wrap_tool_call`)** for context injection and safety guards  
- **OpenAI Chat Models (`gpt-4o-mini`)** for natural language to SQL conversion  

It ensures that **all SQL queries are scoped to a specific customer** to prevent accidental data leaks.

---

## ✅ Capabilities

The agent can:

- Convert natural language queries into SQL for a database (`northwind.db`)  
- Inject the current customer context (`CustomerID`) into agent messages  
- Guard SQL execution to ensure **only queries scoped to the provided CustomerID** are allowed  
- Execute queries safely and return structured responses  

Example:

- User: `"List my recent 5 orders"`  
- Runtime context: `CustomerID = "ALFKI"`  
- Agent executes only SQL queries containing `CustomerID = 'ALFKI'`

---
