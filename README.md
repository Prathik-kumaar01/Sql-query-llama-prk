from langchain_ollama import OllamaLLM
import sqlite3

model = OllamaLLM(model="llama3")

DB_SCHEMA = """
TABLE employees(id, name, age, department, salary)
TABLE attendance(emp_id, date, status)
"""

def nl_to_sql(query):
    prompt = f"""
    Convert the user request into SQL based on this schema:
    {DB_SCHEMA}

    User Query: "{query}"
    Return only SQL.
    """
    return model(prompt)

def execute_sql(sql):
    conn = sqlite3.connect("company.db")
    cursor = conn.cursor()
    cursor.execute(sql)
    rows = cursor.fetchall()
    conn.close()
    return rows

def explain_results(results, original_query):
    prompt = f"""
    User asked: {original_query}
    Database returned:
    {results}

    Explain the answer clearly.
    """
    return model(prompt)

def pipeline(user_query):
    sql = nl_to_sql(user_query)
    rows = execute_sql(sql)
    final = explain_results(rows, user_query)
    return final

print(pipeline("Show the list of employees older than 50"))
