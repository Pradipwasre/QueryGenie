# MYSQL Installation 

Installation and Setup of MySQL and MySQL Workbench
MySQL is a widely used relational database management system, and MySQL Workbench is a graphical tool for working with MySQL databases. Here's a detailed guide on how to install and set up MySQL and MySQL Workbench on macOS, Linux, and Windows.

## On macOS
### Installing MySQL
Download MySQL:

Visit the MySQL Community Downloads page. https://dev.mysql.com/downloads/mysql/
Select the macOS version (DMG Archive) and download it.
Install MySQL:

Open the downloaded DMG file and follow the on-screen instructions.
During the installation, you'll be prompted to set up a root password. Make sure to remember this password.


### Download MySQL Workbench:

Visit the MySQL Workbench Downloads page. https://dev.mysql.com/downloads/workbench/
Select the macOS version and download it.
Install MySQL Workbench:

Open the downloaded DMG file and drag MySQL Workbench to the Applications folder.


## On Linux (Ubuntu)

### Installing MySQL

Update Package Index:
sudo apt update

Install MySQL:
sudo apt install mysql-server

Secure MySQL Installation:
sudo mysql_secure_installation

Follow the prompts to set up the root password and secure your MySQL installation.
Start MySQL Service:
sudo systemctl start mysql

Optionally enable it to start on boot:
sudo systemctl enable mysql


Installing MySQL Workbench


### Download MySQL Workbench:

Visit the MySQL Workbench Downloads page. https://dev.mysql.com/downloads/workbench/
Select the Linux version and download the DEB file.

Install MySQL Workbench:
sudo dpkg -i mysql-workbench-community-*.deb
sudo apt-get -f install  # To fix any dependency issues


## On Windows

### Installing MySQL

Download MySQL Installer:

Visit the MySQL Community Downloads page. https://dev.mysql.com/downloads/mysql/
Download the MySQL Installer for Windows.

Install MySQL:
Run the downloaded installer and follow the setup instructions.
During the installation, select the "Developer Default" setup type for a complete MySQL installation.
Set up a root password when prompted.

Start MySQL Server:

The MySQL server should start automatically after installation. If not, you can start it via the MySQL Workbench or from the command line.


### Installing MySQL Workbench
Download MySQL Workbench:

The MySQL Workbench installer can be installed with the MySQL Installer. If not Visit the MySQL Workbench Downloads page. https://dev.mysql.com/downloads/workbench/
and download microsoft version.

Open MySQL Workbench:

Once installed, you can open MySQL Workbench from the Start menu.


## Connecting to MySQL Server using MySQL Workbench

Launch MySQL Workbench:

Open MySQL Workbench from your Applications (macOS), start menu (Windows), or application menu (Linux).
Create a New Connection:

Click on the "MySQL Connections" tab and then click the "+" button to create a new connection.
Enter the connection name, hostname (usually localhost), port (default is 3306), and the root username.
Click "Test Connection" and enter the root password you set during installation.
If the connection is successful, click "OK" to save it.
Manage Databases:

Once connected, you can manage your databases, run queries, and perform other database tasks using MySQL Workbench's graphical interface.


References:

- Install mysql -> For mac: brew install mysql or follow https://flaviocopes.com/mysql-how-to-install/
- Run: mysql.server start
- Install mysql-workbench -> https://www.mysql.com/products/workbench/
- Install mysql-connector -> pip install mysql-connector


# ==Project Documentation==


# QueryGenie Project Documentation

A Text-to-SQL GenAI application built with LangChain and Streamlit. This document explains the project structure, code flow, and key concepts so you can understand the project deeply and answer interview questions confidently.

---

## 1. What This Project Does

QueryGenie lets a user type a question in plain English (or ask for a chart) and get answers from a MySQL database, without writing SQL themselves.

Two things happen behind the scenes:

- A SQL Agent converts the user's question into SQL, runs it against the database, and returns the answer in natural language.
- A Python Agent takes that answer and writes Plotly code to visualize it, if the user asked for a plot/graph/chart.

The interface is built in Streamlit, so it behaves like a chatbot.

---

## 2. Project Structure (3 Files)

| File | Responsibility |
|---|---|
| `agent.py` | Creates the two LangChain agents (SQL agent and Python agent) and connects them to the LLM and database |
| `chat_app.py` | Streamlit UI, session state, routing user input to the right agent, rendering responses |
| `helper.py` | Small utility functions to clean and format text/image/code output before displaying |

Think of it as:
- `agent.py` = the brain (decides what SQL to run, what Python to generate)
- `chat_app.py` = the face (chat interface, decides which brain to call)
- `helper.py` = the cleanup crew (formats messy LLM output for display)

---

## 3. Code Flow (Step by Step)

1. App starts, `chat_app.py` calls `create_agent_for_sql()` and `create_agent_for_python()` from `agent.py`, once, and stores them in `st.session_state` so they persist across reruns.
2. User types a question in the chat input.
3. `chat_app.py` checks if the question contains plotting keywords (`plot`, `graph`, `chart`, `diagram`).
4. If NOT a plot request:
   - It goes straight to the SQL agent.
   - The SQL agent (from `agent.py`) uses the LLM to figure out the right SQL query, executes it on the MySQL database, and returns a natural language answer.
5. If it IS a plot request:
   - First the SQL agent gets the raw data/answer.
   - Then that answer is passed to the Python agent, which writes Plotly code to visualize it.
   - `chat_app.py` extracts the Python code block from the agent's response using `display_python_code_plots()` (from `helper.py`).
   - It patches the code (adds pandas import, replaces `fig.show()` with `st.plotly_chart(...)`) and executes it live with `exec()`.
6. The response (text, image, or plot code) is displayed and saved into `st.session_state.messages` so the chat history persists.

---

## 4. Deep Dive: agent.py

### 4.1 What it sets up

- Loads Groq API key from `.env` (Groq is used as an OpenAI-compatible LLM provider, model: `openai/gpt-oss-120b`)
- Connects to a MySQL database called `ecommerce` using `SQLDatabase.from_uri(...)`
- Defines two agent creator functions:
  - `create_agent_for_sql()`
  - `create_agent_for_python()`

### 4.2 create_agent_for_sql()

This builds a LangChain SQL Agent using `create_sql_agent()`.

Key pieces:
- `SQLDatabaseToolkit`: gives the agent tools to list tables, describe schema, and run SQL queries against the database.
- `ConversationBufferMemory`: stores conversation history so the agent remembers earlier questions in the session.
- `SQLChatMessageHistory`: persists that memory into a MySQL table called `message_store`, so history survives even if the app restarts.
- `CUSTOM_SUFFIX`: a custom prompt appended to the agent's instructions. It tells the agent:
  - Do not hallucinate, only use data from the tables
  - Use `LOWER()` and `LIKE` for case-insensitive/fuzzy text matching
  - How to calculate "return percentage" (business logic baked into the prompt)
  - Return "No results found" instead of making something up
  - Final answer must strictly be the SQL query's output, nothing extra

Interview point: This is called prompt engineering for domain-specific reliability. Business logic and guardrails are added directly into the agent's prompt rather than hardcoded in Python.

### 4.3 create_agent_for_python()

This builds a separate LangChain agent using `create_openai_functions_agent()` with a `PythonREPLTool`.

Key pieces:
- Its only job is to take data/text and write Plotly visualization code.
- The instructions explicitly say: use Plotly only, not Matplotlib, and return code wrapped in a ```python code block.
- `AgentExecutor` wraps the agent so it can actually run the Python REPL tool if needed.

Interview point: Why two separate agents instead of one? Separation of concerns. The SQL agent focuses only on database logic and accuracy. The Python agent focuses only on visualization. Mixing both into a single agent's prompt would confuse the model and reduce reliability. This is a common multi-agent design pattern.

---

## 5. Deep Dive: chat_app.py

### 5.1 Session state setup

Streamlit reruns the entire script on every user interaction. To avoid recreating agents (which is slow and would wipe memory) on every rerun, the code stores agents inside `st.session_state`:

```python
if 'agent_memory_sql' not in st.session_state:
    st.session_state['agent_memory_sql'] = create_agent_for_sql()
    st.session_state['agent_memory_python'] = create_agent_for_python()
```

This is the standard Streamlit pattern for anything expensive that should persist across reruns (agents, database connections, chat history).

### 5.2 generate_response()

This is the router function.

- If `code_type == "python"`: first calls the SQL agent to get data, checks if the response indicates the agent is confused (keywords like "please provide", "don't know", "vague request"), and if not, sends that data to the Python agent to generate plotting code.
- Else: calls the SQL agent directly and returns its answer.
- Wraps every agent call in try/except, logs the error, and returns `"NO_RESPONSE"` on failure so the UI can show a friendly retry message instead of crashing.

Interview point: Why check for keywords like "don't know" in the SQL response? Because LLM agents sometimes respond with clarifying questions instead of failing loudly. This is a manual fallback/guardrail check on top of the agent's own error handling, since agent outputs aren't fully predictable.

### 5.3 reset_conversation()

Clears `st.session_state.messages` and recreates fresh SQL and Python agents. Bound to the "Reset Chat" button using `on_click`.

### 5.4 Rendering existing chat history

Loops through `st.session_state.messages` and re-renders each message based on its role:
- `assistant` or `error` → rendered via `display_text_with_images()`
- `plot` → the stored code string is executed again with `exec()` to redraw the chart
- anything else (`user`) → rendered as plain markdown

This is why plots reappear correctly even after the page reruns.

### 5.5 Handling new input

```python
if prompt := st.chat_input("Please ask your question:"):
```

This is the walrus operator: it assigns the chat input to `prompt` and checks in the same line if it is truthy (non-empty).

Then the logic branches:
- Plot-related keywords present → build additional context from the last assistant message, call `generate_response("python", prompt)`, extract and execute the returned Plotly code.
- Otherwise → treat it as a pure SQL/text question, optionally attach up to 2 previous assistant messages as context, call `generate_response("sql", prompt)`.

Interview point: Why attach previous assistant messages as context manually, when there's already a `ConversationBufferMemory` in the agent? This is an extra safety layer to make sure recent context is explicitly available to the agent even if the memory retrieval inside LangChain doesn't surface it well. It is redundant by design, for robustness.

### 5.6 Executing generated code safely (relatively)

```python
code = "import pandas as pd\n" + code.replace("fig.show()", "")
code += "st.plotly_chart(fig, theme='streamlit', use_container_width=True)"
exec(code)
```

The raw code from the LLM is patched:
- `fig.show()` is removed (that only works in local Python, not Streamlit)
- Replaced with `st.plotly_chart(...)` so it renders inside the Streamlit app

Interview point (important, be ready for this): Using `exec()` on LLM-generated code is a security risk in production. It should be mentioned as a known limitation if asked "how would you improve this project." A safer approach would be to sandbox execution, validate the code, or restrict allowed operations before running it.

---

## 6. Deep Dive: helper.py

### 6.1 display_text_with_images(text)

The SQL agent's response might contain image URLs mixed with text (for example, product image links from the ecommerce table). This function:
- Uses regex to find any URLs ending in something like `image....jpg`
- Splits the text around those URLs
- Displays each text chunk with `st.markdown()`
- Displays each matched image with `st.image()`

It also skips over chunks that are pure punctuation with no real letters, to avoid rendering junk fragments.

### 6.2 display_python_code_plots(text)

Uses a regex pattern to extract code between triple backtick python fences:

```python
pattern = r'```python\s(.*?)```'
```

Returns the first match (the actual Plotly code), or `None` if the LLM didn't return code in the expected format.

Interview point: Why regex instead of parsing more robustly? Because LLM output for code blocks is fairly consistent in format, regex is simple and fast for this use case. A more robust production version could use a structured output/JSON mode instead of regex extraction.

---

## 7. Key Concepts to Be Ready to Explain in Interviews

- LangChain Agent: An LLM combined with a set of tools, where the LLM decides which tool to call and in what order, instead of following a fixed script.
- SQLDatabaseToolkit: A pre-built LangChain toolkit that gives an agent tools to inspect a database schema and run queries safely.
- ConversationBufferMemory: Stores the full conversation so far and feeds it back into the prompt, giving the agent short-term memory.
- Prompt engineering via CUSTOM_SUFFIX: Injecting business rules and guardrails directly into the agent's prompt to control its behavior without changing code logic.
- Multi-agent architecture: Splitting responsibilities (SQL generation vs code generation) between two separate agents instead of one, for reliability and focus.
- Streamlit session state: The mechanism Streamlit provides to persist data (like agents, chat history) across reruns of the script, since Streamlit reruns top to bottom on every interaction.
- Groq as an LLM provider: Used here as an OpenAI-compatible endpoint, which is why `ChatOpenAI` is used with a custom `base_url` pointing to Groq instead of OpenAI.

---

## 8. Common Interview Questions to Practice

1. Why are there two separate agents instead of one agent doing everything?
2. What is the purpose of `CUSTOM_SUFFIX` in the SQL agent, and why is business logic (like return percentage calculation) written into the prompt instead of Python code?
3. How does the app avoid recreating the LLM agents on every user interaction in Streamlit?
4. What is the risk of using `exec()` on LLM-generated Plotly code, and how would you make it safer?
5. How does chat history persist across app restarts? (Hint: `SQLChatMessageHistory` writing to the `message_store` table)
6. What happens if the SQL agent doesn't understand a question? How does the app detect and handle that?
7. Why is `unidecode` used on the input text before sending it to the agent?
8. Walk through what happens end to end when a user asks: "Show me a bar chart of total sales by category."

---

## 9. Suggested Improvements (Good Talking Points)

- Replace `exec()` based code execution with a sandboxed or validated execution approach.
- Replace regex-based code block extraction with structured output (for example, asking the LLM to return JSON with a `code` field).
- Add input validation before sending user prompts to the database-connected agent, to reduce risk of prompt injection affecting SQL generation.
- Move hardcoded credentials (currently `'password'` as a placeholder string in the connection URI) into environment variables consistently, matching how `GROQ_API_KEY` is already loaded from `.env`.
