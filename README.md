# Project Started on GenAI - Work in Progress


# QueryGenie — Modular Architecture Explanation (Teaching Script)

Yeh document aapko class mein line-by-line samjhane ke liye hai — ki har file company mein kis role mein kaam karti hai, aur pura system ek dusre se kaise connected hai.

---

## Sabse Pehle: "Modular Code" Kya Hota Hai Aur Yeh Important Kyun Hai

**Opening line for students:**

"Agar hum sab kuch ek hi file mein likh dete — database connection, AI agent, UI, helper functions, sab ek jagah — toh yeh kaam toh chal jaata, lekin yeh 'company-level code' nahi kehlata. Real companies mein jab 5-10 engineers ek hi project pe kaam karte hain, tab agar sab kuch ek file mein ho, toh do log ek saath us file ko edit karenge aur conflict ho jayega. Isliye hum code ko responsibility ke hisaab se alag-alag files mein todte hain — isko kehte hain **Separation of Concerns**."

Is project mein hum 4 layers mein kaam divide karte hain:

| Layer | File | Real-world analogy |
|---|---|---|
| **Brain / Intelligence layer** | `src/agent.py` | Data Science team — jo actual query samajhta hai aur SQL/Python likhta hai |
| **Interface layer** | `src/chat_app.py` | Frontend/UI team — jo user se baat karta hai |
| **Utility layer** | `src/helper.py` | Shared tools team — reusable helper functions jo koi bhi module use kar sakta hai |
| **Config/Secrets layer** | `.env` | DevOps/Security team — sensitive keys yahan rakhi jaati hain, code mein kabhi nahi |

Yeh exact wahi pattern hai jo aap kisi bhi production GenAI product mein dekhoge — chahe wo ChatGPT Enterprise ho, chahe Snowflake Cortex.

---

## Folder Structure Recap 

```
QueryGenie/
    .env                  --> Secret keys (OpenAI API key)
    requirements.txt      --> Dependency list
    Readme.md             --> Documentation
    Query_To_Dashboard.ipynb  --> Notebook: DB setup + testing ground
    data/                 --> Raw CSV files (source of truth for MySQL tables)
    src/
        agent.py           --> Core AI logic (SQL agent + Python agent)
        chat_app.py        --> Streamlit chat interface (main app)
        helper.py          --> Reusable utility functions
        stream_lit_demo.py --> Practice/demo file (not part of main app)
```

"Notice karo — `src` folder alag hai `data` folder se. Yeh bhi ek convention hai: **source code** aur **data** ko kabhi mix mat karo. Agar kal aapko dataset badalna hai, aapko `src` ke andar kuch touch nahi karna padega."

---

## File 1: `.env` — The Secrets Vault

**Kya hai:**
```
OPENAI_API_KEY=your_actual_key_here
```

**Class mein kya bolna hai:**

"Yeh file sabse chhoti hai lekin sabse sensitive hai. Isme humara OpenAI API key store hota hai. Yeh key kabhi bhi directly code ke andar hardcode nahi karte — kyun? Kyunki agar hum apna code GitHub pe push kar dete hain aur key code ke andar likhi hui hai, toh duniya bhar ke log wo key dekh sakte hain aur apne bill pe use kar sakte hain. Isliye `.env` file ko `.gitignore` mein daala jaata hai taaki yeh kabhi GitHub pe upload na ho."

`chat_app.py` mein yeh line dekhna:
```python
load_dotenv(os.path.join(parent_dir, ".env"))
```
Yeh line `.env` file ko padhti hai aur uski values ko environment variables ke roop mein load karti hai — jise `agent.py` ke andar `ChatOpenAI` class internally use karti hai.

---

## File 2: `src/agent.py` — The Brain (Sabse Important File)

"Yeh file poore project ka dimaag hai. Isme do alag-alag AI agents define hote hain. Chalo dono ko step by step samajhte hain."

### Part A — Configuration & Prompt Engineering

```python
CUSTOM_SUFFIX = """Begin!
Relevant pieces of previous conversation:
{chat_history}
...
My final response should STRICTLY be the output of SQL query.
{agent_scratchpad}
"""
```

**Samjhao:** "Yeh ek **prompt template** hai — humara instruction set jo LLM ko diya jaata hai. Notice karo teen important cheezein:

1. `{chat_history}` — yeh placeholder hai, jahan pichli conversation daali jaati hai, taaki agent ko memory mile
2. Ek business rule bhi likha hai seedha prompt mein: *'return percentage = total returns / total orders'* — yeh domain knowledge hai jo hum LLM ko manually sikha rahe hain, kyunki LLM ko khud se nahi pata ki humari company return percentage kaise calculate karti hai
3. `DO NOT hallucinate` wali line — yeh guardrail hai, taaki agent fake answer na de agar result empty ho"

### Part B — Database Connection

```python
db = SQLDatabase.from_uri(f"mysql://root:{'pradip'}@localhost:3306/ecommerce")
```

**Samjhao:** "Yeh line LangChain ko humari MySQL database se jodti hai. `SQLDatabase` ek LangChain wrapper class hai jo database ka schema (table names, column names, data types) khud padh leti hai — hume manually schema likh ke dene ki zaroorat nahi. Isi wajah se hum apne README mein bol paate hain: 'agent understands database schema without being explicitly told'."

**Classroom safety note bolna zaroori hai:** "Yahan password seedha code mein likha hua hai — `'pradip'`. Real production mein yeh galat practice hai, isko bhi `.env` file se load karna chahiye. Hum yahan sirf sikhne ke liye simplify kar rahe hain."

### Part C — Two Helper Functions: `get_chat_openai` and `get_sql_toolkit`

**Samjhao:** "Yeh dono functions **factory functions** hain — inka kaam sirf ek object banana aur return karna hai. `get_chat_openai()` ek ChatOpenAI model object banata hai. `get_sql_toolkit()` us model ko `SQLDatabaseToolkit` ke andar wrap karta hai — yeh toolkit hi agent ko batata hai ki wo SQL queries kaise likhe, kaise run kare, kaise errors handle kare."

"Yeh function-based design isliye hai taaki agar kal humein model badalna ho — GPT-4o se kisi aur model pe — toh humein sirf ek function ka argument badalna padega, poori file rewrite nahi karni padegi. Isko kehte hain **reusability**."

### Part D — `create_agent_for_sql()` — Agent Number 1

**Samjhao:** "Yeh humara pehla agent hai — **SQL Query Engine**. Iska kaam:
1. User ka plain English question leta hai
2. Toolkit se database ka schema padhta hai
3. Sahi SQL query generate karta hai
4. Us query ko MySQL pe run karta hai
5. Result return karta hai

Notice karo `SQLChatMessageHistory` — yeh agent ki memory ko bhi MySQL ke andar hi ek `message_store` table mein save karta hai. Iska matlab agent ko yaad rehta hai ki pichhle sawal kya the — 'previous context' wale kaam ke liye yeh zaroori hai."

`AgentType.ZERO_SHOT_REACT_DESCRIPTION` — "Yeh batata hai ki agent kis reasoning strategy se kaam karega. ZERO_SHOT ka matlab hai ki hum agent ko examples nahi de rahe, sirf instructions de rahe hain, aur REACT ka matlab hai ki agent 'Thought → Action → Observation' cycle follow karta hai — pehle sochta hai, phir action leta hai (query run karta hai), phir result dekhta hai, phir agla step decide karta hai."

### Part E — `create_agent_for_python()` — Agent Number 2

**Samjhao:** "Yeh humara dusra agent hai — **Dashboard Engine**. Iska kaam pehle agent se bilkul alag hai: yeh SQL nahi likhta, yeh **Python plotting code** likhta hai — Plotly use karke."

```python
tools = [PythonREPLTool()]
```
"`PythonREPLTool` ek tool hai jo LLM ko real Python code execute karne ki permission deta hai — matlab yeh agent sirf code suggest nahi karta, wo usse **actually chala ke dekhta bhi hai**, aur agar error aaye toh khud debug karta hai. Yeh line dekho instructions mein: `'If you get an error, debug your code and try again'` — yeh self-correcting agent ka example hai."

**Key teaching moment:** "Ab socho — humare paas do independent agents hain. Ek SQL likhta hai, ek Python likhta hai. Inko hum `chat_app.py` mein **chain** karenge — pehle agent ka output, dusre agent ka input ban jaayega. Isi ko kehte hain **multi-agent orchestration** — yeh bilkul wahi concept hai jo aap Agentic AI mein aage LangGraph mein padhoge, lekin yahan hum ise manually chain kar rahe hain."

---

## File 3: `src/chat_app.py` — The Interface Layer (Orchestrator)

"Yeh file `agent.py` ko call karti hai aur ek chat interface banati hai jahan student real time mein sawaal pooch sakta hai."

### Part A — Setup and Session State

```python
if 'agent_memory_sql' not in st.session_state:
    st.session_state['agent_memory_sql'] = create_agent_for_sql()
    st.session_state['agent_memory_python'] = create_agent_for_python()
```

**Samjhao:** "Streamlit ek interesting behaviour follow karta hai — jab bhi page pe koi interaction hoti hai (button click, text input), **poora script phir se top se bottom tak run hota hai**. Agar hum har baar naya agent bana dete, toh memory har baar reset ho jaati, purani conversation bhool jaati. `st.session_state` ek dictionary jaisi cheez hai jo Streamlit reruns ke beech mein bhi data ko zinda rakhti hai — isliye hum agent ko yahan ek baar banate hain aur session state mein store kar dete hain."

### Part B — `generate_response()` — Routing Logic

**Samjhao:** "Yeh function decide karta hai ki user ka sawaal SQL agent ko bhejna hai ya Python (plotting) agent ko. Dekho iska logic:

```python
if code_type == "python":
    response = st.session_state.sql_agent.invoke(...)  # pehle SQL se data nikalo
    ...
    return st.session_state.python_agent.invoke(prompt)  # phir Python agent ko plot bolo
else:
    return st.session_state.sql_agent.run(prompt)
```

Matlab agar user 'plot' ya 'chart' bolta hai, toh code **pehle SQL agent ko call karta hai data nikalne ke liye, phir uska result Python agent ko deta hai plotting ke liye** — yeh exact hai jo README mein bola tha: 'Chain multiple agents together (SQL agent then coding agent)'."

### Part C — Keyword Detection for Routing

```python
keywords = ["plot", "graph", "chart", "diagram"]
if any(token in prompt.lower() for token in keywords):
```

**Samjhao:** "Yeh ek simple lekin effective trick hai — hum poora NLP classifier nahi bana rahe intent detect karne ke liye, sirf keyword match kar rahe hain. Yeh production mein 'quick and dirty' approach hai — kaam chal jaata hai, lekin agar student 'visualize karo' bole aur 'plot/chart/graph' na bole, toh yeh miss ho jaayega. Yahan aap discuss kar sakte ho — behtar approach kya hoga? (Answer: ek intent-classification LLM call, ya router agent)."

### Part D — Displaying Results Dynamically

```python
elif message["role"] == "plot":
    exec(message["content"])
```

**Important caution for students:** "`exec()` ek dangerous function hota hai kyunki yeh kisi bhi string ko live Python code ke roop mein run kar deta hai. Yahan hum LLM-generated code ko `exec()` kar rahe hain — yeh sirf isliye theek hai kyunki hum controlled classroom environment mein hain aur trusted OpenAI model use kar rahe hain. Production mein aisa karna risky hai agar untrusted input ho — isko **code injection vulnerability** kehte hain. Yeh ek achha discussion point hai security ke baare mein."

---

## File 4: `src/helper.py` — The Utility Layer

"Yeh file chhoti hai lekin important design principle sikhati hai: **DRY — Don't Repeat Yourself**."

### `display_text_with_images()`

**Samjhao:** "Kabhi kabhi LLM ka answer text ke saath image URL bhi return karta hai. Yeh function regex use karke text ke andar se image URLs dhoondhta hai, unhe text se alag karta hai, aur Streamlit mein text aur image dono ko sahi order mein display karta hai."

### `display_python_code_plots()`

```python
pattern = r'```python\s(.*?)```'
matches = re.findall(pattern, text, re.DOTALL)
```

**Samjhao:** "Jab Python agent code generate karta hai, wo usually markdown format mein aata hai — ```python ... ``` ke andar wrapped. Yeh function us wrapper ko hata ke sirf raw executable code nikaalta hai, taaki `chat_app.py` usse `exec()` kar sake."

"Yeh dono functions `chat_app.py` mein import hoke use hote hain:
```python
from helper import display_python_code_plots, display_text_with_images
```
Isi ko kehte hain modular reusability — agar kal koi aur file bhi text-with-image display karna chahe, usse yeh function copy-paste nahi karna, seedha import karna hai."

---

## File 5: `src/stream_lit_demo.py` — The Playground

**Samjhao:** "Yeh file production app ka part nahi hai — yeh sirf ek practice/demo file hai jahan naye Streamlit components try kiye jaate hain, jaise `st.date_input()`. Real companies mein bhi aisi 'scratch' ya 'sandbox' files hoti hain jahan engineers naye ideas test karte hain bina main app ko todhe. Isko explain karna good practice hai — 'har file production-critical nahi hoti, kuch files sirf learning/testing ke liye hoti hain'."

---

## Bonus: `app.py` (Standalone Matplotlib Demo)

Yeh file jo aapne share ki, wo `QueryGenie` ke `src/` folder ka part nahi hai — yeh ek standalone static example hai jo hardcoded data ke saath matplotlib chart dikhata hai. Isko class mein compare karne ke liye use karo:

"Dekho is `app.py` mein data hardcoded hai — `countries` aur `returns_percentage` list manually likhi hui hai. Yeh **static dashboard** hai. Jabki humara `chat_app.py` **dynamic dashboard** banata hai — data live MySQL se aata hai, AI decide karta hai kaunsa chart banana hai. Yeh farak samajhna hi is poore project ka core learning hai: hum manual dashboard building se AI-powered dynamic dashboard generation ki taraf move kar rahe hain."

---

## End-to-End Flow Diagram (Bolke Samjhane Ke Liye)

```
User types question in chat_app.py
        |
        v
generate_response() decides: SQL only, or SQL + Plot?
        |
        v
create_agent_for_sql() [agent.py] --> reads schema --> writes SQL --> runs on MySQL --> returns result
        |
        v (only if plotting needed)
create_agent_for_python() [agent.py] --> writes Plotly code --> executes it
        |
        v
helper.py functions clean/format the output
        |
        v
Streamlit renders final text + chart in browser
```

"Bas yehi ek line mein poora project hai: **User ka English question, SQL bankar data banta hai, data Python code bankar chart banta hai, aur Streamlit sabko browser mein dikhata hai.**"

---

## Suggested Class Flow (Order to Teach)

1. Poora flow diagram board pe pehle draw karo (top-down)
2. `.env` — 2 min (security concept)
3. `agent.py` — 15-20 min (sabse zyada time yahan do, yeh core hai)
4. `chat_app.py` — 10 min (orchestration + Streamlit session state)
5. `helper.py` — 5 min (reusability concept)
6. `stream_lit_demo.py` + `app.py` comparison — 5 min (static vs dynamic dashboard)
7. Live run karke dikhao — question poochke SQL result aur chart dono generate karte dikhao

Total: roughly 45-50 min explanation before hands-on setup.
