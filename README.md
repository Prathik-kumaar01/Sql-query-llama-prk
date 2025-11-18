# Sql-query-llama-prk
from langchain_ollama import OllamaLLM import sqlite3  model = OllamaLLM(model="llama3")  DB_SCHEMA = """ TABLE employees(id, name, age, department, salary) TABLE attendance(emp_id, date, status
