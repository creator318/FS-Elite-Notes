#AgenticAI

# 🤖 Agentic HR Assistant with MongoDB + OpenAI + FAISS

You are tasked with building an agentic AI assistant for HR policies using the following tools:

🧠 OpenAI gpt-4o for answering policy questions.
📄 FAISS vector store for storing and retrieving HR policy text.
🛠️ MongoDB tools to interact with employee records.
🤖 LangChain agent to orchestrate reasoning and tool selection.

### Part 1: Create the HR Policy Vector Store
- Use the provided HR policy paragraph (in-memory, no file I/O).
- Chunk the document using `RecursiveCharacterTextSplitter`.
- Embed it using `OpenAIEmbeddings`.
- Store in a FAISS vectorstore (in-memory, no `save_local()`).


```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings
from langchain.schema import Document
from langchain.text_splitter import RecursiveCharacterTextSplitter
import os

os.environ['OPENAI_API_KEY'] = <openai api key>

# 🧾 HR policy as a string (no file I/O)
hr_policy_text = """
At ABC Corporation, full-time employees are entitled to a total of 20 paid leave days per calendar year, including vacation and casual leaves. Sick leave is limited to 10 days annually and must be accompanied by a valid medical certificate for absences longer than two days. Maternity leave of 90 days is granted to female employees, while paternity leave of 15 days is granted to male employees.

Employees are expected to complete 8 hours of work per day, excluding breaks. Remote work is permitted for up to two days a week, subject to manager approval. All employees undergo a 6-month probation period, during which they are not eligible for certain benefits like work-from-home and insurance. Upon successful completion of probation, they become eligible for health and dental insurance coverage.

Salary increments and promotions are reviewed annually in January, based on performance metrics defined by team leads. Any HR grievances must be formally submitted through the internal HR portal and are addressed within 14 business days. Contact information such as phone number and address must be kept up to date in the employee portal to ensure timely communication of official notices.

Termination requires a 30-day notice by either party unless waived by mutual consent. Employees may be placed on performance improvement plans (PIPs) if goals are unmet. Violation of policies, including repeated tardiness or misconduct, may lead to disciplinary action including termination.
"""

splitter = RecursiveCharacterTextSplitter(chunk_size=300, chunk_overlap=50)
docs = splitter.create_documents([hr_policy_text])
embedding = OpenAIEmbeddings()
vectorstore = FAISS.from_documents(docs, embedding)
```

## ✅ Part 2: Implement the Following Tools

Create the following **Python functions** and wrap them as **LangChain tools** so that the agent can call them when needed.

### 🔧 Required Tools:

1. **view_remaining_leaves (name: str)**  
   - Retrieves the total remaining paid leaves for the given employee.
   - Assume 20 total paid leaves per year.
   - Subtract `leaves_taken` from 20 to return the balance.

1. **view_salary (name: str)**  
   - Returns the salary history dictionary for the given employee.
   - Example Output: `"Salary history for Bob: {'2022': 55000, '2023': 58000, '2024': 61000}"`

```python
from pymongo import MongoClient

client = MongoClient('mongodb://student:kmit123$@10.11.52.112:27017/admin')
db = client['hrdb']
employees = db['employees']

def view_remaining_leaves(name: str):
    doc = employees.find_one({"name":name})
    return f"{name} has {20 - doc['leaves_taken']} remaining paid leaves." if doc else 'Employee not found.'    

def view_salary(name: str):
    doc = employees.find_one({"name":name})
    return f"Salary history: {doc.get('salary', {})}" if doc else 'Employee not found.'    
```

## ✅ Part 3: Initialize the LangChain Agent

After creating your tools, configure the AI agent to orchestrate them:

1. Use `ChatOpenAI` with the model `"gpt-4o"` as your LLM.

2. **qa_chain** using `RetrievalQA`  
   - This tool allows the agent to retrieve HR policy details from the FAISS vectorstore.
   - The OpenAI LLM for response generation
   - Use it to answer natural language policy questions like:
     - "What is the sick leave policy?"
     - "Can I work from home during probation?"

3. Define a `tools` list with the following:
   - All four MongoDB-based tools
   - The `qa_chain` for answering policy questions

4. Use `initialize_agent()` from LangChain:
   - Pass the tools list and LLM
   - Set agent type to `"zero-shot-react-description"`
   - Set `verbose=True` to view the agent's reasoning


```python
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI
from langchain.agents import Tool, initialize_agent

llm = ChatOpenAI(model_name='gpt-4o')

retriever = vectorstore.as_retriever()
qa_chain = RetrievalQA.from_chain_type(llm=llm, retriever=retriever)

tools = [
    Tool(name='HRPolicyQA', func=qa_chain.run, description='Answer HR policy questions'),
    Tool(name='ViewRemainingLeaves', func=view_remaining_leaves, description='Get remaining leaves'),
    Tool(name='ViewSalary', func=lambda x: view_salary('Bob'), description='Get salary history for Bob')
]
agent = initialize_agent(tools=tools, agent='zero-shot-react-description', llm=llm, verbose=True)
```


