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


# System Requirements for Open AI API Execution

Using the OpenAI API involves several considerations related to the system requirements and setup to ensure smooth and efficient operation. Below are the key aspects to consider:

### 1. Operating System
The OpenAI API can be accessed from any operating system that supports HTTP requests. Commonly used operating systems include:

Windows: Windows 10 or later is recommended.
macOS: macOS 10.15 (Catalina) or later.
Linux: Modern distributions such as Ubuntu 18.04 or later

### 2. Hardware Requirements
The hardware requirements are relatively minimal, as the actual processing happens on OpenAI's servers. However, for development and integration purposes, consider the following:

Processor: A modern multi-core processor 
Memory: At least 8GB of RAM is recommended, if you are handling large datasets or multiple applications simultaneously consider more.
Storage: Sufficient storage space for your development environment and any necessary dependencies. SSD storage is recommended for faster read/write operations.

### 3. Internet Connection
A stable and reasonably fast internet connection is essential, as interactions with the OpenAI API require sending and receiving data over the internet.

Speed: A minimum of 2-4 Mbps is recommended to handle API requests and responses efficiently.
Stability: A stable connection with low latency is crucial to avoid timeouts and ensure quick responses from the API.

### 4. API Key and Authentication
API Key: To use the OpenAI API, you need an API key, which you can obtain by signing up for an account on the OpenAI platform and creating an API key.








# Code Execution Instructions

## Python version 3.10

To create a virtual environment and install requirements in Python 3.10 on different operating systems, follow the instructions below:

### For Windows:

Open the Command Prompt by pressing Win + R, typing "cmd", and pressing Enter.

Change the directory to the desired location for your project:


`cd C:\path\to\project`

Create a new virtual environment using the venv module:


`python -m venv myenv`

Activate the virtual environment:

`myenv\Scripts\activate`


Install the project requirements using pip:

`pip install -r requirements.txt`

### For Linux/Mac:
Open a terminal.

Change the directory to the desired location for your project:

`cd /path/to/project`

Create a new virtual environment using the venv module:

`python3.10 -m venv myenv`


Activate the virtual environment:

`source myenv/bin/activate`

Install the project requirements using pip:

`pip install -r requirements.txt`

These instructions assume you have Python 3.10 installed and added to your system's PATH variable.

## Execution Instructions if Multiple Python Versions Installed

If you have multiple Python versions installed on your system, you can use the Python Launcher to create a virtual environment with Python 3.10. Specify the version using the -p or --python flag. Follow the instructions below:

For Windows:
Open the Command Prompt by pressing Win + R, typing "cmd", and pressing Enter.

Change the directory to the desired location for your project:

`cd C:\path\to\project`

Create a new virtual environment using the Python Launcher:

`py -3.10 -m venv myenv`

Note: Replace myenv with your desired virtual environment name.

Activate the virtual environment:

`
myenv\Scripts\activate
`

Install the project requirements using pip:

`pip install -r requirements.txt`


### For Linux/Mac:
Open a terminal.

Change the directory to the desired location for your project:

`cd /path/to/project
`
Create a new virtual environment using the Python Launcher:


`python3.10 -m venv myenv`


Note: Replace myenv with your desired virtual environment name.

Activate the virtual environment:

`source myenv/bin/activate`

Install the project requirements using pip:

`pip install -r requirements.txt`


By specifying the version using py -3.10 or python3.10, you can ensure that the virtual environment is created using Python 3.10 specifically, even if you have other Python versions installed.



## Run streamlit application

`streamlit run app.py`




==============================================================


Complete Project view: 

QueryGeine is like hiring a smart Assitant, who? 
- Understands normal english Question from shop owner.
- Knows how to search the register himself (write SQL query behind scenes)
- Reads the answer from the register 
- If owner ask for the picture / graph, it assist to provide the graph.

# The Three Files:
**chat_app.py** : The face of the project. This is that chat screen the user sees and types into (build the stremit)
**agent.py** : The brain of the project. This creates two AI worker. One who talk to the database, one who write the python code for charts.
**helper.py** : The support staff. Small helper functions that clean and display the AI's reply nicely, and pull out the image or code blocks from the reply text.


3 - layer pattern: 
1. Frontend / UI layer  : (chat_app.py)
2. Logic/brain layer    : (agent.py)
3. Utility/helper layer : (helper.py)

---------------------------------

# what happens the moment the app starts (before user types anything): 

chat_app.py : 
- load_dot.env 
- st.set_page_config(page_title="QueryGenie Analytics")
- session_stats = 
- SQL agent / python agent


==================================

# agent.py

load_dotenv("/Users/pradipwasre/Desktop/QueryGenie/.env")
GROQ_API_KEY = os.environ.get("GROQ_API_KEY")
os.environ['GROQ_API_KEY'] = GROQ_API_KEY

GROQ_BASE_URL = "https://api.groq.com/openai/v1"
DEFAULT_MODEL_NAME = "openai/gpt-oss-120b"

CUSTOM_SUFFIX = """Begin!

Relevant pieces of previous conversation:
{chat_history}
(Note: Only reference this information if it is relevant to the current query.)

Question: {input}
Thought Process: It is imperative that I do not fabricate information not present in any table or engage in hallucination; maintaining trustworthiness is crucial.
In SQL queries involving string or TEXT comparisons, I must use the `LOWER()` function for case-insensitive comparisons and the `LIKE` operator for fuzzy matching. 
Queries for return percentage is defined as total number of returns divided by total number of orders. You can join orders table with users table to know more about each user.
Make sure that query is related to the SQL database and tables you are working with.
If the result is empty, the Answer should be "No results found". DO NOT hallucinate an answer if there is no result.


""
never hallucinate answers : if no SQL result, repond "No results found"
Final output must be only the SQL qury result, nothing extra

This acts like a saftey net to keep the agent trustworthy and SQL- focused
"" 


My final response should STRICTLY be the output of SQL query.

{agent_scratchpad}
"""

langchain_chat_kwargs = {
    "temperature": 0,
    "max_tokens": 4000,
    "verbose": True,
}
chat_openai_model_kwargs = {
    "top_p": 1.0,
    "frequency_penalty": 0.0,
    "presence_penalty": -1,
}
----------
**In short**:
- Loads your Groq API key securly.
- Sets up the Groq API endpoint the default model.
- Defines strict SQL query reles via a customer suffix.
- Configures the LLM to be deterministic, verbose and trustworthy.

----------

# Code if you've set up passowrd in mysql
# import urllib.parse

# password = urllib.parse.quote_plus("you-password")  # Replace "your#password" with your actual password
# db = SQLDatabase.from_uri(f"mysql://root:{password}@localhost:3306/ecommerce")

db = SQLDatabase.from_uri(f"mysql://root:{'password'}@localhost:3306/ecommerce")

# db = SQLDatabase.from_uri("mysql://localhost:3306/ecommerce?user=root")


def get_chat_openai(model_name):
    """
    Returns an instance of the ChatOpenAI class initialized with the specified model name.

    Args:
        model_name (str): The name of the model to use.

    Returns:
        ChatOpenAI: An instance of the ChatOpenAI class.

    """
    llm = ChatOpenAI(
        model_name=model_name,
        api_key=GROQ_API_KEY,
        base_url=GROQ_BASE_URL,
        model_kwargs=chat_openai_model_kwargs,
        **langchain_chat_kwargs
    )
    return llm

- model_name : choose the model groq/openAI.... ("openai/gpt-oss-120b")
- api_key = pulled from .env (file GROQ_API_KEY)
- base_url : Groq's API Endpoint
- model_kwargs: {temprature . max tokens or toolkits}

---> First snippet df = SQLDatabase... conencts langchain to your MySQL database/
---> get_chat_openai = prepare the Groq LLM instanace with all the right credentials and paramerters.

--------------****------------
def get_sql_toolkit(tool_llm_name: str):
    """
    Instantiates a SQLDatabaseToolkit object with the specified language model.

    This function creates a SQLDatabaseToolkit object configured with a language model
    obtained by the provided model name. The SQLDatabaseToolkit facilitates SQL query
    generation and interaction with a database.

    Args:
        tool_llm_name (str): The name or identifier of the language model to be used.

    Returns:
        SQLDatabaseToolkit: An instance of SQLDatabaseToolkit initialized with the provided language model.
    """
    llm_tool = get_chat_openai(model_name=tool_llm_name)
    toolkit = SQLDatabaseToolkit(db=db, llm=llm_tool)
    return toolkit


-1.  Toolkit in Langchain : 
    -A Toolkit bundle of tools that an agent can use.
    - Think of lika Swiff Army knife : each blade tool does somthing specefic, but the tookit organizes them for the agent. 
    -**SQLDatabaseToolkit** : specefically give the agent:
        - Knowledge of the Database schema (tables, column)
        - Functions to generate and run SQL queries
        - Utilities to interpret results safely

-2. LLM + DataBase = Reasoning + Action:    
    - The LLM (llm_tool) provides reasoning : it understnd the user, natural language
    - The Database (db) provides ground truth data : .
    - The toolkit fuses them : it let the llm translate human questions into SQL queries and then execute them.

    - **Example** : "show me all customer who returned more than 5 orders.

select user_id, count(*) as returns
from orders
where status = 'returned'
group by user_id
having count(*) > 5;

- Why pas the LLM into the Toolkit? 
    - The toolkit isn't smart by itself : it needs the LLm to:
        - Parse natural language into sql.
        - Decide which tables/ column to use.
        - handle ambiguous queries (e.g : "orders" vs "purchses)
    - The databse is libraray, the toolkit is a librarian's desk, and the llm is tha librarian's bran that understnads your quesion and fetches the right book.
    
    - get_chat_openai : bulds the LLM.
    - SQLDatabaseToolkit : builds the database tools.
    - get_sql_toolkit : Combines them


----------------************--------------------
def get_agent_llm(agent_llm_name: str):
    """
    Retrieve a language model agent for conversational tasks.

    Args:
        agent_llm_name (str): The name or identifier of the language model for the agent.

    Returns:
        ChatOpenAI: A language model agent configured for conversational tasks.
    """
    llm_agent = get_chat_openai(model_name=agent_llm_name)
    return llm_agent

-- Give me a lauguage model that can talk and reason, so i can plug it into an agent.
--- purpose:
        - This function is a helper that creates a ChatOpenAI model instance specifically for the agent.
        - It's differnt form the SQL toolkit functions. here, the llm is not tied to a database tools, but instead acts as the conversational brain of the agent.

-2. Arugments : 
-   agent_llm_name: str: 
    - This is the model idnetifies (e.g : "openai/gpt-oss-120b' or "gpt-4.1")
    - It tells the funtions which model to load 
    - passing it an argument makes the function flexible - you can swap models without rewiring code.

Example : 
    - For SQL-heavy task, you might use the smaller model.
    - for conversational reasoning, you might use a larger, more capable model.


------------------------------***--------------------

def create_agent_for_sql(tool_llm_name: str = DEFAULT_MODEL_NAME, agent_llm_name: str = DEFAULT_MODEL_NAME):
    """
    Create an agent for SQL-related tasks.

    Args:
        tool_llm_name (str): The name or identifier of the language model for SQL toolkit.
        agent_llm_name (str): The name or identifier of the language model for the agent.

    Returns:
        Agent: An agent configured for SQL-related tasks.

    """
    # agent_tools = sql_agent_tools()
    llm_agent = get_agent_llm(agent_llm_name)
    toolkit = get_sql_toolkit(tool_llm_name)
    message_history = SQLChatMessageHistory(
        session_id="my-session",
        # connection_string="mysql://localhost:3306/ecommerce?user=root", # use this if password need f"mysql://root:{password}@localhost:3306/ecommerce"
        connection_string=f"mysql://root:{'password'}@localhost:3306/ecommerce",
        table_name="message_store",
        session_id_field_name="session_id"
    )
    memory = ConversationBufferMemory(memory_key="chat_history", input_key='input', chat_memory=message_history, return_messages=False)

    agent = create_sql_agent(
        llm=llm_agent,
        toolkit=toolkit,
        agent_type="tool-calling",
        input_variables=["input", "agent_scratchpad", "chat_history"],
        suffix=CUSTOM_SUFFIX,
        memory=memory,
        agent_executor_kwargs={"memory": memory, "handle_parsing_errors": True},
        verbose=True,
    )
    return agent


    -- Puprose: 
            - This function creates a full SQL agent  : An intellegent system that can: 
                - uderstant nautral languge questions
                - Translate them into SQL queries.
                - Executes those queries on the databse.
                - return results safely and cleary.

        - It's the Glue that combines:
            - The llm brain (get_agent_llm)
            - The SQL Toolkit (get_sql_toolkit)
            - The memory system (conversatinal buffer memeory)
            - The agent orchiestation (create_sql_agent)

llm_agent = get_agent_llm(agent_llm_name)
    - Creates the brain for the agent (conversation + orchistartion)
    - this is the model that interprets user queries and decies what to do.   
toolkit = get_sql_toolkit(tool_llm_name)
    - Creates the toolset for interacting with databse
    - Includes schema awareness , query generation, and exceuction funtions.

message_history = SQLChatMessageHistory(
        session_id="my-session",
        # connection_string="mysql://localhost:3306/ecommerce?user=root", # use this if password need f"mysql://root:{password}@localhost:3306/ecommerce"
        connection_string=f"mysql://root:{'password'}@localhost:3306/ecommerce",
        table_name="message_store",
        session_id_field_name="session_id"

- stores past conversation in a SQL table (message_store)
- Ensures the agent remembers the context across turns.
- session_id = "my-sesion" -> groups all message under one session.
-> this is persistent memory -> unlike temporary buffers, it saves history in the DB.

memory = ConversationBufferMemory(memory_key="chat_history", input_key='input', chat_memory=message_history, return_messages=False)

    - Wraps the messsages history into langchain memoery object.
    - memory_key = "chat_history" -> tells the agent where to injext the past converstaion
    - input_key = 'input' mapping user queries into the meomory system
    -- This is short-term memory layerd on top of the SQL message history. 

agent = create_sql_agent(
        llm=llm_agent,
        toolkit=toolkit,
        agent_type="tool-calling",
        input_variables=["input", "agent_scratchpad", "chat_history"],
        suffix=CUSTOM_SUFFIX,
        memory=memory,
        agent_executor_kwargs={"memory": memory, "handle_parsing_errors": True},
        verbose=True,
    )
    return agent

- llm = llm_agent : the reasoning brain.
- toolkit = the SQL tools.
-agent_type = "tool-calling' : agent can call tools (sql excution)
- input_variables = definse what goes into the prompt:
    - input : "user's query"
    - agent_scratchpad : agent's internal reasoning steps.
    - chat_history = past conversation.
- suffix=CUSTOM_SUFFIX : strict SQL rules (no hallucination, case-insensityive queries,)
-memory=memory : conversation + SQL history
- agent_executor_kwargs : 
    - handle_parsing_errors : prevents crashes if SQL parsing fails.
- verbose=True : Logs detailed execution for debugging.

- Use : 
    - Accept natural language quesion like: 
        - Show me all users who return more the 10 orders.
        - What's the average order value last month?
    
    - Trnslates them into SQL queries
    - Executes those queries on the ecommerce datbase.
    - Returns only the SQL queries, with no hallucination.

---------------------****------------------------

 create_agent_for_python(agent_llm_name: str = DEFAULT_MODEL_NAME):
    """
    Create an agent for Python-related tasks.

    Args:
        agent_llm_name (str): The name or identifier of the language model for the agent.

    Returns:
        AgentExecutor: An agent executor configured for Python-related tasks.

    """
    instructions = """You are an agent designed to write a python code to answer questions.
            You have access to a python REPL, which you can use to execute python code.
            If you get an error, debug your code and try again.
            You might know the answer without running any code, but you should still run the code to get the answer.
            If it does not seem like you can write code to answer the question, just return "I don't know" as the answer.
            Always output the python code only.
            Generate the code <code> for plotting the previous data in plotly, in the format requested. 
            The solution should be given using plotly and only plotly. Do not use matplotlib.
            Return the code <code> in the following
            format ```python <code>```
            """
    tools = [PythonREPLTool()]
    base_prompt = hub.pull("langchain-ai/openai-functions-template")
    prompt = base_prompt.partial(instructions=instructions)
    llm = ChatOpenAI(
        model=agent_llm_name,
        api_key=GROQ_API_KEY,
        base_url=GROQ_BASE_URL,
        temperature=0
    )
    agent = create_openai_functions_agent(llm, tools, prompt)
    agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)
    return agent_executor


    ## This function bulds python agent - an intellegent system that can:
        - Accept natural langue quesitons
        - Generate python code as the answer
        - Execute that code in python REPL (Read - Eval - Print Loop)
        - Debug itself if erros occur.
        - Return the final code (formatted neatly)

    - It's essentially like a python programmer

swap model : 
    - Speed vs Accuracy.
    - Cost vs capabilty.

llm = ChatOpenAI(
        model=agent_llm_name,
        api_key=GROQ_API_KEY,
        base_url=GROQ_BASE_URL,
        temperature=0

- Crates the LLM braing for the agent
- Temprature = 0 : deterministic , no randomness.
- Uses Groq API key + base USE - > connects to the Groq LLM service

agent = create_openai_functions_agent(llm, tools, prompt)

    - combines:
        - The LLM brain (llm)
        - the python REPL tool (tools) 
        - The Cusom pormpt (prompt)
    
    - creates an agent that can:
        - parse question
        - Genrate python code.
        - Execute / debug the code. 
        - return detils
    
    This is fusion step : brain + tool + rules = Agent.

======

user types question : 
        |
        |
Does it Contatin Plot/ graph/chart/diagram?
        |
        |
------------------        
|                   |
Yes                NO
|                    |
Chart Path          Normal Q&A path
