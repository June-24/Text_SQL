# Text-to-SQL Agent with LangGraph & Llama 3

This project demonstrates a robust **Text-to-SQL Agent** capable of converting natural language questions into SQL queries, executing them against a SQLite database, and providing human-readable answers. It leverages **LangGraph** for state management, **LangChain** for tool abstractions, and **Groq** (Llama 3.3) for high-speed inference.

##  Overview

The agent is designed to interact with a SQL database (in this case, a sample employee database) in a conversational manner. It doesn't just write SQL; it understands the database schema, handles errors gracefully by retrying with corrected queries, and synthesizes the final results into natural language.

##  Key Features

- **Schema Awareness**: The agent can inspect the database schema (tables and columns) to write accurate queries.
- **Self-Correction**: If a generated SQL query fails (e.g., syntax error), the agent catches the error and retries with a corrected query.
- **State Management**: Built on top of LangGraph to manage the conversation state and workflow transitions efficiently.
- **High-Performance LLM**: Uses Groq's Llama 3.3-70b-versatile model for fast and accurate SQL generation.

## Tech Stack

- **[LangChain](https://python.langchain.com/)**: Framework for LLM applications.
- **[LangGraph](https://langchain-ai.github.io/langgraph/)**: Library for building stateful, multi-actor applications with LLMs.
- **[Groq](https://groq.com/)**: High-performance AI inference engine.
- **SQLite**: Lightweight disk-based database for the sample data.

## Setup & Installation

1.  **Clone the repository** (if applicable) or ensure you have the `agent_sql.ipynb` file.

2.  **Install Dependencies**:
    You will need the following Python packages:
    ```bash
    pip install langchain langgraph langchain-groq langchain-community pydantic
    ```
    *Note: You might also need `ipython` and `jupyter` if running the notebook locally.*

3.  **Environment Variables**:
    You must have a Groq API key. Set it in your environment:
    ```bash
    export GROQ_API_KEY="your_groq_api_key_here"
    ```
    *(The notebook expects this environment variable to be set).*

## Usage

1.  Open the **`agent_sql.ipynb`** notebook.
2.  Run the cells in order. The notebook will:
    - Create a sample SQLite database (`employee.db`) with tables: `employees`, `customers`, and `orders`.
    - Define the agent's tools and workflow graph.
    - Execute a series of sample questions to demonstrate the agent's capabilities.

## Example Queries

The agent handles various types of questions, such as:

- *"Tell me name of the employee who's salary is more than 50000?"*
- *"Show me all the orders"*
- *"What are the phone numbers of customers whose last name is 'Smith'?"*

## Workflow

The agent follows this simple graph-based workflow:
1.  **Fetch Table Info**: Gets schema details (if needed).
2.  **Query Generator**:
    - Generates SQL based on the question and schema.
    - Executes the query.
    - **If Error**: Loops back to retry with error context.
    - **If Success**: Generates a natural language final answer.
3.  **End**: Returns the final answer to the user.
