# Day 4 Teaching Guide — HTTP, APIs & Chrome DevTools
## TDS Bridge Bootcamp

> **Who this is for:** Students who are comfortable with the terminal and Python projects (Days 1–3 done). Today they learn how the web actually works — and how to debug it.
> **Goal by end of session:** Student understands HTTP request/response, can debug any API call using DevTools or `curl`, and has a running FastAPI service they built and tested themselves.

---

## Section 1 — How the Web Actually Works (15 min)

### Every web interaction is a request and a response

When you open a website, your browser sends a **request** to a server, and the server sends back a **response**. That's it. Every API call, every form submission, every login — all of them follow this same pattern.

```
Browser/Client                          Server
     |                                    |
     |  --- HTTP Request  -------------> |
     |       Method: GET                 |
     |       URL: /api/users             |
     |       Headers: Accept: json       |
     |                                   |
     |  <-- HTTP Response --------------|
     |       Status: 200 OK             |
     |       Body: [{"id":1,...}]        |
```

### The 5 HTTP methods — what each one means

| Method | Meaning | Like saying… |
|--------|---------|--------------|
| `GET` | Fetch data, don't change anything | "Show me this" |
| `POST` | Send data to create something new | "Here's new data, save it" |
| `PUT` | Replace an existing resource entirely | "Replace this with what I'm sending" |
| `PATCH` | Update part of an existing resource | "Change just this field" |
| `DELETE` | Remove a resource | "Delete this" |

**Rule of thumb for beginners:** You'll use `GET` and `POST` for 90% of everything. `PUT`/`PATCH`/`DELETE` come in later when you build proper REST APIs.

### Anatomy of a URL

```
https://api.example.com/users/42?format=json&page=1
│       │               │       │
│       │               │       └── Query parameters (key=value pairs after ?)
│       │               └────────── Path (/users/42)
│       └────────────────────────── Host (domain)
└────────────────────────────────── Scheme (https)
```

**Query parameters** are how you send small pieces of data in a GET request. They appear after `?` and are separated by `&`. Example:

```
/search?q=python&page=2&sort=recent
```

---

**Section summary:** Every web interaction is a request + response. GET fetches, POST creates, PUT/PATCH update, DELETE removes. A URL has a scheme, host, path, and optional query parameters.

---

## Section 2 — Status Codes (10 min)

Status codes are three-digit numbers the server sends to tell you what happened. You must be able to read them instantly.

### The five families

| Range | Category | Think of it as… |
|-------|----------|-----------------|
| `1xx` | Informational | "Still working on it…" |
| `2xx` | Success | "Here you go ✅" |
| `3xx` | Redirect | "It moved, go here instead" |
| `4xx` | Client error | "You did something wrong" |
| `5xx` | Server error | "I (the server) broke" |

### The codes you'll actually see

| Code | Name | What happened |
|------|------|---------------|
| `200` | OK | Request succeeded, here's your data |
| `201` | Created | POST succeeded, new resource was created |
| `204` | No Content | Success, but nothing to return (e.g. after DELETE) |
| `400` | Bad Request | Your request was malformed (wrong JSON, missing field) |
| `401` | Unauthorized | You're not logged in / no valid token |
| `403` | Forbidden | You're logged in but don't have permission |
| `404` | Not Found | That URL/resource doesn't exist |
| `422` | Unprocessable Entity | Request structure is right but values are invalid |
| `429` | Too Many Requests | You're hitting the API too fast (rate limit) |
| `500` | Internal Server Error | The server crashed — the bug is on their side |

### 401 vs 403 — the one people always confuse

- **401:** The server doesn't know who you are. Send a token.
- **403:** The server knows who you are — you're just not allowed to do this.

### What to do when you see each

- `400` → Look at your request body. Something is wrong with the data you sent.
- `401` → Are you sending the `Authorization` header? Is your token correct?
- `403` → You're authenticated but hitting something you don't have access to.
- `404` → Check the URL. Did you typo the path? Does the resource exist?
- `429` → Slow down. Add a delay or check the API's rate limit docs.
- `500` → The server has a bug. Check if it's a known issue or try again later.

---

**Section summary:** 2xx = success, 4xx = you made a mistake, 5xx = server made a mistake. Know 200, 201, 400, 401, 403, 404, 429, 500 by heart. 401 means "who are you?", 403 means "I know you, but no."

---

## Section 3 — Headers (10 min)

### What are headers?

Headers are key-value pairs sent with every HTTP request and response. They carry metadata — not the main data, but information *about* the request or response.

**Common request headers:**

| Header | Purpose | Example |
|--------|---------|---------|
| `Content-Type` | What format am I sending? | `application/json` |
| `Accept` | What format do I want back? | `application/json` |
| `Authorization` | My credentials | `Bearer eyJhbGc...` |
| `User-Agent` | What client am I? | `Mozilla/5.0...` |

**Common response headers:**

| Header | Purpose | Example |
|--------|---------|---------|
| `Content-Type` | What format is this response? | `application/json` |
| `Content-Length` | How big is the response? | `1234` |
| `X-RateLimit-Remaining` | How many requests left? | `98` |

### Why headers matter in TDS

When calling an API (like OpenAI), you'll almost always need to set:

```
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

Missing either of these gives you a `401` or `400`. Learning to read headers in DevTools tells you exactly what the browser sends that you need to replicate in your code.

---

**Section summary:** Headers are metadata that travel with requests and responses. The two you'll set most often: `Authorization: Bearer <token>` and `Content-Type: application/json`.

---

## Section 4 — Chrome DevTools Network Tab (20 min)

### Why this skill matters

Every time your Python code calls an API and it fails, you waste time guessing. DevTools shows you exactly what was sent and received — headers, body, status code, timing. It's the difference between debugging blind and debugging with X-ray vision.

### Opening DevTools

- **Windows/Linux:** `F12` or `Ctrl+Shift+I`
- **macOS:** `Cmd+Option+I`
- Click the **Network** tab

### What you see

When you load a page or click a button, every request appears as a row in the Network tab.

```
Name          Status    Type      Size    Time
─────────────────────────────────────────────
api/users     200       fetch     4.2 kB  120ms
login         401       xhr       0.8 kB  43ms
/static/main  200       script    82 kB   210ms
```

### Inspecting a request — click any row

You'll see four sub-tabs:

- **Headers** — request and response headers
- **Payload** — the body you sent (for POST/PUT)
- **Preview** — the response formatted nicely
- **Response** — the raw response body

### Filter to only API calls

Click **Fetch/XHR** in the filter bar. This hides images, fonts, and scripts — showing only the data requests your page makes.

### Copy as cURL — the killer feature

Right-click any request → **Copy → Copy as cURL**

This gives you the exact `curl` command to reproduce that browser request in your terminal. This is how you:
- Test API calls without a browser
- Share reproducible examples with teammates
- Debug what changed between a working browser call and a failing script

> **Live demo:** Open any website (e.g. `jsonplaceholder.typicode.com`), go to Network, filter Fetch/XHR, copy a request as cURL, paste in terminal.

---

**Section summary:** DevTools Network tab shows every HTTP request your browser makes. Clicking a row reveals headers, payload, and response. "Copy as cURL" lets you replay any request in the terminal. Filter by Fetch/XHR to see only API calls.

---

## Section 5 — curl: Your API Swiss Army Knife (15 min)

`curl` is a command-line tool for making HTTP requests. You'll use it constantly to test APIs.

### Basic GET request

```bash
curl https://jsonplaceholder.typicode.com/posts/1
```

### GET with query parameters

```bash
curl "https://jsonplaceholder.typicode.com/posts?userId=1&_limit=3"
```

> Always quote URLs with `&` in them — the shell treats `&` as "run in background".

### POST with a JSON body

```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "my post", "body": "hello world", "userId": 1}'
```

Breaking it down:
- `-X POST` — use the POST method
- `-H "..."` — add a header
- `-d '...'` — the request body (data)

### Adding an Authorization header

```bash
curl https://api.example.com/data \
  -H "Authorization: Bearer your-token-here" \
  -H "Accept: application/json"
```

### Pretty-print JSON responses

`curl` output is raw. Pipe through `python3 -m json.tool` to format it:

```bash
curl https://jsonplaceholder.typicode.com/posts/1 | python3 -m json.tool
```

Or install `jq` (a dedicated JSON processor):

```bash
sudo apt install jq -y
curl https://jsonplaceholder.typicode.com/posts/1 | jq .
```

### See the full request and response headers

```bash
curl -v https://jsonplaceholder.typicode.com/posts/1
```

`-v` (verbose) shows exactly what was sent and received — very useful for debugging auth issues.

### Useful curl flags summary

| Flag | Meaning |
|------|---------|
| `-X POST` | Set HTTP method |
| `-H "Key: Value"` | Add a header |
| `-d '{"key":"val"}'` | Request body |
| `-v` | Verbose — show full request/response |
| `-s` | Silent — suppress progress bar |
| `-o file.json` | Save response to a file |

---

**Section summary:** `curl` makes HTTP requests from the terminal. `-X` sets method, `-H` adds headers, `-d` sends a body. Pipe to `jq` for readable JSON. Use `-v` to debug auth issues.

---

## Section 6 — Building a FastAPI Service (30 min)

### What is FastAPI?

FastAPI is a Python framework for building APIs. You define Python functions and decorate them with `@app.get(...)` or `@app.post(...)` — FastAPI handles the HTTP layer for you.

It also auto-generates a **Swagger UI** at `/docs` where you can test your endpoints visually in the browser.

### Setup

```bash
cd ~/tds/bootcamp
uv init day4-api
cd day4-api
uv add fastapi "uvicorn[standard]"
```

### Your first FastAPI app

Create `app.py`:

```python
from fastapi import FastAPI
from datetime import datetime, timezone

app = FastAPI()


@app.get("/health")
def health():
    return {"status": "ok"}


@app.post("/echo")
def echo(payload: dict):
    return {
        "received": payload,
        "timestamp": datetime.now(timezone.utc).isoformat()
    }
```

### Run the server

```bash
uv run uvicorn app:app --reload
```

- `app:app` — the file is `app.py`, the FastAPI object inside it is also named `app`
- `--reload` — automatically restarts when you save the file

You'll see:
```
INFO:     Uvicorn running on http://127.0.0.1:8000
```

### Test it

**In the browser:** Open `http://localhost:8000/docs` — you get a full interactive Swagger UI.

**With curl:**

```bash
# Test GET /health
curl http://localhost:8000/health

# Test POST /echo
curl -X POST http://localhost:8000/echo \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "score": 95}'
```

**Expected responses:**

```json
{"status": "ok"}
```

```json
{
  "received": {"name": "Alice", "score": 95},
  "timestamp": "2026-06-05T15:30:00+00:00"
}
```

### Inspect it in DevTools

Open `http://localhost:8000/docs` in Chrome, use the UI to make a request, then open DevTools → Network tab → click the request. You'll see the request your browser made to *your own server*.

---

**Section summary:** FastAPI lets you define HTTP endpoints as Python functions. `uvicorn` is the server that runs it. `/docs` gives you a free interactive UI. Test endpoints with `curl` or the browser — both are valid.

---

## Live Exercise — Do Together (10 min)

```bash
# 1. Go to your day4-api project
cd ~/tds/bootcamp/day4-api

# 2. The server should already be running. If not:
uv run uvicorn app:app --reload

# 3. Open a second terminal and test:
curl http://localhost:8000/health
curl -X POST http://localhost:8000/echo \
  -H "Content-Type: application/json" \
  -d '{"message": "hello from curl"}'

# 4. Open http://localhost:8000/docs in the browser
# 5. Make a POST /echo request from the Swagger UI
# 6. Open DevTools → Network → inspect that request
# 7. Right-click the request → Copy → Copy as cURL
# 8. Paste that cURL command in the terminal and run it
```

**Discuss:** What does the copied cURL command look like? How is it different from what you typed manually?

---

**End of Day 4**

> Remind students: fill in the post-session checklist and push `day4.md` + `app.py` to their GitHub repo.
