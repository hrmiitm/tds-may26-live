
### Module 4: API & FastAPI Foundations

# 🔌 Connecting the Web: APIs & FastAPI Foundations

**Welcome to your API foundational refresher!** APIs sound highly technical, but they are simply digital messengers. They form the backbone of the modern internet, allowing different software applications to communicate securely.

🎬 **Watch This First:** [What is an API? & FastAPI basics (YouTube)](https://www.google.com/search?q=https://www.youtube.com/results%3Fsearch_query%3DWhat%2Bis%2Ban%2BAPI%2Band%2BFastAPI%2Bbasics)
📖 **Read More:** [FastAPI Official First Steps](https://fastapi.tiangolo.com/tutorial/first-steps/)

### 🌱 The Absolute Basics: What is an API?

API stands for Application Programming Interface[cite: 2]. It is the bridge that allows a frontend application (like a mobile app) to talk to a backend database over the internet[cite: 2].

When these applications talk, they need a common language. They usually send data back and forth formatted as **JSON** (JavaScript Object Notation), which looks exactly like a standard Python dictionary: `{"name": "John", "age": 30}`[cite: 2].

### ✅ Module Checklist

* [ ] Grasp the Client-Server architecture[cite: 2].
* [ ] Understand standard JSON formatting[cite: 2].
* [ ] Know the difference between primary HTTP Methods (`GET`, `POST`, `PUT`, `DELETE`)[cite: 2].
* [ ] Understand HTTP Status Codes (200, 404, 500).
* [ ] Build a minimal API using Python's FastAPI[cite: 2].
* [ ] Test endpoints using `curl` and the browser[cite: 2].

### 🧠 The Core Concept: Client, Server, and HTTP

**Think of a Restaurant:**

1. **You (The Client):** You sit at the table and want to view the menu or order a burger[cite: 2].
2. **The Waiter (The API):** You tell the waiter your order. The waiter securely carries the message to the kitchen[cite: 2].
3. **The Kitchen (The Server):** They do the heavy lifting, cook the burger, and give it to the waiter to return to you[cite: 2].

**HTTP Methods:**
When sending these messages, the web uses specific HTTP verbs[cite: 2]:

* `GET`: "Give me data" (Reading the menu)[cite: 2].
* `POST`: "Here is new data, save it" (Placing a new order)[cite: 2].
* `PUT`/`PATCH`: "Update this existing data" (Changing your order)[cite: 2].
* `DELETE`: "Remove this data" (Canceling your order)[cite: 2].

### 🏋️ Practice Exercises & Solutions

**Exercise 1: The Kitchen Setup**
Initialize a new `uv` project, add `fastapi` and `uvicorn`, and write a basic script (`main.py`) with a single `GET` route at `/health` that returns `{"status": "API is running perfectly!"}`.

**Exercise 2: Serve the API**
Start your FastAPI server so it continuously listens for incoming requests on your local machine port 8000.

**Exercise 3: The Client Request**
Leave the server running in your first terminal. Open a brand **NEW** terminal window, and use the `curl` command to act as the client and "order" data from your API.

```bash
# Solution 1: Environment Setup
uv init api_demo
cd api_demo
uv add fastapi uvicorn standard

# main.py code:
cat << 'EOF' > main.py
from fastapi import FastAPI
app = FastAPI()

@app.get("/health")
def read_health():
    return {"status": "API is running perfectly!"}
EOF

# Solution 2: Run the server in terminal 1
uv run fastapi dev main.py

# Solution 3: In a NEW terminal, test the endpoint
curl http://127.0.0.1:8000/health

```

### 🤔 Conceptual Questions

1. When you type `google.com` into your browser and hit Enter, what specific HTTP method is your browser executing under the hood?
2. If your API returns a `404` status code, what does that indicate to the client? What about a `500` status code?

---